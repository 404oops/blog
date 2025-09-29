+++
date = "2025-08-19T19:28:22+02:00"
title = "Comma-Separated Notation: A quick sketch notation"
categories = [ "music" ]
tags = [ "notation", "writing", "experiments" ]
slug = "comma-separated-notation"
+++

Notation isn't really easy. Or hard. But when you're on a quick trip and get a melody and suffer from a memory loss condition, things are hard. Your brain can trip and forget a good melody and then you sit around wondering what it was before you can salvage what's left in your brain.

That's why I searched far and wide for apps that I can use to quickly write notes on. Line notation isn't an option. Not like it's a problem to write, but it's a chore to jot things down quickly before your brain erases the next sequence of notes and dumps your entire idea

Since I found Nothing, I resorted in writing notes in tables. A grid that represents an idea. But I found that to be less overwhelming (but still overwhelming) than manually writing line notation.

Then I found the most brilliant idea. Text can represent musical ideas, it's up to the artist to reinterpret them. But this. This is bigger than the idea. This doesn't precisely remember things like note duration, but it's literally as good as it gets for jotting ideas down quickly so you can replay them in your head and redo them.

## Introducing: CSN

"What could possibly be the difference between CSV and CSN?"

Well for starters CSN and CSV are NOT mutually compatible because CSV is an extension. If you're a programmer, think of it as what Typescript is to Javascript. TS will yell at you if you don't use types, JS will yell at you if you use types because it goes against its syntax.

Let's explain the Extensible spec:

# The Spec:

## Note declarations:

Notes in this system span 2 octaves. Again, this is for sketching simple melodies, but if you wanna go higher you can add numbers that span your respective octaves.

Capital letters are reserved for the default octave, while lowercase letters are reserved for an octave lower. If you prefer using lowercase letters as the default octave, then so be it, but it's up to you to rewrite it to MIDI as your preferred octave

This spec only allows sharp accidentals because they can be easily typed with a hashtag and because flat accidentals can easily be confused with B, the last note on the C scale.

## Note grid declarations

Notes go in a Comma-separated grid relative to your song's time signature. If it's a 4/4, it will have 4 columns, and so on.

```
E,D#,E,D#
E,b,D,C
```

Those were the first 8 notes to Für Elise.

Notes can be extended with a . in the slot, usually meaning an extension that's relative to half of the note's duration, but in this spec's case it's a full note.

## Multiple notes in a cell

Even though one note is allowed within a cell, there are cases where a melody is complex enough that a separate table needs to be added. This allows for multiple notes within one cell.

```
d,(d,f,a,D),B,A
```

## Chords in a cell

Since this spec only allows sharp accidentals, grouping notes in one cell allows for a chord to be written. You do not need parentheses for this but you can use them if it's more legible to you

```
d,(d,f,a,D),Bfd,Aec#
```

# Here come the interesting parts:

## Melody declarations

CSN can hold YAML-like indented key info (due to better legibility) and can hold dual key info in use for melody switching or a possibility of a parallel chord melody sounding way better

```
Amin:
	a,.,x,x
	C,.,x,x
Emin:
	g,.,x,x
	e,.,.,.
F/Dmin:
	F,x,B,x
	C,.,x,x
B/E:
	B,.,x,x
	E,D,c,B
```

## Comments

Comments are declared outside of the table spec. Do not write comments that can be mistaken as notes.

## Computer-parsable Comments

If you really need something that isn't an LLM reading this spec, I strongly suggest you use // or /\* \*/ for comments. Although you won't really need comments in a note sheet, it's best if you enclose them so that a computer doesn't mistake them for notes. Which I doubt they will

# The epilogue?

There is NOT much that I should have to add to this spec. I went overboard with things I don't really use like chords and comments but I thought why not add them since there are some people that really need things like this

With love, by 404oops <3
