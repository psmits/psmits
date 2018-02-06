+++
date = "2017-05-10T13:24:42-05:00"
draft = false
highlight = true
math = true
tags = ["academic", "stan"]
title = "An attempt at a basic capture-mark-recapture model in Stan"

[header]
  caption = ""
  image = ""

+++
Capture-mark-recapture (CMR) models come up in ecology and paleobiology when attempting to estimate species richness, birth rates, death rates, and sampling rates from occurrence history data. I use [Stan](http://mc-stan.org/) to write all my statistical models, and I found implementing a CMR model a bit of a bear because Stan's gradient-based samplers cannot estimate discrete parameters. Instead, as described in the Stan manual, I needed to marginalize over all possible occurrence histories for each subject. Lucky for me, there are only two possible states (0 and 1) at each time point and they follow relatively simple rules. Sadly, the number of potential occurrence histories can grow dramatically as the number of subjects and time points increase. Here's my solution as implemented in Stan. Oh, and in case it wasn't obvious, this is a Bayesian model.

I'm going to focus on the Jolly-Seber (JS) model as presented in [Royle and Dorazio](https://www.amazon.com/Hierarchical-Modeling-Inference-Ecology-Metapopulations/dp/0123740975). At its core, this model is a discrete-time hidden Markov model with an absorbing state ("death"). 

The type of data suited for a JS model is where multiple subjects (e.g. species, individuals) are sampled at multiple discrete times (e.g. days, geologic stages). If a subject is observed, it gets a 1 for that time point; if the subject is not observed at that time point, it gets a 0. 

The recorded absences are very important for filling in the missing "true" occurrence histories. While all non-observations are recorded as a 0, species are expected to range-through all time points between their first and last occurrences (i.e. observation history 101 implies occurrence history of 111). Additionally, as is certainly the case with paleontological data, the **observed** first occurrence is not necessarily the **true** first occurrence; same for last occurrence. Another important detail is that once a subject leaves the system, they've left it permanently (i.e. extinction, death). 

The JS model over comes this by estimating the probability of observing a subject if it is present. The JS model expressed hierarchically involves two statements, one for the probability of observing a subject and one for the probability that the subject is present. In the first statement, $y$ is the matrix of observations, $p$ is the probability of observing a subject if present, and $z$ is the "true" occurrence matrix.

$$y_{i,t} \sim \text{Bernoulli}(p z\_{i,t})$$

The second statement is effectively a prior on $z$, and it is more involved because the birth and death probabilities need to be included. Also, because death is an absorbing state I need to insure that the probability of a subject returning after death is 0. Here $\phi$ is the probability of a subject surviving from one time point to the next and $\lambda$ is the probability of subject originating at time $t$. While these models are called birth-death, we are actually estimating birth and survival. The product term in the equation ensures no "zombies" or de-extinctions. 

$$z_{i,t} \sim \text{Bernoulli}\left(\phi z\_{i, t - 1} + \lambda \left(\prod\_{n=1}^{t}(1 - z\_{i, n})\right)\right)$$

Priors then need to be given for $p$, $phi$, and $\lambda$. Covariates can be
incorporated into these priors as you would with a logistic regression.[^1]

[^1]: see [BDA3](https://www.amazon.com/Bayesian-Analysis-Chapman-Statistical-Science/dp/1439840954/), [ARM](https://www.amazon.com/Analysis-Regression-Multilevel-Hierarchical-Models/dp/052168689X/), or [Statistical Rethinking](https://www.amazon.com/Statistical-Rethinking-Bayesian-Examples-Chapman/dp/1482253445/).

As you might be able to guess, the big issue is the matrix $z$ of which almost every value must be estimated. To implement the above in Stan, I need to write a function to marginalize over all possible values of $z$. Using that function I can then estimate the log posterior. There are other helped functions to smooth everything along; you might recognize them from the Stan manual. I owe a lot of thanks to [Bob Carpenter](http://bob-carpenter.github.io/) and the extremely helpful [Stan mailing list](https://groups.google.com/forum/?fromgroups#!forum/stan-users) for helping me figure out this solution!

```stan
functions {
  int first_capture(int[] y_i) {
    for (k in 1:size(y_i))
      if (y_i[k])
        return k;
    return 0;
  }
  int last_capture(int[] y_i) {
    for (k_rev in 0:(size(y_i) - 1)) {
      int k;
      k = size(y_i) - k_rev;
      if (y_i[k])
        return k;
    }
    return 0;
  }
  real state_space_lp(int[] y, real origin, real stay, real p) {
    int ft;
    int lt;
    int S;
    int i;
    int prod_term;
    int lp_size;

    S = size(y);
    i = 1;
    ft = first_capture(y);
    lt = last_capture(y);
    lp_size = ft * (S - lt + 1); // how many possible combinations

    // have to go through all possible event histories for each species
    {
      // vector of log probabilities for all possible valid z-s
      vector[lp_size] lp;  

      for(t_first_alive in 1:ft) {
        for (t_last_alive in lt:S) {
          real sl;
          int z[S];

          for(j in 1:S) {
            z[j] = 0;
          }
          // fill in the range through knowledge
          for(a in t_first_alive:t_last_alive) {
            z[a] = 1;
          }
          // notice how z is never actually estimated 
          // instead, log probability of every possible history is calculated

          // first time point only allows birth
          sl = bernoulli_lpmf(z[1] | origin);
          prod_term = 1 - z[1];

          {
            vector[S - 1] gg;
            for(j in 2:S) {
              prod_term = prod_term * (1 - z[j - 1]); // calculate if extinct
              gg[j - 1] = bernoulli_lpmf(z[j] | (z[j - 1] * stay) + 
                  prod_term * origin);
            }
            sl = sl + sum(gg);
          }

          // finally, y as function of z and p
          {
            vector[S] hh;
            for(k in 1:S) {
              hh[k] = bernoulli_lpmf(y[k] | z[k] * p);
            }
            sl = sl + sum(hh);
          }

          lp[i] = sl;
          i = i + 1;
        }
      }
      return log_sum_exp(lp);
    }
  }
}
data {
  int N;  // number of species
  int T;  // number of temporal units
  int sight[N, T];  // observed presences
}
parameters {
  real<lower=0,upper=1> origin; // birth
  real<lower=0,upper=1> stay; // survival
  real<lower=0,upper=1> p;
}
model {
  // priors are kinda arbitrary but not flat
  origin ~ beta(2, 2);
  stay ~ beta(2, 2);
  p ~ beta(2, 2);
  
  // the typing of the function doesn't allow for vectorizing, but ok
  for(n in 1:N) {
    target += state_space_lp(sight[n], origin, stay, p);
  }
}
```
