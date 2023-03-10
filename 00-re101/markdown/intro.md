<!-- .slide: data-background-gradient="radial-gradient(#283b95, #17b2c3)" -->

# RE101

Reverse It, Hack It, Learn It

---

## What I cannot create, I do not understand

(c) Richard Feynman

Note: That's why today we will learn how to first create things. We will start with an assembly and C crash course, then we will learn how to use tools to reverse engineer binaries. And finally, we will do some hands-on exercises.

---

## WHOAMI

* ๐ฑโ๐ป Theodor-Arsenij Larionov
* ๐ Ex Kaspersky, Ex Postgres, Now ISP RAS
* ๐ป Reverse Engineer
* ๐ฆพ Security Researcher
* ๐งช Low-level programmer
* ๐ฉ CTF player

---

## Links

<!-- Bruh, I'm not a designer -->
<div id="social-media" style="display: flex; flex-direction: column; align-items: left; position: absolute; top: 105%; right: 37%">
 <div style="display: flex; align-items: center; margin-bottom: 5px;">
  <img src="assets/logos/lightning.png" style="height: 1.5em; width: 1.5em; margin-right: 32px;"
   alt="Pwn-Report" />
  <span style="vertical-align: middle;"><a href="https://pwn.report">pwn.report</a></span>
 </div>
 <div style="display: flex; align-items: center; margin-bottom: 5px;">
  <img src="assets/logos/telegram.png" style="height: 1.5em; width: 1.5em; margin-right: 32px;"
   alt="Telegram" text="pwn_report" />
  <span style="vertical-align: middle;"><a href="https://t.me/pwn_report">pwn_report</a></span>
 </div>
 <div style="display: flex; align-items: center; margin-bottom: 5px;">
  <img src="assets/logos/twitter.png" style="height: 1.5em; width: 1.5em; margin-right: 32px;" alt="Twitter" />
  <span style="vertical-align: middle;"><a href="https://twitter.com/m4drat">m4drat</span>
 </div>
 <div style="display: flex; align-items: center; margin-bottom: 5px;">
  <img src="assets/logos/github.png" style="height: 1.5em; width: 1.5em; margin-right: 32px;" alt="GitHub"/>
  <span style="vertical-align: middle;"><a href="https://github.com/m4drat/">m4drat</span>
 </div>
</div>

---

## Agenda

* Why RE?
* Compiled VS Interpreted
* Computer architecture crash course
  * Intro to C and assembly
  * Memory
  * Function calls
  * System calls
* Tools
* Hands-on exercise
* Resources
* What's next?

Note:

Memory - stack, heap, data, bss, text, etc + dynamic linker

---

<!-- .slide: data-background-gradient="radial-gradient(#C36B6B, #A12929)" -->

## Disclaimer

* All materials are for educational purposes only
* Do not hack what you do not allowed to hack!

---

## Why RE?

* ๐งช Understand how things work
* ๐ฅ Vulnerability research
* ๐ค Malware analysis
* ๐จโ๐ป Low-level programming
* ๐ฉ CTF
* ๐คก Fun

Note:

1. Low-level programming - cheats/anti-cheats, game mods, etc.
2. Who wouldn't like to find a way how to get unlimited money in your favorite offline game? :)  
