MUSS5632M Elec & Comp Music Practice
# Introduction to SuperCollider: A New Hope
Date: Week 9, Tuesday 22 October, 2016
### Summary:
Introductory session to the SuperCollider audio programming language.

http://supercollider.github.io

**NOTE: These next two sessions are intended as a _very_ brief introduction to SuperCollider. You would do well to read up on some of the more detailed resources provided at the end of this document.**
***
## part i.
### Background

##### Why bother with text-based audio programming?
>'allows one to represent musical concepts as objects, to transform them via functions or methods, to compose transformations into higher-level building blocks, and to design interactions for manipulating music in real-time, from the top level structure of the piece down to the level of the waveform.’
James McCartney, 'Foreword' in *[The SuperCollider Book][17]*, eds. Wilson, Cottle, Collins (MIT, 2011)

We can use it to create:
-	real-time interaction
-	installations
-	generative music
-	electroacoustic music
-	audiovisuals
-	live-coding
-	networked performances
-	often open source and thus open-ended…

##### Brief History of SuperCollider (SC hereafter)…
-	Created by James McCartney in 1996 and made open source in 2002.
-	designed for real time: ‘it is a language for describing sound processes.’ ~James McCartney, SCbook Foreword.
-	free, widely used and very well supported on Windows, Mac and Linux.

##### SuperCollider (SC)
-	Split into three parts:
-	scSYNTH: real-time audio server, the core
-	scLANG: interpreted programming language that controls scsynth via OSC.
-	scIDE: an editor for sclang with integrated help system.

***
## part ii.
### 1. Getting Started & The Basics

#### 1.1 Hello, World!
Before we begin, it’s good to know 3 keyboard shortcuts:

Mac:
	`cmd + .` = emergency stop
	`cmd + D` = get help on highlighted text
	`cmd + Enter` = compile line/highlighted text

Windows:
	`ctrl + .` = emergency stop
	`ctrl + D` = get help on highlighted text
	`ctrl + Enter` = compile line/highlighted text

`“Hello, World!”.postln;` // post the message “Hello, World!” in the post window.

When you've typed your code into SuperCollider, you need to *highlight* it and **compile** it with `alt + Enter`.

To hear some audio, we'll first need to boot up our server that handles audio:
`s.boot;` // boot our server

If you need to quit it: `s.quit;`

#### 1.2 Hello, Sine Wave!
Before you try compiling this next line, don't worry too much about what's going on, we'll come to that soon... **MAKE SURE YOU TURN DOWN YOUR VOLUME!**

`{SinOsc.ar}.play;`

Hear that? A pure sine wave! Stop the sound (`ctrl + .`) and let's make this a little more interesting...

