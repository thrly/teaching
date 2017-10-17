###### MUSS3640 Music Technology / MUSS5632M Electronic Computer Music
# Programming (Interactive) Music
last edited: 2 October 2017
[o.thurley@leeds.ac.uk](mailto:o.thurley@leeds.ac.uk)
### Summary:
Using computer programming languages to create interactive music systems, and some reasons why you might want to...
💻🚀✨🙉
***

## Table of Contents
- [0. Why program music?](#0-why-program-music?)
- [1. What can Max do?](#1-what-can-max-do)
- [2. What can SuperCollider do?](#2-what-can-supercollider-do)
- [3. What can Microcontrollers do?](#3-what-can-microcontrollers-do)
- [Further Reading](#further-reading)
- [Resources](#resources)

***

## Introduction
As we look at a few simple examples & ideas, think about how *you* could develop these technologies in your own project. Think about how these concepts could be developed into larger scale projects, and the larger ramifications of this.

Be ready to discuss and ask questions as we go through.
# 0. Why program music?
#### Interaction as a *Conversation*
A lecture/soliloquy is not interactive, it is a one-way transaction of information (me -> you). For interaction to take place, a sort of **feedback loop** needs to occur (me <--♻️--> you). (It is worth noting that this isn't necessarily a closed feedback loop that you might be used to in a 'squawking' microphone+speaker system.)

For Joshua Noble, the feedback loop is the
> process of an entity communicating with itself while checking with either an internal or external regulatory system'

~ Joshua Noble, *Programming Interactivity*, 3

Sergi Jordà holds that
>'Interaction' involves the existence of a mutual or reciprocal action or influence between two or more systems.

~ Sergi Jordà, 'Interactivity and live computer music' in *The Cambridge Companion to Electronic Music*, eds. Collins & d'Escrivan, 90

##### Computer Music is still _Music_
>'Musicians interact with their instrument, with other musicians, with dancers or with the audience...'
~ Jordà, ibid.

Jordà goes on to say that in recorded music, music seemed to lose some of its interactivity: music became fixed once it was captured on recording. Now, with the aide of advanced computer systems, music can return to its former interactive nature.

Todd Winkler gives an example of possible components of an interactive music system:
>1. Human input, instruments: Human activity is translated into digital information and sent to the computer
>2. Computer listening, performance analysis: The computer receives the human input and analyses the performance information for timing pitch, dynamics, or other musical characteristics.
>3. Interpretation: the software interprets the computer listener information, generating data that will influence the composition.
>4. Computer composition: computer processes, responsible for all aspects of the computer generated music, are based on the results of the computer's interpretation of the performance.
>5. Sound generation and output, performance: The computer plays the music, using sounds created internally, or by sending musical information to devices that generate sound.

~ Todd Winkler, '*Composing Interactive Music*', p.6

This is a pretty simplistic view of the possibilities of interaction in music, but it provides a starting point for us to develop upon.

#### Laurie Spiegel's *Music Mouse*
In 1985, composer and programmer [Laurie Spiegel](https://unseenworlds.bandcamp.com/album/the-expanding-universe) released [*Music Mouse*](http://retiary.org/ls/programs.html), an 'intelligent instrument' which turned the computer in an instrument anyone could play by moving the mouse across an onscreen grid.

![Music Mouse](http://retiary.org/ls/gifs_ls/mm_screen.gif)

[Laurie Spiegel's _The Expanding Universe_](https://unseenworlds.bandcamp.com/album/the-expanding-universe)

The software generates music by manipulating its own subroutines based on the simple *interaction* of the user. *Music Mouse* isn't the 'birth' of interactive music (not by a long way), but it is a nice example of how we can think about programming interactivity in music.

***
## Read the ß∆$†∆®& manual
Before we start, it doesn't matter if you don't have much programming experience, or if you haven't used (or heard of) the technologies we discuss today. There is a solution...

**The HELP FILE** 📋

All of the following technologies come with excellent documentation, support networks, help-files, forums and easy to follow tutorials that can guide you from the basics of the technology up to very advanced techniques.

*Even if you're a seasoned pro, the help-file is still your friend.* _**DO NOT FEAR THE HELP-FILE.**_

***
# 1. What can Max do?
[Max](https://cycling74.com) (fka 'Max/MSP + Jitter') is a ubiquitous, patchable 'visual' programming language. It can do more things than is practical to mention here, but some of the things it does BEST are:
- MIDI data (Max)
- Audio (MSP)
- Visuals (Jitter, OpenGL, etc.)
- Interfacing with other tech (cv synthesisers, controllers, dmx lighting, Arduino, projectors, etc.)
- Live inputs.

Max's visual architecture allows a very different workflow and method of thinking from more traditional text-based programming languages. This has advantages and disadvantages (we can think linearly and non-linearly, but sometimes dealing with messy wires is more confusing than neatly blocked script).

Max has _**truly excellent tutorials, help and support systems**_ which make it a pretty friendly tool to use.

#### Some interesting musical projects using Max

[![Rodrigo Constanzo's Cut Glove](https://img.youtube.com/vi/GA3rSCHklMQ/0.jpg)](https://www.youtube.com/watch?v=GA3rSCHklMQ)

VIDEO: [Rodrigo Constanzo's Cut Glove](https://www.youtube.com/watch?v=GA3rSCHklMQ)

--NO IMAGE--
VIDEO: [MONOME @ MOMA ~ Brian Crabtree and Kelli Cain's Monome controlling Max instruments](https://vimeo.com/32331721)

[Sarah Angliss](http://www.sarahangliss.com/therobots) uses Max + Arduinos (w/ servo motors) to 'perform' with her custom built robots:
![Angliss's Robot Drummer](http://www.sarahangliss.com/wp-content/uploads/2013/02/Wolfgang-600.jpg)

Some other ideas for projects [Made with Max.](https://cycling74.com/made-with-max/)

##### Max for Live
Max patches can be written, edited and run inside Ableton Live allowing the powerful flexibiility of Max to be used as part of the signal/MIDI/processing path of Ableton. Think of it like an incredibly powerful and open Audio Unit or VST

##### GEN
Max's GEN language allows us to control data right down to the sample level: much closer than previous and affording some *very* powerful audio manipulation.

GEN also allows you to export its code as iOS, AU and VST plugins to use in other systems (although this aspect is a _little_ underdeveloped).

#### Living off a single-board computer
With all its bells and whistles, Max is a pretty "heavy" environment, so if you wanted to run Max off a smaller single-board computer system (like a Raspberry Pi or BeagleBoard), you'd be better off looking at something like [Pure Data](https://puredata.info) which can run in its "Vanilla" mode on the Pi... there's some helpful info about this [on the Pd site.](https://puredata.info/docs/raspberry-pi)

(See also: [Pure Data](https://puredata.info), [Reaktor](https://www.native-instruments.com/en/products/komplete/synths/reaktor-6/), [Axoloti](http://www.axoloti.com), [vvvv](https://vvvv.org))

***************************************************************************
************************************************************************
# 2. What can SuperCollider do?
**SuperCollider** (1996/2002) is a powerful text-based audio programming language. It's creator, James McCartney, describes it as 'a language for describing sound processes.’

SuperCollider,
> allows one to represent musical concepts as objects, to transform them via functions or methods, to compose transformations into higher-level building blocks, and to design interactions for manipulating music in real-time, from the top level structure of the piece down to the level of the waveform.

~ James McCartney, 'Foreword' in *[The SuperCollider Book](https://www.amazon.co.uk/SuperCollider-Book-Scott-Wilson/dp/0262232693)*, eds. Wilson, Cottle, Collins (MIT, 2011)

We can use SuperCollider to create:
-   real-time interactions
-   installations
-   generative music
-   electroacoustic music
-   audiovisuals
-   live-coding
-   networked performances
-   often open source and thus open-ended…


### Live Coding
--NO IMAGE--

> VIDEO: Andrew Sorensen emulating Keith Jarrett's [Sun Bear Concerts](https://www.youtube.com/watch?v=e7T6Z000SKw): [A Study in Keith](https://vimeo.com/2433947)

#### TOPLAP.org
Selected highlights from the [manifesto of live coding](http://toplap.org/wiki/ManifestoDraft) from [TOPLAP](http://toplap.org):

> We demand:
- Give us access to the performer's mind, to the whole human instrument.
- Obscurantism is dangerous. Show us your screens.
- Programs are instruments that can change themselves.
- Live coding is not about tools. _**Algorithms are thoughts. Chainsaws are tools. That's why algorithms are sometimes harder to notice than chainsaws.**_

> We acknowledge that:
- It is not necessary for a lay audience to understand the code to appreciate it, much as it is not necessary to know how to play guitar in order to appreciate watching a guitar performance.

While SuperCollider (with the 'Just-In-Time' library) is a popular tool for live coders, many opt to build their own languages, and with them, an ideosyncratic approach to live coding practice. (See: Thor Magnusson's [ixi lang](http://www.ixi-audio.net/ixilang/), Alex McClean's [Tidal](http://tidalcycles.org) and Ryan Kirkbride's [FoxDot](https://github.com/Qirky/FoxDot))

[Bill Orcutt](https://en.wikipedia.org/wiki/Bill_Orcutt) (free-improviser and former guitarist of Adris Hoyos' noise band [Harry Pussy](https://en.wikipedia.org/wiki/Harry_Pussy)) created [I DROPPED MY PHONE THE SCREEN CRACKED](http://idroppedmyphonethescreencracked.tumblr.com), a web-audio language for live coding noise music.

More live coders and groups: [yaxu](https://github.com/yaxu), [slub](http://slub.org), [OFFAL](http://offal.github.io), [Joanne](http://joannnne.github.io) of [Algobabez/OFFAL](http://algorave.com/algobabez/), [Shelly Knotts](https://shellyknotts.wordpress.com) of [Algobabez/OFFAL](http://algorave.com/algobabez/), [Heavy Lifting](https://heavy-lifting.github.io), [FoxDot](https://github.com/Qirky/FoxDot), [Anny](http://www.anny.audio), Belisha Beacon, [Click Nilson](https://composerprogrammer.com/research/livecodingpractice.pdf), and finally the [ICLC conference](http://iclc.livecodenetwork.org/) amongst many, many, many others.

### Data Sonification
##### Listen to Wikipedia
The Hatnote sonification allows you [to listen live to Wikipedia's 'Recent Changes' feed.](http://listen.hatnote.com) :notes:

##### The OTHER SuperCollider: CERN's *ATLAS experiment* sonification Project
![ATLAS project](http://atlas.cern/sites/atlas-public.web.cern.ch/files/field/image/Quantizer-infographic-final.png)
> '[ATLAS](http://atlas.cern) is an experiment at CERN designed to explore the secrets of the universe.'
CERN also have their own [Arts programme & artists residency](http://arts.cern/home)...

Look into open-source data sets for other potential sources...

**Q:** Does this really sonification tell us *about* the data set? (Or, as artists, is simply drawing our attention to it enough?)

#### Living off a single-board computer
If you wanted to run SuperCollider [for an installation performnce](http://sc3howto.blogspot.co.uk/2014/05/how-to-keep-installation-running.html), you can run SC 3.6 off something like a Raspberry Pi or BeagleBone Black without *too* much of a headache. There is some info on [this GitHub page](http://supercollider.github.io/development/building-raspberrypi.html) and a tutorial [here](http://sc3howto.blogspot.co.uk/2015/04/how-to-keep-installation-running-on.html)

(See also: [ixi lang](http://www.ixi-audio.net/ixilang/), [Tidal](http://tidalcycles.org), [Chuck](http://chuck.cs.princeton.edu))


***************************************************************************
************************************************************************
# 3. What can microcontrollers do?
What *is* a microcontroller? 💻 A computer minus a few bits (hard drive etc.) Generally, many of the same principles apply, just scaled down a bit. (For instance, there is RAM, just a lot less than you're used to).

The most common and simple to use example is the Arduino:![Arduino UNO](https://www.arduino.cc/en/uploads/Guide/A000066_iso_both.jpg)

(It was originally created as a rapid-prototyping tool for students without a background in electronics and programming!)

The Arduino (which is both a hardware board, a language/compiler, and an IDE (programming environment, based on [Processing](https://processing.org))) can be programmed to accept a number of digital or analog inputs, and control a number of digital or analog outputs. Arduino lets us create physical controls to interact with, interactive environments that read data from the outside world, or control physical things from digital data, amongst many, many other possibilities.


#### Interacting with people 👋
[Marco Donnarumma](http://marcodonnarumma.com/works/xth-sense-biophysical-music/) uses electrode sensors to build his 'Xth Sense' instruments, creating a 'biophysical music'. [VIDEO](https://player.vimeo.com/video/128386465)

Imogen Heap developed 'Mi.Mu', a set of [wearable controllers](https://www.arduino.cc/en/Main/ArduinoBoardLilyPad) (in the form of gloves) to help shape her live performances. More comprehensive information on Heap's gloves [here.](http://www.imogenheap.co.uk/thegloves/)

[![Imogen Heap’s Mi.Mu glove controllers](https://img.youtube.com/vi/JCeASAdiQ0A/0.jpg)](https://www.youtube.com/watch?v=JCeASAdiQ0A)

VIDEO: [Imogen Heap’s glove controllers](https://www.youtube.com/watch?v=JCeASAdiQ0A)

##### Interacting with plants 🌱
Similar technology was used to build the [MIDI Sprout](http://www.midisprout.com), a tool for translating the 'language' of plants into MIDI data. Here is [Robert Aiki Aubrey Lowe](https://www.instagram.com/kirlianauras/)'s (fka [Lichens](https://www.discogs.com/artist/376305-Lichens)) take on the technology.

[![R.A.A.Lowe + MIDI Sprout](https://img.youtube.com/vi/W9ypZAKhRJo/0.jpg)](https://www.youtube.com/watch?v=W9ypZAKhRJo)

VIDEO: [Robert A.A. Lowe + MIDI Sprout](https://www.youtube.com/watch?v=W9ypZAKhRJo)

##### Interacting with Light 🌞
[Leafcutter John](http://leafcutterjohn.com) uses an Arduino with simple photoresistors (light sensors), combined with a Max patch to create light-sensitive algorithmic music:

[![Leaf cutter John’s Light Music](https://img.youtube.com/vi/qTvE0NqpAiE/0.jpg)](https://www.youtube.com/watch?v=qTvE0NqpAiE)

VIDEO: [Leaf cutter John’s Light Music](https://www.youtube.com/watch?v=qTvE0NqpAiE)

##### ~~Electrocuting~~ Interacting with people ⚡️
[![FaltyDL - Straight & Arrow](https://img.youtube.com/vi/BREp7HYxJPw/0.jpg)](https://www.youtube.com/watch?v=BREp7HYxJPw)
>FaltyDL - Straight & Arrow (_Hardcourage_, 2013)

Developing on the work by Japanese artist [Daito Manabe](http://www.daito.ws/en/#1) who you might know from his work attaching electrodes to people's faces...

[![Daito Manabe - Electric Stimulus to Face](https://img.youtube.com/vi/CvmE4TZfeuo/0.jpg)](https://www.youtube.com/watch?v=CvmE4TZfeuo)
>Daito Manabe - Electric Stimulus to Face

##### Interacting with instruments
[![Peter Ablinger - A Letter from Schoenberg](https://img.youtube.com/vi/BBsXovEWBGo/0.jpg)](https://www.youtube.com/watch?v=BBsXovEWBGo)
>Peter Ablinger - A Letter from Schoenberg

All of the above are simple ideas and 'relatively' simple technologies... can you think about developing similar concepts into a larger project? Does this raise more questions? Does this allow us to approach music in a different way?

Music by [Johann Svensson](http://johansvensson.nu) and [Clara Iannotta](http://claraiannotta.com) also makes excellent use of blending 'mechanical' elements with acoustic instruments/human performers.

![Nebulae](http://www.schneidersladen.de/media/catalog/product/cache/1/image/400x400/040ec09b1e35df139433887a97daa66f/1/3/130354_front.jpg)![Nebulae-back](http://www.schneidersladen.de/media/catalog/product/cache/1/image/400x400/040ec09b1e35df139433887a97daa66f/1/3/130354_back.jpg)

The [Qu-Bit Electronix *Nebulae*](http://www.qubitelectronix.com/s/Nebul_Manual_V1_1.pdf) is a Eurorack synth module which uses an Arduino and Raspberry Pi to run CSound and Pure Data patches in a modular synthesiser setup, using the Pi to run the audio and the microprocessor from an Arduino to crunch the controllers and translate the Control Voltage inputs.

***
## REFLECTIONS
###### So, **WHY** program music?
- Invite change & indeterminacy into music.
    - Allow variation and contingency for audience and 'musicians' (who are sometimes the same)
- Music can react to situations, spaces, events etc.

'Programming' (in its most nebulous term) allows a simple way to insert **conversation** and **feedback** into our music, making music interactive.

###### Questions:
What sort of thing can we change in our music?
What can we make interactive, what is possible?
***

##### Andrew Hugill's 'DIGITAL MUSICIAN' _(Routledge, 2008)_
Remember, a ‘digital musician’ should possess the following skills
- **Aural awareness** (an ability to hear and listen both widely and accurately, linked to an understanding of how sound behaves in space and time)
- **Cultural knowledge** (an understanding of one’s place within a local and global culture coupled with an ability to make critical judgements and a knowledge of recent cultural developments)
- **Musical abilities** (the ability to make music in various ways – performance, improvisation, composition, etc. – using new technologies)
- **Technical skills** (skill in recording, producing, processing, manipulating and disseminating music and sound using digital technologies)

***

## Further reading
- ~*Composing Interactive Music: Techniques and Ideas Using Max* Todd Winkler (MIT Press, 2001)~ _EDIT: this is rather out of date now...
- *Handmade Electronic Music: the art of hardware hacking* 2nd ed. ~ Nicolas Collins (Routledge, 2009)
- *Programming Interactivity: a designer's guide to Processing, Arduino, and openFrameworks* 2nd ed. ~ Joshua Noble (O'Reilly, 2012)
- *Electronic and Experimental Music: technology, music, culture* 5th ed. ~ Thom Holmes (Routledge, 2015) ❤️

## Other useful resources
#### Useful Languages
- [Max](http://cycling74.com)
- [SuperCollider](http://supercollider.github.io)
- [CSound](http://csound.github.io)
- [openFrameworks](http://www.openframeworks.cc) (C++)
- [Pure Data](https://puredata.info)
- [Arduino](https://www.arduino.cc) / [Axoloti](http://www.axoloti.com) / Raspberry Pi / other microcontrollers etc.

#### Websites
- [CODE ACADEMY](https://www.codecademy.com) - excellent, free online courses teaching basics of various programming languages and techniques
- [STACK OVERFLOW](http://stackoverflow.com) - questions, problem-solving, etc.
- [INFORMATION AESTHETICS](http://infosthetics.com)
- [PROSTHETIC KNOWLEDGE](http://prostheticknowledge.tumblr.com)
- [CREATIVE APPLICATIONS](http://www.creativeapplications.net) ❤️
- [CREATE DIGITAL MUSIC](http://cdm.link)
- [MANY MANY WOMEN](https://manymanywomen.com)
- [YORKSHIRE SOUND WOMEN NETWORK](https://yorkshiresoundwomen.wordpress.com)
- [RHIZOME](http://rhizome.org)
- [MUFFWIGGLER](https://www.muffwiggler.com/forum/index.php) - synthesiser community forum
- [THONK](shop for DIY synth building, kits, etc.)
- [UBUWEB](http://www.ubuweb.com)
- [TOPLAP](http://toplap.org) - home of live-coding

#### Books
##### Electronic/Computer Music History and Culture
- Electronic and Computer Music ~ Peter Manning
- [Audio Culture: readings in modern music](https://www.bloomsbury.com/uk/audio-culture-revised-edition-9781501318368/) ~ Cristoph Cox & Daniel Warner ❤️ _A revised edition has been released for 2017!_
- The Sound Studies Reader ~ Jonathan Sterne
- Composing Electronic Music ~ Curtis Roads
- Cambridge Companion to Electronic Music ~ Collins & d'Escriván
- Electronic Music (Cambridge Introductions to Music) ~ Collins, Schedel, Wilson

##### Music Technology
- The Computer Music Tutorial ~ Curtis Roads
- The Audio Programming Book ~ Richard Boulanger et al.
- The SuperCollider Book ~ Scott Wilson et al.
- Electronic Music ~ Allen Strange
- Musimathics: the mathematical foundations of music 1 & 2 ~ Gareth Loy
- Sound and Recording ~ Rumsey & McCormick
- Modern Recording Techniques ~ Huber & Runstein
- Cambridge Introductions to Music Technology - Julio d'Escriván
- The Digital Musician - Andrew Hugill

##### Creative Digital Technology
- Getting started with Processing ~ Casey Reas & Ben Fry
- Getting started with Arduino ~ Massimo Banzi
- Visualising Data ~ Ben Fry
- Generative Art ~ Matt Pearson
- Nature of Code ~ Dan Shiffman (also freely available [online](http://natureofcode.com))

#### Journals
- [COMPUTER MUSIC JOURNAL](http://www.mitpressjournals.org/cmj)
- [ORGANISED SOUND](https://www.cambridge.org/core/journals/organised-sound)
- [TEMPO](https://www.cambridge.org/core/journals/tempo)
- [CONTEMPORARY MUSIC REVIEW](http://www.tandfonline.com/toc/gcmr20/current)
- [AUDIO ENGINEERING SOCIETY](http://www.aes.org)

#### International Conferences
- [New Interfaces for Musical Expression (NIME)](http://www.nime.org/archives/)
- [International Computer Music Conference (ICMC)](http://www.computermusic.org/page/23/)
- [International Conference on Live-Coding (ICLC)](http://iclc.livecodenetwork.org)
