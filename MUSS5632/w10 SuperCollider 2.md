Module: MUSS5632M Electronic & Computer Music Practice, Week 10
# Introduction to SuperCollider II: The SuperCollider Strikes Back
November 2016 || 2hrs

### Summary:
Carrying on from last week's introduction to SuperCollider, we'll be looking at **PATTERNS**
:computer:

***
## Table of Contents
- Patterns & Pbind
- Pseq & Sequencing
- Chords
- Scales
- Random sequences
- Rests & \\dur
- Tempo
- Multiple Pbinds
- SynthDefs & patterns
- Taking it further... Live Coding
- Laurie Spiegel & patterns
***

## Patterns & Pbind
> This section borrows heavily from '[The Gentle Guide to SC](https://ccrma.stanford.edu/%7Eruviaro/texts/A_Gentle_Introduction_To_SuperCollider.pdf)'

Today's class focuses on SuperCollider's Pattern family. A set of functions that allow us to create patterns with relative ease.

After booting your server (`s.boot`), run the following line:

`Pbind(\degree, 0).play;`

This plays the note *middle C* once per second.

`\degree` refers to the degrees of the scale as an index (so **0** = the *first*/root note of the scale, which by default is a C major scale. So 0=C, 1=D, 2=E... etc. We'll look at changeing our scales later.

We can use the `\dur` keyword to alter the duration of our notes:
`Pbind(\degree, 0, \dur, 0.5).play;`

So if we wanted to speed things up a bit more, we'd reduce the duration of our notes even more (try 0.2 for now)

## Pseq & Sequencing
Okay, so we have a repeating note... not that interesting, right? That's where `Pseq()` come in, allowing us to create sequences of notes and other values.

In a sequence, we create an **array** of values between square brackets`[]`. This tells us to step through each value in order. We can replace our static **middle C** with a **sequence** to create something more melodic!

`Pbind(\degree, Pseq([0,1,2,3,4,5,6,7]), \dur, 0.2).play;`

By default, our sequence plays through only once before it stops. We can pass `Pseq()` another argument to tell it how many times to repeat (1,2,3,etc.) or even tell it to loop infinitely.

`Pbind(\degree, Pseq([0,1,2,3,4,5,6,7], 5), \dur, 0.2).play;`

There is no reason we can't use a `Pseq` for things other than pitch...

`Pbind(\degree, Pseq([0,1,2,3,4,5,6,7], inf), \dur, Pseq([0.5,0.1,0.3],inf)).play; // notice how the differing loop lengths create a kind of phasing in the melody's rhythm`

Before we continue, let's clean up our code to make it more readable before we add more information:

```supercollider
(
Pbind(
	\degree, Pseq([0,1,2,3,4,5,6,7], inf),
	\dur, Pseq([0.5,0.1,0.3],inf)
).play;
)
```

> There are a few different options for defining pitch in our Patterns...
> - \degree : degree of a scale (default: middle C, major scale)
> - \note : 'chromatic' interval steps (middle C = 0)
> - \midinote : MIDI note (middle C = 60)
> - \freq : specify a frequency in Hz (middle C = 261.6Hz)


## Chords

Chords are incredibly easy to implement in Pbinds. Just write them as a list (comma-separated values in square-brackets `[]`)

```supercollider
(
Pbind(
	\degree, Pseq([0,[3,5],7,[3,6],7,[3,5,9],[4,7],[2,9,11],2], inf),
	\dur, 0.1,
).play;
)
```
We could add some subtle variation to these chords by adding a `\strum` command. This mimicks the strum of a guitar by slightly delaying the attack of each note in a chord. Try a small value to start like `\strum, 0.1,`

### A quick word on Scales
Remember that our default scale for the `\degree` setting is major?
If we want to change that, we have to define a new scale. SuperCollider has a *lot* of default scales and tuning systems built in, so we can just call them (or define our own).
> To get a full list of scales available, run: `Scale.directory;`

```supercollider
// first we create a new Scale from the Scale library
~setScale = Scale.minor;

// then, inside our Pbind, we call this instead of the default!
(
Pbind(
	\scale, ~setScale,
    // patterns!
    ).play
)
```
There is a lot more to scales than this in SuperCollider, so make sure you check-up in the reference and the scales directory!

You can also offset a scale by changing the `\root` note.
Try shifting the root of your scale from default 0 to -12.

## Random sequences
### Prand
A number of Pbind variants offer the ability to later our patterns and add a degree of randomness to the values called... **Prand** takes a list of values and chooses from them randomly, rather than in sequence:

```supercollider
(
Pbind(
	\scale, ~setScale,
	\degree, Pseq([0,2,4,[0,3,7],5,[2,6],4], inf),
	\dur, 0.1,
	\strum, 0.1,
).play;
)
```
Try changing the above Pseq sequence to a Prand pattern and notice the difference.

#### Other useful Patterns...
From Wilson & Hewitt's [tutorial documents](http://supercollider.svn.sourceforge.net/viewvc/supercollider/trunk/common/build/Help/Tutorials/Getting-Started/Sequencing%20with%20Patterns.html):

>Many patterns take lists of values and return them in some order.

**Pseq**(list, repeats, offset) -- return the list's values in order
**Pshuf**(list, repeats) -- scramble the list into random order
**Prand**(list, repeats) -- choose from the list's values randomly
**Pxrand**(list, repeats) -- choose randomly, but never return the same list item twice in a row
**Pwrand**(list, weights, repeats) -- like Prand, but chooses values according to a list of probabilities/weights

>Other patterns generate values according to various parameters. In addition to these basic patterns, there is a whole set of random number generators that produce specific distributions, and also chaotic functions.

**Pseries**(start, step, length) -- arithmetic series, e.g., 1, 2, 3, 4, 5
**Pgeom**(start, grow, length) -- geometric series, e.g., 1, 2, 4, 8, 16
**Pwhite**(lo, hi, length) -- random number generator, uses rrand(lo, hi) -- equal distribution
**Pexprand**(lo, hi, length) -- random number generator, uses exprand(lo, hi) -- exponential distribution

>Other patterns modify the output of value patterns. These are called FilterPatterns.

**Pn**(pattern, repeats) -- repeat the pattern as many times as repeats indicates
**Pstutter**(n, pattern) -- repeat individual values from a pattern n times. n may be a numeric pattern itself.

## Rests & `\dur`
If we have a sequence of durations for our pattern, we can add rests by inserting a Rest() into our Pseq:

`	\dur, Pseq([1,1,1,1,Rest(1)],inf),`


## Tempo
The values for `\dur` in Pbind refer to the *number of beats* (i.e. 1 = one beat, 0.5 = half a beat).
SuperCollider's default tempo is 60 BPM (beats per minute). You can specify a different tempo by creating a new `TempoClock()`. The following example from the '[Gentle Guide to...](https://ccrma.stanford.edu/~ruviaro/texts/A_Gentle_Introduction_To_SuperCollider.pdf)' shows a `Pbind` set to play at 120 BPM:

```supercollider
(
Pbind(\degree, Pseq([0, 0.1, 1, 2, 3, 4, 5, 6, 7]),
        \dur, 1;
).play(TempoClock(120/60)); // 120 beats over 60 seconds: 120 BPM
)
```

We could also define the TempoClock as a variable to make it easier to call:

`t = TempoClock(120/60)`

and then call it at the end of the play function as `).play(t);`

## Multiple Pbinds
It's straightforward to run multiple Pbinds. Just add each Pbind().play instance inside a parenthesised (()) block of code:

```supercollider
(
t = TempoClock(200/60);

Pbind(
	// pattern
).play(t);

Pbind(
	// a simultaneous pattern
).play(t);
)
```

If you want to explore playing Pbinds in a time-ordered fashion, take a look at the `{ }.fork` method.

**You can also create a pattern of patterns**
>**Excercise:**Try creating four Pbind pattern of four notes each, and nest them inside a larger Pseq or Pxrand (or any other kind of pattern) to create changing melodies/basslines/chords/beats!

***
## INSTRUMENTS: Pbinding with a SynthDef!

Remember our SynthDefs from last week? Now it's time to play our synth as a pattern!
`\instrument, \nameOfYourSynthDef`

The arguments from the SynthDef are available to the Pbind using the Pbind key (i.e. `\+argName`)

Some Tips:
-To control pitch, make sure your SynthDef has a `freq` argument, and SuperCollider should handle the pitch conversion (midi/note/degree) for you!
- *IMPORTANT*: Make sure that your SynthDef includes either a sustained envelope (`Env.adsr`) or a `doneAction:2` to make sure that you free up synth nodes and don't overload the server as you create lots of voices!

>**Excercise:** Try to build a basic drum set from SynthDefs (Maybe a kick, snare and hi-hat), and then see what kind of rhythms you can develop for them...
> Tip: look into the `\delta` argument as a way to approximate beat timings...

## Taking Patterns further: Live-Coding
>While not done in SuperCollider, the following video is a great example of live-coding patterns. You can see how patterns are modified and updated in real-time to create a musical and *expressive* performance.
> Andrew Sorensen's *A Study in Keith*: https://vimeo.com/2433947

If you'd like to explore live-coding a bit more in SuperCollider, the [Just In Time Library](http://doc.sccode.org/Overviews/JITLib.html) is an extension to SuperCollider that allows on-the-fly programming, or **live-coding**.

One of JITlib's incredibly useful classes is the [Pdef](http://doc.sccode.org/Classes/Pdef.html) that allows us to store patterns globally and modify them on-the-fly.


## Laurie Spiegel & Pattern Manipulation
In her 1981 article '[Manipulations of Musical Patterns](http://retiary.org/ls/writings/musical_manip.html)', computer music pioneer Laurie Spiegel identifies the role of various pattern algorithms in her (and much other) music:

1. Transposition
2. Reversal
3. Rotation
4. Phase Offset
5. Rescaling
6. Interpolation
7. Extrapolation
8. Fragmentation
9. Substitution
10. Combination
11. Sequencing
12. Repetition
13. The Great Unknown

>**Excercise:** After reading this short article, can you use SuperCollider to implement these pattern manipulations?


***
***

## RESOURCES FOR [SUPERCOLLIDER](http://supercollider.github.io)

The following guides were used in creating this lesson, they are each *incredibly* useful and worth further reading...

- [Computer Music (with examples in SC3)](http://www.mat.ucsb.edu/275/CottleSC3.pdf) ~ David Cottle
    - .pdf book on ‘Computer Music’ generally with specific focus on SuperCollider
- [Quick Reference for SC](https://jahya.net/blog/quickref-for-supercollider/
) ~ Andrew McWilliams
- [Getting Started with SC](http://supercollider.svn.sourceforge.net/viewvc/supercollider/trunk/common/build/Help/Tutorials/Getting-Started/Getting%20Started%20With%20SC.html
) ~ Scott Wilson and James Harkins
- [Nick Collins’ SuperCollider Tutorial](http://composerprogrammer.com/teaching/supercollider/sctutorial/tutorial.html
)
- [Gentle Guide to SuperCollider](https://ccrma.stanford.edu/~ruviaro/texts/A_Gentle_Introduction_To_SuperCollider.pdf) ~ Bruno Ruviaro
