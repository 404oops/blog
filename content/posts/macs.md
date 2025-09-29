+++
date = "2025-08-19T19:49:51+02:00"
title = "Why I use macOS (as well as the rest of the Apple ecosystem)"
categories = [ "computers" ]
tags = [ "configuration", "opinion" ]
slug = "mac-apple-ecosystem"
+++

Hi y'all!

There's a lot of things that I should clarify before people start assuming stuff. I respect your tech choice no matter if you respect mine or not. I'm happy with my tech for the reasons I'll state and nothing can sway me from drifting off what I currently use

With that being said:

### Here's my life story and experience with Laptops!

When I was a child I used to have a laptop (HP Pavilion dv6500) that was gifted to me by a greek because of my dad's efforts to fix it going futile. Someone bought a replacement laptop for the laptop my dad was fixing and my dad got it gifted because it "felt right" by the owner. Years went by and I received the laptop that was essentially collecting dust

My sister OPPOSED the idea of me receiving my laptop, but since I was the family tech whiz I received it and the first OS it ran was Windows 8.1 when I received it. I did a LOT on it, played TF2, browsed the web, had a hell of a time on it, but there was one fatal flaw with it: it was SLOW as BALLS.

TF2 lagged like hell, it had a Core 2 Duo and a GeForce 8400M if I recall correctly. It was HORRIBLE but it was fun. It gets worse though, just as you thought it wouldn't get worse. The charger cable kept melting on the barrel jack end. It got so dire that I had to quit using it. And then I received a laptop that would forever change my perspective on laptops

_Before I go over to my next piece of hardware, this laptop briefly ran Hackintosh OS X Yosemite. It felt nicer to use, but I felt handicapped since this was BEFORE OpenCore was a thing. Clover couldn't do iMessage or any proper iServices back then, so it wasn't really worth it to run macOS on it, so I switched to Windows 10._

I received a MacBook (Late 2006). Not a pro, just a hunk of plastic with debatably worse specs than the Pavilion since it lacked a discrete GPU. The MacBook was released in 2006, and the Pavilion was released in 2007. One year apart, probably even months, and the Mac managed to "outperform" the Pavilion.

I did get it way too late for any games to actually run on it, but I primarily used it for schoolwork. It felt stabler, faster and despite all differences and the fact that it ran OS X Lion back when the whole phase of skeuomorphic design in user experiences, it legitimately felt like I could do more, when realistically I could do less. Then I started producing on it.

Running virtual machines was no hassle on VirtualBox back when it was, you know, _Usable_. I installed FL Studio on it and began making what current me would call slop but it was something that my whole class bumped and praised so I just went with it. I ran Windows XP because, well, _Core 2 Duo again_, but the OS didn't feel like it was taking any hits.

Playing doom through a source port felt like a breeze, since that was all I could legitimately play anyways. And then I returned to the Pavilion (for 1 week) to ACTUALLY do schoolwork, since my Mac was GENUINELY nearing its End-Of-Life. It's like if you keep someone braindead on life support for no reason.

Then my dad bought me a MacBook Pro. Mid 2010, ran patched macOS since the UEFI wasn't 32-bit, and it felt great to use. Ran Catalina back when, you know, Catalina was a thing and it was awesome. I could actually run a DAW now! But it felt slow because the hardware requirements, to no one's surprise, actually meant that previous hardware would choke.

Then I got a 2013 MacBook that was handed to me, used by my dad, then mom, and I had a LOT of fun on it. It was versatile. I ran Linux, Windows and macOS on it, and all but Windows felt great. Windows required a LOT of mods to even detect my sound card which sucked but whatever and when I used it, it felt a bit... odd. But whatever.

Then 2022 came and I earned my own MacBook Pro (2020). The M1. The Beast. Suddenly I could do ANYTHING I wanted on my laptop. But the thing is, when I got ahold of it, I felt spoiled with the power. Suddenly everything else started feeling slow.

