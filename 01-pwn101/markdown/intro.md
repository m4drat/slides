<!-- .slide: data-background-gradient="radial-gradient(#393a65, #242424)" -->

# PWN101

---

## WHOAMI

* 🐱‍💻 Theodor-Arsenij Larionov
* 📚 Ex Kaspersky, Ex Postgres, Now ISP RAS
* 💻 Reverse Engineer
* 🦾 Security Researcher
* 🧪 Low-level programmer
* 🚩 CTF player

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

* Why PWN?
* Vulnerability types overview
  * Buffer overflow
  * Format string
  * Heap bugs
    * Use-after-free
* Mitigations
* Exploitation techniques
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

## Why PWN?

* 💥 Vulnerability research
* 🤖 0-day analysis
* 👨‍💻 Low-level programming
* 🚩 CTF
* 🤡 Fun

Note:

1. Low-level programming - cheats/anti-cheats, game mods, etc.
2. Who wouldn't like to find a way how to get unlimited money in your favorite offline game? :)  

---

## Current state of affairs

<div class="r-stack">

<img height="500px" class="fragment fade-in-then-out" data-fragment-index="1" src="assets/materials/memory-safety-vulnerabilities-by-year.png">
<img height="500px" class="fragment fade-in-then-out" data-fragment-index="2" src="assets/materials/amount-of-new-code.png">
<img height="500px" class="fragment fade-in-then-out" data-fragment-index="3" src="assets/materials/new-native-code.png">

</div>

Memory Safe Languages in Android 13: <!-- .element: style="font-size: 20px" --> [google-blog](https://security.googleblog.com/2022/12/memory-safe-languages-in-android-13.html) <!-- .element: style="font-size: 20px" -->

---

## 🦀 Rust is not a 100% solution

Note: Soundness bugs

---

## Current state of affairs

At some point, memory safety issues will (almost) go extinct <!-- .element: class="fragment" -->

But who knows when it will happen  <!-- .element: class="fragment" -->

Note:

1. We're living in a much more secure world, and it's going to improve
2. The base level will surge (and it's already)
3. Some well-known apple researchers are leaving the industry because complexity level is enormous