``` supercollider
	`{SinOsc.ar(LFNoise0.kr(10).range(500, 1500), mul: 0.1}.play;`
```

#### 1.3 Error!
Hear a sound? Yes? No? What happens if we make a mistake? You get an error message that helpfully points you towards the problem!
```supercollider
ERROR: Parse error
  in file 'selected text'
  line 1 char 54:

  {SinOsc.ar(LFNoise0.kr(10).range(500, 1500), mul: 0.1}.play;
                                                       ^
-----------------------------------
opening bracket was a '(', but found a '}'
  in file 'selected text' line 1 char 54
ERROR: syntax error, unexpected BADTOKEN, expecting ')'
  in file 'selected text'
  line 1 char 54:

  {SinOsc.ar(LFNoise0.kr(10).range(500, 1500), mul: 0.1}.play;
                                                       ^
-----------------------------------
ERROR: Command line parse failed
-> nil
```
Oops, looks like I forgot to close a parenthesis here. That code should end be: `{SinOsc.ar(LFNoise0.kr(10).range(500, 1500), mul: 0.1)}.play;`!

>TIP: you can enlarge or shrink the font size of the editor window with `ctrl + +` or `ctrl + -` (that's "control and plus" or "control and minus") respectively.

Once you've got some sound, remember you can **stop audio** any time with `ctrl + .`

#### 1.5 Changing Values
Let's try something different:
``` supercollider
play({RLPF.ar(Dust.ar([12,15]), LFNoise1.ar(1/[3,4],1500,1600),0.02)})
```

What is happening here? *Subtractive synthesis!*: a resonant low-pass filter is applied to low-frequency random impulses.

Take a few moments to play around with this sound. From the '[Gentle guide to SuperCollider](https://ccrma.stanford.edu/%7Eruviaro/texts/A_Gentle_Introduction_To_SuperCollider.pdf)':
>Stop the sound, change some of the numbers, and evaluate again. For example, what happens when you replace the numbers 12 and 15 with lower numbers between 1 and 5? After LFNoise1, what if instead of 0.3 and 0.2 you tried something like 1 and 2? Change them one at a time. Compare the new sound with the previous sound, listen to the differences. See if you can understand what number is controlling what. This is a fun way of exploring SuperCollider: grab a snippet of code that makes something interesting, and mess around with the parameters to create variations of it. Even if you don’t fully understand the role of every single number, you can still find interesting sonic results.

***
### 2. Housekeeping & Good Coding Practice
Remember last week, we talked about keeping your code clean and well commented? The same applies in SuperCollider!
#### 2.1 Indents & Code Blocks
Indent code block to make their scope (the (way things in (parenthesis) or {braces} run)):
```supercollider
(
play({
	var sines = 5, speed = 6;
	Mix.fill(sines,
		{arg x;
		Pan2.ar(
			SinOsc.ar(x+1*100,
				mul: max(0,
					LFNoise1.kr(speed) +
					Line.kr(1,-1,30)
				)
			), rand2(1.0))})/sines})
)
// see how the code can be broken up over a number of lines to make it easier to read and understand the scope.
```

You can also use parentheses to create *blocks* of code that executes multiple lines at once when you highlight the block:
```supercollider
(
  "My name is, ".post;
  "<my name>.".postln;
 ) // double-click the bracket to hightligh the whole block quickly!
```
#### 2.2 ALWAYS. BE. COMMENTING.
Put the coffee down: ALWAYS. BE. COMMENTING. Well-commented code is always easier to understand and debug!
``` supercollider
// Remember to comment your code!

/* Or
	like this
		over a few
	lines
*/
```
### 3. Variables & Functions
##### 3.1 Variables
You can store numbers, words, UGens, functions and even entire blocks of code in a variable. Variables can be assigned with an equals sign (=):

	x = 10;
	y = 500;
	y; // see what is held in 'y'
	z = (x + y);

Sometimes it will be more helpful to give your variables a slightly longer, more descriptive name. To do this, you need to add a tilde sign (~) at the start of the name to declare a global variable with a longer name.
`~myFrequencies = [415, 220, 440, 880, 220, 990];`

##### 3.2 Functions
**Functions are objects** and can be assigned and treated as objects:
`f = { "Function evaluated".postln; };`
`f.value; // The method .value just says 'evaluate this method now'`


**Arguments**: Functions can have arguments:
`f = { arg a, b; a - b; };`
`f.value(5, 3);`


**Equivalent syntax**: Declare arguments within pipes:
`f = { |a, b| a - b; };`

See [here](https://jahya.net/blog/quickref-for-supercollider/#Functions-and-Other-Functionality) for more information on Functions.
***

### 4. Unit Generators
Remember our basic Sine Wave? (`{SinOsc.ar}.play;`) The sine wave is generated by a special function called a **Unit Generator** (aka a "UGen"). UGens are a basic building block (similar to objects in Max/MSP or Reaktor) that handle all sorts of things (tone generators, impulses, filters, spatialisers, delays). We can string these UGens together to create a signal path similar to that of Max, however we're doing it with a textual rather than a visual language.

Each Ugen has a set of inputs and outputs. Most UGens have just one output, an audio stream or some sort of control signal. The inputs vary a lot depending on the function of the UGen. Like our Max objects, UGens accept a number of *arguments* as their inputs...

Let's examine the `SinOsc` UGen. Type `SinOsc` into your SC editor, highlight it and press `ctrl+D` (or select *Help > Look Up Documentation for Cursor...*). This should open the documentation for `SinOsc` in the Help Browser window...

#### 4.1 SinOsc Class Methods
>*ar (freq: 440, phase: 0, mul: 1, add: 0)
>*kr (freq: 440, phase: 0, mul: 1, add: 0)
**_Arguments_**:
 **freq**:	Frequency in Hertz.
**phase**:	Phase offset or modulator in radians. (Note: phase values should be within the range +-8pi. If your phase values are larger then simply use .mod(2pi) to wrap them.)
**mul**:	Output will be multiplied by this value.
**add**: This value will be added to the output.

#### 4.2 ar & kr
ar = audio rate
kr = control rate

Use kr UGens to control an ar UGen:
```supercollider
  { var ampOsc;
    ampOsc = SinOsc.kr(0.5, 1.5pi, 0.5, 0.5);
    SinOsc.ar(440, 0, ampOsc); // freq: 440, phase: 0, mul: modulated by "ampOsc"
  }.play;
```
***
## part iii.
### SynthDefs
(See [here](http://supercollider.svn.sourceforge.net/viewvc/supercollider/trunk/common/build/Help/Tutorials/Getting-Started/SynthDefs%20and%20Synths.html) for a much more detailed introduction to SynthDefs.)

Up to this point, we've been using simple functions to generate audio. While this is a quick and dirty way to test  audio and learn the basics of UGens, it isn't the most efficient way of working as the Server doesn't understand functions. 'It wants information on how to create audio output in a special form called a synth definition. A synth defintion is data about UGens and how they're interconnected.'

**SynthDefs** (= Synthesis Definition) define a network of unit generators. They are efficient, flexible and much more reusable than functions, which make them useful for large, complex sounds like granular synthesis ('clouds of sound') and ensemble processes.

```supercollider
//first the Function
{ SinOsc.ar(440, 0, 0.2) }.play;

// now here's an equivalent SynthDef
SynthDef.new(\sine, { Out.ar(0, SinOsc.ar(440, 0, 0.2)) }).play; // we could also name this "sine", which is the same as \sine
```

>SynthDef-new takes a number of arguments. The first is a name, usually in the form of a String as above. The second is in fact a Function. This argument is called a UGen Graph Function, as it tells the server how to connect together its various UGens.

You'll also notice that we're specifying an output with the Out.ar UGen (where 0 is the oulet number, and the second argument is the "signal"/SinOsc.ar).

In the SynthDef above, replace the sine wave's fixed frequency (440) with `Rand(440,880)`

Once a SynthDef recipe is known to the system, you can create an individual Synth object to that specification:

Execute the following: `Synth(\sine);` Hear it? That's out SynthDef, and we called it just by using its name!

In fact, it can be used as many times over as you desire (run these lines one at a time):

`a=Synth(\sine);`
`b=Synth(\sine);`
`c=Synth(\sine);`

And these lines one at a time to individually stop each synth, rather than using the `ctrl+.` shortcut:

`a.free;`
`b.free;`
`c.free;`

>Note how each of the Synths got initialised to a random frequency from 440 to 880 when created; this is due to the **Rand** UGen in the SynthDef above!

##### Arguments with a SynthDef
From Nick Collins' [SuperCollider tutorials](http://composerprogrammer.com/teaching/supercollider/sctutorial/3.2%20SynthDefs.html):

Let's now have a look at adding arguments to a SynthDef:

`SynthDef(\sine,{arg freq=440, amp=0.1; Out.ar(0,SinOsc.ar(freq,0,amp))}).add; //added frequency and amp arguments to recipe; make sure they have default values (e.g. freq=440)`

`Synth("sine"); //now this accepts the defaults`

`Synth("sine",[\freq,880]); //this makes another Synth from the recipe an octave up, by being explicit about the frequency argument to the SynthDef`

You can see how this allows us to make lots of related Synths from a common recipe with slight variations in sound between them.


`a=Synth(\sine);`
`b=Synth(\sine,[\freq,550]);`
`c=Synth(\sine,[\freq,660, \amp, 0.5]); `

We can continue to set the named inputs when we feel like it:

`c.set(\freq, 1000);`
`b.set(\amp, 0.3, \freq, 100)`


Finally, use these lines one at a time to individually stop each synth:

`a.free;`
`b.free;`
`c.free;`

**Exercise:**

Try taking a simple synthesis patch you've been working on and turn it into a SynthDef.
As a prototype you want something like:
```supercollider
(
SynthDef(\synthdefname,{arg input1= defaultvalue; //any arguments go here, make sure they have default values

//some code for UGens - the sort of thing that went inside {}.play before

Out.ar(0, finaloutput) //finaloutput is the final result UGen you want to hear
}).add
)
```

`Synth(\synthdefname, [\input1, inputval1]); //inputval1 is the constant starting value for argument input1`

Be sure to check the Document Browser window for a list of UGens to play with.
**ALWAYS READ THE DOCUMENTATION AS IT PROVIDES EXAMPLES OF HOW TO USE THE UGEN**
Some UGens you might like to explore to start with:

- SinOsc: a simple sine wave
- LFTri: a triagle wave
- RLPF: resonant low-pass filter
- MoogFF: a Moog inspired VCF (filter)
- Line: line generator
- LFNoise0 & LFNoise1
- Decay
- Impulse
- Dust: random impulses
- Env: envelope generator (IMPORTANT: read documentation)

#### Plotting & Scoping sound
Sometimes it can be useful to **see** the waveforms we're creating. [SuperCollider gives us a couple of options](http://supercollider.svn.sourceforge.net/viewvc/supercollider/trunk/common/build/Help/Tutorials/Getting-Started/Scoping%20and%20Plotting.html).

The **function plot** makes a graph of the signal produced by the output of the Function. This can be useful to check what's happening, and if you're getting the output you think you're getting:
`{ PinkNoise.ar(0.2) + SinOsc.ar(440, 0, 0.2) + Saw.ar(660, 0.2) }.plot;`

The second method, **Function-scope**, shows an oscilloscope-like display of the Function's output. This only works with what is called the internal server, so you'll need to boot that before it will work (`s.boot`).

`{ PinkNoise.ar(0.2) + SinOsc.ar(440, 0, 0.2) + Saw.ar(660, 0.2) }.scope;`

This should open a new window. The optional'zoom' argument controls the zoom of the scope. You can also scope the output of the internal server at any time, by calling 'scope' on it (i.e. `s.scope`).

Another nice tool (not mentioned in the tutorial) is the frequency analyzer:
`{ PinkNoise.ar(0.2) + Saw.ar(660, 0.2) }.freqscope;`
***

### Recording the output of SuperCollider
If you want to record the output of SuperCollider, you can use this as a quick guide:
```supercollider
// QUICK RECORD
// Start recording:
  s.record;
// Make some cool sound
  {Saw.ar(LFNoise0.kr([2, 3]).range(100, 2000), LFPulse.kr([4, 5]) * 0.1)}.play;
// Stop recording:
  s.stopRecording;

// Optional: GUI with record button, volume control, mute button:
  s.makeWindow;
```
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

#### For next session...