### But 404, what about PCs?

Welp, I didn't miss that. At the gap in between the M1 Mac and the 2013 Mac, I forgot the charger somewhere in Kosovo. It was embarrassing to find out, but whatever, so I took an Intel NUC (specifically NUC8i5BEK) and used it as my main machine. It ran EVERYTHING, but the problem is, Windows felt... unstabler.

I HAD to use Windows because using Desktop linux felt like a CHORE in my field of work, especially during my schoolwork. And Windows still felt like it didn't belong to me.

### Now, onto the reasons why I prefer Macs

When I used Windows, it felt forced. Everything felt like it was convoluted. Different methods to achieve one thing, all of which suck both internally and in hindsight. Production felt like patchwork, Nothing was native and everything kept breaking. It was HELL.

Everything felt like I needed to install yet another piece of software that will achieve one thing. Nothing was concise, I feared malware could be installed on my PC, and it was very much more prone to freezing.

Even Intel Macs felt stabler.

Now if I wanted to get into specifics let's divide it:

### Hardware:

On Macs:

- Hardware is concise. You need to worry about your Chip, RAM and Storage, and nothing more unless you're a nerd who wants the most out of your hardware
- Since the hardware's concise and curated by Apple, Apple can tune its software and drivers to best perform on that piece of hardware. The reason why screens have a notch is because they want to give you the most vertical clearance while sacrificing the least (which is the Menu Bar).
- Hardware on Macs nowadays run on ARM processors which, if you haven't been following the news, require LESS instructions to do a thing that many x86 CPUs need. They run on your phone even. They draw less power, making them the best portable machine, and run programs much more efficiently than x86 CPUs.
  - Notice how I said efficiently and not faster? That's because they're not supercomputers. They just take less to do more. And that's why they're beautiful

### Software:

- The reason why Apple gets so much usage is because their software is deeply tied to the hardware you use, which is why Everything Works On Macs. Video composition and editing (even Blender) can be done through the Metal renderer (By Apple), and all other APIs are utilized because they're the best in class for the hardware.
- For Music Production, not only is ARM the best, the fact that it's RISC makes sense for production. Music is _**REAL TIME COMPUTING**_ and requires all the shortcuts you could give it, even if the software sounds simple. That's why many audio devices, like digital mixers, run Cortex SoCs. It makes sense to have something that takes shortcuts, and when you pair that up with the fact that the software APIs are the best in class, you get the best experience
- Speaking of APIs and Music, Core Audio and Core MIDI have proven themselves to be the best option to go to. Windows has no good audio backend. Virtually no headroom, always ducks sounds to prioritize system sounds (with a REALLY HIGH RESPONSE TIME), and the worst of all **HIGH ROUNDTRIP LATENCY**. I can also go on speaker assignments (nonexistent) and the fact that you need Drivers for Every. Professional. Device. Drivers on Mac are only required if the device is really meticulous and complicated and needs special communication, like Universal Audio, but for a chickenshit device like a Komplete Audio 1 it shouldn't and it doesn't on Mac. ASIO was made by Steinberg to mitigate the shitty audio backends you could use to give Windows audio, and it taps DIRECTLY into the Device. That's how shit Windows' backends are.

### Conclusion

It's a matter of preference, but in my line of work it has proven to be:

- Stable
- Faster
- Robust

and interoperable. Speaking of which, the Apple ecosystem's interoperability is something I've never seen anywhere else. You could argue that you can replicate functionality, but why download a jillion apps just to do half of what Apple's tech does just so you can say "I can do that too!"? It feels wrong, and it just doesn't sit right with me. I depend on iCloud Drive for sample storage, photos, app storage, because everything sits there and I don't have to worry about potentially losing stuff. I can answer calls, text people, interact with my iPhone, use my iPad as a second display, all of which require separate apps with questionable support on all other platform combinations.

It just feels better to use.
