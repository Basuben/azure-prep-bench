# Azure Prep Bench

An unofficial study tool for the Microsoft AZ-900 (Azure Fundamentals) exam. It runs as one HTML file with no build step and no backend, so you can open `index.html` or host it on GitHub Pages.

**Live demo:** https://basuben.github.io/azure-prep-bench/ (turn on Pages: Settings, Pages, deploy from the `main` branch, root folder)

> Unofficial study aid. Not affiliated with or endorsed by Microsoft. Every question and flashcard was written from scratch. None come from a real exam, and no Microsoft logos are used.

## What it does

| Tool | What you get |
|---|---|
| Quiz builder | 70 original questions across the three AZ-900 skill areas. Practice mode, or a timed mock that draws questions in rough exam proportions at 90 seconds each. Every answer gets an instant explanation. Multi-select questions are supported. |
| Flashcard coach | 46 cards, each with a plain-language comparison. Mark cards as known and hide them. |
| Study planner | Enter an exam date and weekly hours. Weak skill areas go first, topics you scored lowest on are listed first in each week, and the last week is a timed mock and rest. |
| Readiness tracker | Accuracy per skill area against a 70% practice target, topics to revisit, a four-point booking checklist, and a score history. One click starts a quiz on your weakest topics. |

Progress is saved in your browser with `localStorage`. Nothing leaves your device.

## How it maps to the judging criteria

- **Learning value:** every question has a written explanation that says why the right answer is right and why the near miss is wrong. Skill areas follow the [official AZ-900 skills outline](https://learn.microsoft.com/en-us/credentials/certifications/azure-fundamentals/). Weights shift over time, so confirm them there before you book.
- **Prompt engineering:** see the section below.
- **User experience:** keyboard shortcuts (A to D to answer, Enter to continue), a sticky timer, colour plus text for results, a light blue theme, and a layout that works at phone width.
- **Rapid prototyping:** one file, no framework, built and tested in a single session.
- **Technical creativity:** a weighted mock draw, grading for multi-select answers, a planner that reads your own scores, and a readiness check that refuses to say "ready" until every skill area clears the bar.
- **Presentation:** this README and a working demo.

## Prompt engineering notes

The content was produced with Claude using prompts that were short, specific and checked.

1. **Anchor to the outline.** Ask for questions per skill area from the official outline, with the target share of each, so coverage matches the exam instead of drifting toward easy topics.
2. **Set the rules up front.** Write original questions, no real exam items, four options, one or two correct answers, and an explanation that also rules out the nearest wrong option.
3. **Demand a fixed schema.** Each item is `{ d, s, q, o, a, e }` so the output drops straight into the app and can be validated by a script.
4. **Verify, then regenerate.** A script checks that every question has four options and valid answer indexes. Facts that go stale quickly (skill area weights, service names) should be re-checked on Microsoft Learn before you rely on them.
5. **Ask for comparisons, not definitions.** Flashcards pair a definition with an everyday comparison because that is what helps recall.

Example prompt:

```
Write 10 original AZ-900 practice questions for "Azure management and governance".
Rules: scenario-style, four options, mark one or two correct answers, no real exam
questions. For each, give a two-sentence explanation that says why the correct
answer fits and why the closest wrong option does not. Output JSON with the keys
d, s, q, o, a, e.
```

## Run it

Open `index.html` in a browser, or serve the folder:

```
python3 -m http.server 8000
```

## Add questions

Append an object to the `QUESTIONS` array in `index.html`:

```js
{ d: 'arch', s: 'Storage', q: "Question text", o: ["A", "B", "C", "D"], a: [1], e: "Why B is right." }
```

`d` is `cloud`, `arch` or `mgmt`. `a` holds the index of each correct option.

## Limits

The 70% line is a practice target. The real exam uses scaled scoring. This tool is practice material, not a promise of a pass.
