+++
date = "2017-05-09T17:08:35-05:00"
draft = false
highlight = true
math = false
tags = ["rpg"]
title = "Recording the audio from online calls on Ubuntu"

[header]
  caption = ""
  image = ""

+++

I've recently started playing role-playing games online over audio/video chat using platforms like Google Hangouts or [appear.in](https://appear.in/). Inspired by a [recent Gnome Stew post](http://www.gnomestew.com/game-mastering/gming-advice/the-listen-back/), I've wanted to start recording those sessions so I can listen to them again, share them, and get better at GMing.

I have yet to record any RPG sessions using this set up, but if everything goes as planned I should be able to record my next [Ryuutama](http://kotohi.com/ryuutama/) game!

I found this approach after reading a few different tutorials until I found [one that worked and made sense](https://sputniza.wordpress.com/2015/01/29/how-to-record-audio-from-a-google-hangout-on-ubuntu/). I've got to write this kind of procedure down somewhere or I'll forget it, so hopefully this post will help me next time I have to record something.

- install **pavucontrol** using apt.
- install **audacity** using apt.
- using Audacity's Device Toolbar, set the host to **ALSA**, output to
  **pulse**, and input to sound card or whatever (probably the default).
- currently, this setup is recording everything that comes through the output. What is missing is your voice, which now needs to be looped back through your speakers/output.
- open a terminal and type `pactl load-module module-loopback latency_msec=1` to
  loop your own voice back through output.
- start call using whatever platform you choose.
- click record in audacity.
- open pavucontrol, go to **Recording** tab, select **Monitor of Built in Analog
  Stereo**.
- when done recording, type `pactl unload-module module-loopback` to stop your voice
  looping back through the speakers.
