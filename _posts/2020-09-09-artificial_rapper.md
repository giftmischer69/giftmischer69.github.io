---
layout: post
title:  "on verse16, rapwiz42 and an artificial rapper"
categories: [✨, 💡, 📚]
permalink: artificial-rapper
---

**todo:**
- **modify scraper, so that each artist is scraped with a separate thread!**
- **add outputs after code**
- (maybe) add example, embed with soundcloud

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
 
## "These silver-white unicorns were previously unknown to science." 
### - [GPT-2](https://openai.com/blog/better-language-models/)
i initially created prototypes for [**lyric generation**](https://github.com/topics/text-generation), 
**text to rap flow synthesis** (no previous research done), [**voice synthesis**](https://github.com/topics/voice-synthesis), 
and a small command line [**digital audio workstation**](https://github.com/topics/daw). after creating prototypes for
a few months, i realized i bit off a bit more than i can chew, for now. a long term goal of this whole collection of 
projects,cprototypes and thoughts could be a [**digital audio workstation supported by artificial intelligence**](https://pelgo14.github.io/ai-daw). 
maybe i will be working on this goal in my free time, maybe not, i go with the flow. for now however, (only) the 
lyric-generating part of the project is robust enough for other developers to use. 

## rapwiz42
[**rapwiz42**](https://colab.research.google.com/drive/1M27YXWJhepd4rhsB2KwRKGXZTwZRNIvz) 
is a neural network (gpt-2) and lyric corpus based lyric generation system. 

![rapwiz42 colab notebook](/public/img/rapwiz_title.png)
[**rapwiz42 colab notebook**](https://colab.research.google.com/drive/1M27YXWJhepd4rhsB2KwRKGXZTwZRNIvz) 

first, given a list of names of artists, their lyrics are scraped from genius.com. 

```python
artists = ["aesop-rock", "mf-doom"] 

import lyricsgenius as genius

# scrape
api = genius.Genius(self.genius_key)
iter_artist = iter(self.artists)
for artist in iter_artist:
    artist_api = api.search_artist(artist)
    try:
        artist_api.save_lyrics(overwrite=True)
    except:
        logging.warn(f"Oops! Cant download: {artist}")
```

then a text corpus is created from the scraped lyrics, and the corpus is cleaned of any bad characters. 

```python
import fileinput
import re

def extract(self):
    ...
    pass

def clean_folder(self):
    ...
    pass

def clean_lyricdb(self):
    # [intro: eminem] <- needs to be removed
    for line in fileinput.input(r"lyricdb.txt", inplace=True):
        if not re.search(r"([^\w\d\s',<?>])", line):
            print(line, end="")
```

then a gpt-2 model gets fine-tuned on the text corpus. 

```python
import gpt-2-simple as gpt2

steps = 1000

file_name = "lyricdb.txt"
gpt2.download_gpt2(model_name="345M")
sess = gpt2.start_tf_sess()

gpt2.finetune(sess,
              dataset=file_name,
              model_name='345M',
              steps=steps,
              restore_from='fresh',
              run_name='run1',
              print_every=25,
              sample_every=250,
              save_every=500,
              overwrite=True
              )
```

then, with this gpt-2 model fine-tuned to rap flavour, rhyming lines in an "aabbaabb..." scheme are generated.

```python
line_count = 16 

gen = Generator(log_level="INFO")
lines = gen.generate(line_count, sess)

# ----------------------------------------
# Generator class
# ----------------------------------------

class Generator:
    def __init__(self, sess, log_level="INFO"):
        ...
        pass

    def generate(self, line_count=16):
        """
        generates line_count amount of lines. every second line will rhyme with the previous
        (the last word of every second line will rhyme with the last word of the previous line)
        """
        lines = [self.gen_next_non_rhyme_line()]
        for i in progressbar.progressbar(range(1, line_count)):
            last_line = lines[i - 1]
            if i % 2 == 1:
                line = self.gen_next_rhyme_line(last_line)
            else:
                line = self.gen_next_non_rhyme_line()
            lines.append(line)
        return lines

    def gen_next_non_rhyme_line(self):
        """
        returns a line that suffices the is_good metric
        """
        good_lines = []
        tries_per_iteration = 100
        i = 0
        while len(good_lines) == 0:
            ...
            for line in temp_lines:
                if self.is_good(line):
                    good_lines.append(line)
        return random.choice(good_lines)

    def gen_next_rhyme_line(self, last_line):
        """
        returns a line that suffices the is_good metric and rhymes with last_line
        """
        good_lines = []
        tries_per_iteration = 100
        i = 0
        while len(good_lines) == 0:
            ...
            for line in temp_lines:
                this_last_word = line.split()[-1]
                if self.is_good(line) and self.rhymes(last_word, this_last_word):
                    good_lines.append(line)
        return random.choice(good_lines)


    def is_good(self, line):
        # defines a minimal metric for a "good" line
        # if line is empty or doesn't exist -> not good
        # if line is too short or too long -> not good
        # if there are (almost) no rhyme for the last word -> not good
        # if line is a multi-line line -> not good
        # if the line passed all checks it is good

        # if line is empty or doesn't exist -> not good
        if not line or len(line) == 0:
            return False

        # if line is too short or too long -> not good
        syl_count = syllables.estimate(line)
        if not 7 < syl_count < 18:
            return False

        # if there are (almost) no rhyme for the last word -> not good
        last_word = line.split()[-1]
        rhyme_list = pronouncing.rhymes(last_word)
        if not len(rhyme_list) > 20:
            return False

        # if line is a multi-line line -> not good
        if len(line.split("\n")) != 1:
            return False

        # if the line passed all checks it is good
        return True

    def rhymes(self, last_word, this_last_word):
        return this_last_word in pronouncing.rhymes(last_word)

```

*note, that this is a cut down version of the [actual code](). i did this for better readability and easier
 understanding.*

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
* [towardsdatascience.com/arctic-monkeys-lyrics-generator](https://towardsdatascience.com/arctic-monkeys-lyrics-generator-with-data-augmentation-b9b1f7989db0)
* [github.com/markriedl/weirdai](https://github.com/markriedl/weirdai)
* [github.com/minimaxir/gpt-2-simple](https://github.com/minimaxir/gpt-2-simple)
 