# Sci-Fi Bot (@scifisanity)

> A Twitter bot that wrote its own science fiction: one-line story seeds put together from hand-built word lists and 53 sentence structures, plus quotes from sci-fi authors, all served from a Sanity content backend. Built in March 2020, before generative AI could do this for you.

| | |
|---|---|
| **Built for** | Personal |
| **Timeline** | March – May 2020 build; posted until September 2021 |
| **Status** | Retired; 2,456 posts |
| **Team** | Solo |
| **Stack** | Python, Twython (Twitter API), Sanity (content model, GROQ queries, bulk import), Google Sheets |
| **Links** | [@scifisanity](https://x.com/scifisanity) · [Source code](https://github.com/kelknightly/scifibot) · [Sanity Community Digest #12](https://www.sanity.io/blog/community-digest-12) |

---

## The problem

I followed a lot of art and generator bots on Twitter, and wanted a good science fiction one to go with them. I couldn't find one, so in the first weeks of COVID lockdown I built it.

## What I built

The bot posts two kinds of tweet.

**Generated story seeds.** Short, complete scenarios like:

> A forgotten star system is home to a theocratic civilisation of spiders. They have acquired a nuclear noose and will use it for war.

> A troubled smuggler destroys a brutal microbe with a zero-gravity virus.

**Quotes.** Lines from science fiction writers, with author and source, such as *"my paws work beautifully as fins!" ~ Italo Calvino, The Complete Cosmicomics*.

There was no language model behind any of this. The generator is grammar and vocabulary I wrote myself:

- **Sentence structures designed in a spreadsheet.** I started by mapping out sentence patterns, such as *[descriptor] [actor] [positive or negative action] [descriptor] [actor] aboard [descriptor] [vessel]*, each with a worked example. That grew to 53 structures in Python.
- **Word lists written by hand.** Actors, descriptors, actions, vehicles, weapons, places and the prepositions that join them, sorted into types so a sentence could ask for "a transport action" or "a place descriptor" and get one that fits.
- **Sanity as the content backend.** I modelled the vocabulary as Sanity content types (nouns, descriptions, actions, article words, quotes, authors, sources) and bulk-imported the spreadsheets. By the end it held 3,834 documents: 1,528 nouns, 1,507 descriptors, 308 actions and 364 quotes from 31 authors. The bot reads them with GROQ queries, so adding words meant editing content, not code.
- **Getting the grammar right.** Most of the 78 commits are fixes like this: "a" versus "an" (with an exceptions list), pronouns, plurals, and sentences too long for a tweet. Every candidate tweet is checked against Twitter's 280-character limit.
- **Never repeating itself.** Each tweet is hashed and logged, and the bot picks again if it has posted that text before.

## Recognition

Sanity featured the bot in [Community Digest #12](https://www.sanity.io/blog/community-digest-12) (20 March 2020), the week it launched, along with a technologist profile of me (as Kelly Quantrill). They also fed one of its tweets into GPT-2 to see how a language model would continue the story. Looking back, that was an early glimpse of where this kind of work was heading.

## Looking back

This is the earliest project here, and it's the same kind of work I do now in a different form. Content modelling, structured vocabularies, and output you can trust because the rules are explicit are what I now bring to prompts, schemas and agent outputs. Building it by hand also taught me how much of "generation" is really careful data design.
