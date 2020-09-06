---
layout: post
title:  "on verse16, rapwiz42 and an artificial rapper"
categories: [idea, docs]
permalink: artificial-rapper
---

*tl;dr:* this post documents the 
* [**rapwiz42 colaboratory notebook**](https://colab.research.google.com/drive/1M27YXWJhepd4rhsB2KwRKGXZTwZRNIvz) and the 
* [**verse16 python package**](https://github.com/pelgo14/verse16) 

and outlines their inception and future.

## how it all came to be...
one evening in February 2020 my friends and i were buying drinks for the night, discussing possible use cases for ai. 
one of us came up with the idea to develop an artificial, ai-powered-rapper. we expanded on the concept and when we 
arrived at my apartment, we thought about of a full blown digital identity, developed and maintained by us, performing 
self-written rap songs. 

our first thoughts revolved around creating a kind of public figure with an online presence, a face and an animated
3d model performing rap songs, which it has written itself. we thought about an artificial artist who only publishes 
**computer generated, but always original content**. with an artificial voice, face and "*life*". the next day i got to 
work and over the next couple of months, i developed multiple prototypes for this artificial artist, which culminated 
in the creation of many prototypes, [**verse16**](https://github.com/pelgo14/verse16) and [**rapwiz42**](https://colab.research.google.com/drive/1M27YXWJhepd4rhsB2KwRKGXZTwZRNIvz). 

we tried "static lyric generators" like [**Song Lyrics Generator**](https://www.song-lyrics-generator.org.uk/rap/) and they 
seem nice when you generate the first text, but the illusion fades after you generate further lyrics and realize,
that the text is almost the same every time, just with changing keywords. this approach is very limited and requires
keywords from the user to bring some dire needed variation to the lyrics. the next day i came across [**this towardsdatascience article**](https://towardsdatascience.com/arctic-monkeys-lyrics-generator-with-data-augmentation-b9b1f7989db0).
this lead me on a new path, into the world of natural language processing, especially natural language generation.
 
## some inspirational heading about text generation, maybe a quote
i initially created prototypes for [**lyric generation**](https://github.com/topics/text-generation), 
**text to rap flow synthesis** (no previous research done), [**voice synthesis**](https://github.com/topics/voice-synthesis), 
and a small command line [**digital audio workstation**](https://github.com/topics/daw). after creating prototypes for
 a few months, i realized i bit off a bit more than i can chew. a long term goal of this whole collection of projects,
 prototypes and thoughts could be a **digital audio workstation supported by artificial intelligence**. maybe i will be
 working on this goal in my free time, maybe not, i go with the flow. for now however, only the lyric-generating part 
 of the project is robust enough for other developers to use. 

## rapwiz42

<p>🔥</p>

write like [this towardsdatascience article](https://towardsdatascience.com/arctic-monkeys-lyrics-generator-with-data-augmentation-b9b1f7989db0).

## verse16
*from [github.com/pelgo14/verse16](https://github.com/pelgo14/verse16)*

install:

```shell 
pip install verse16
```

usage:

```shell 
verse16 --help
verse16 --lines 4
verse16 --lines 16 --log_level DEBUG
```

import as a class:

```python
from verse16 import Generator
gen = Generator(log_level="INFO")
lines = gen.generate(16)
```

## future plans

list all planned functions with demonstration of where in development this is... maybe someone has some ideas (like for the voice)

## reference
there are amazing notebooks and articles on the subject of natural language and lyric generation.
this project incorporates and makes minimal changes to:
* [github.com/minimaxir/gpt-2-simple](https://github.com/minimaxir/gpt-2-simple)
* [towardsdatascience.com/arctic-monkeys-lyrics-generator](https://towardsdatascience.com/arctic-monkeys-lyrics-generator-with-data-augmentation-b9b1f7989db0)
 