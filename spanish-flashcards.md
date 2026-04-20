# Spanish Flashcards

> A fully interactive, mobile-friendly language learning app I built for myself — with animated cursor trails, celebratory effects, multiple visual themes, and AI-powered content. The most fun I've had building something.

| | |
|---|---|
| **Built for** | Personal |
| **Timeline** | 2 days |
| **Status** | Live |
| **Team** | Solo |
| **Stack** | Next.js 15, TypeScript, Tailwind CSS, Supabase, Google Gemini AI, canvas-confetti, fireworks-js |

---

## The problem

I'm learning Spanish. Existing flashcard apps are fine. They're also, frankly, a bit dull — and when you're doing something in your spare time for fun, the experience of the tool matters more than it does at work. I wanted something that felt good to use: fast, responsive, genuinely satisfying when you get an answer right, and built exactly the way I wanted it to be.

Also, I wanted to see what I could build when the only constraints were my own taste.

## The Chronicles of Narnia

At the time of building this app, I was studying the Narnia audiobook and Kindle book in Spanish. This was done in my own time, not as part of any curriculum, so it was unstructured and difficult to review problem areas. 

## What I built

A full-featured Spanish flashcard app with a few things you won't find in Duolingo:

- **Customisable cursor trails** — multiple themes, each with different particle animations that follow your cursor as you move around the app. This is completely unnecessary and entirely worth it.
- **Animated celebratory effects** — confetti, fireworks, and sound effects that fire when you get a streak going. The app has opinions about your success.
- **Multiple visual themes** — you can change the whole aesthetic of the app with a single toggle.
- **Narnia content** — vocabulary and example sentences pulled directly from the Narnia ebook, so the content stays fresh and contextually relevant to the chapters I'm reading.
- **Mobile-first design** — works just as well on a phone as on a desktop, which matters when you're practising on the go.
- **Progress tracking** — backed by Supabase, so your progress persists across sessions and devices.
- **Full-page chapter text with conjugations** — the Reader section of the app allows me to see conjugations in different colours, search through conjugations, and create custom decks.

## How I built it

Two days of vibe coding, but with a different energy to my work projects. At work, the goal is always to solve a problem as efficiently as possible. Here, the goal was to build something I genuinely enjoyed using. That meant going down some deliberate rabbit holes — getting the cursor trail physics right, tuning the confetti burst timing, experimenting with sound design.

Connecting the Google Gemini AI API to this tool for searching for all conjugations of an infinite verb across the chapter text was new territory for me, but definitely worth the budget.

## How long it took

Two days for the first fully functional version. I've gone back and polished it since — the kind of small refinements you make when you actually use something regularly.

## Where it stands now

Live, fully functional, and in regular personal use. This is the project I'm most likely to show people who ask what I build for fun, because it actually shows what happens when I build something without a committee or a brief or a deadline. I even used it as the topic for one of my Spanish oral exams.

There's a broader point here: the tools and techniques I've developed building production software for clients are the same ones I use when I build something just for myself. Vibe coding, Claude, VS Code, Supabase — the stack doesn't change. What changes is the energy. And sometimes building something purely because you want to is where you learn the most, because there's nothing to blame the brief for.
