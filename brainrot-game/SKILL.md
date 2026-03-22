---
name: brainrot-game
description: >
  Generates a self-contained interactive HTML5 mini-game that tests whether a just-learned concept
  actually landed — through play, not multiple choice questions. Claude builds the entire game as a
  single HTML file, saves it to the workspace, and opens it in the browser.

  Trigger this skill at the end of any anchor-teach session, or whenever the user wants to prove
  they understood something: "test me on this", "quiz me", "let's see if I got it", "make a game
  about what we just covered", "brainrot me on X". Also invoke this skill directly from anchor-teach
  at the end of a teaching session — it replaces the plain multiple choice follow-up.

  When in doubt, offer the game instead of asking a text question. It's a better signal of real
  understanding than a written answer.
---

# Brainrot Game Skill

The final step of a learning session. After anchor-teach has taught a concept through analogy and
Socratic dialogue, this skill generates a hands-on mini-game to confirm the concept actually stuck.
No server, no API calls — Claude generates the entire game as a self-contained HTML file and opens
it in the browser.

## Step 1 — Understand the concept

If the user gave you a concept name (e.g. "closures", "async/await"), use that directly.

If they gave you a code snippet with a bug, identify:
- What the bug is (e.g. "off-by-one error", "missing await", "wrong `this` binding")
- What concept the bug illustrates

If it's unclear what concept to teach, ask one short question: "What concept should the game focus on?"

## Step 2 — Pick the right game mechanic

Choose the mechanic that best fits the concept. The goal is that playing the game *is* understanding the concept — not just answering trivia about it.

| Mechanic | How it works | Best for |
|---|---|---|
| **Bug Hunt** | Code shown with clickable tokens. Player clicks the bug. | Off-by-one, wrong operator, null errors, type mistakes |
| **Variable Tracer** | Step through code line by line. At key lines, pick what the variable equals. | Closures, scope, loops, async execution order |
| **Execution Order** | Labeled blocks. Player clicks them in the order they run. | Async/await, promises, event loop, hoisting |
| **Expression Snap** | An expression shown. Answer tiles float up, player clicks the right result. | Operator precedence, type coercion, arithmetic |
| **Fix the Line** | One broken line shown. Player picks replacement tokens from a bank. | Syntax errors, wrong method, wrong operator |
| **Scope Safari** | Code with colored variable references. Player matches each reference to its binding. | Closures, scope chains, `this` binding |

## Step 3 — Generate the game HTML

Write a complete, self-contained HTML5 game. Every challenge must be hand-crafted — write the actual code examples, wrong answers, and explanations yourself. Do not leave placeholders.

### Visual spec

```
Background:   #09090b  (near-black)
Surface:      #111113 / #1a1a1e / #26262c
Border:       rgba(255,255,255,0.11)
Text:         #f2ede3  (warm white)
Secondary:    #c8c2b4
Dim:          #888
Red:          #e83636  (wrong / danger)
Green:        #2ab870  (correct)
Amber:        #f0a030  (warning / partial)
Code purple:  #c8b8ff
Mono font:    'Courier New', monospace
Body font:    system-ui, sans-serif
```

Grid background texture (add to body):
```css
background-image:
  linear-gradient(rgba(255,255,255,.016) 1px, transparent 1px),
  linear-gradient(90deg, rgba(255,255,255,.016) 1px, transparent 1px);
background-size: 40px 40px;
```

### Layout rules

- Fills exactly 100vw × 100vh, no scrollbars
- All CSS and JS inline — single file, no external dependencies
- Round counter visible at all times: **Round X of Y**
- Score visible: **Score: X**
- Large, obvious hit targets (min 44px touch target)
- Smooth CSS transitions throughout

### Rounds

Build 4–5 rounds that progressively increase difficulty or cover different facets of the same concept. Each round should feel like a slightly harder rep of the same exercise.

After each interaction:
- **Correct**: flash green, brief "Good rep" message, show a one-line insight about *why* it's correct
- **Wrong**: shake animation, flash red, show the correct answer with a one-line explanation

### Tone

Use training gym language throughout: "Round X", "Rep X", "Locked in", "Good rep", "Weak rep", "Training complete". Intense but not mean. The vibe is a coach who believes in you.

### End screen

When all rounds are done, show a completion screen with:
- Score as a percentage (big, colored: green ≥75%, amber ≥50%, red <50%)
- Verdict line: "brainrot eliminated" / "still recovering" / "more reps needed"
- 2–3 sentence plain-English summary of what they practiced
- "Run it again →" button that reloads the page

## Step 4 — Save and open

1. Save the file to the workspace as `brainrot-[concept-slug].html`
   Example: `brainrot-closures.html`, `brainrot-async-await.html`

2. Open it in the user's browser using the Chrome tool (`navigate` to the file path using a `file://` URL, or use whatever browser-opening tool is available).

3. Tell the user: **"Your game is ready — play it in the browser! Come back here when you're done and I can debrief or make a harder version."**

## What makes a good game

- The correct answer should never be obvious from the wording alone — the player has to actually reason about the code
- Wrong answers should be plausible mistakes (not random nonsense)
- Every explanation after a wrong answer should teach the concept, not just say "that's wrong"
- The game should feel *kinetic* — clicking, dragging, selecting — not like a form
- A player who plays all 5 rounds correctly should genuinely understand the concept better than before
