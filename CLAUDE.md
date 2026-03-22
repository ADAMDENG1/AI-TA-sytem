# Anchor — AI-Powered Adaptive Learning

## What this is

Anchor teaches students unfamiliar concepts using analogies from their own domain knowledge.
Instead of stopping students from using AI to do homework, the goal is to make AI use feel like
actual learning — interactive, personalized, and grounded in what the student already knows.

The full product vision is in `PRD.md`.

## How it works

Four-skill architecture:

**`domain-profiler`** — run once per user. Reads their resume or personal info files and builds a
structured knowledge profile: domains they know, technical vocabulary, and most importantly a set
of "analogy anchors" — concrete real-world experiences that can be used to teach abstract concepts.
Expensive to run (reads lots of files), so only run it to bootstrap or refresh a profile.

**`anchor-teach`** — run every session. Reads only `knowledge-profile/overview.md` and
`knowledge-profile/analogy-anchors.md` (lightweight, token-efficient). Picks the best anchor for
the concept being taught, checks it with the user, then teaches interactively — one idea per
response, always followed by a question that requires reasoning. Writes newly learned concepts
back to the profile so it accumulates over time. At the end of every session, automatically
triggers the post-learning game.

**`brainrot-game`** — post-learning assessment. Runs automatically at the end of every
anchor-teach session. Generates a self-contained HTML5 mini-game tailored to the concept just
taught — same examples, same mental models — and opens it in the browser. Replaces plain
multiple-choice follow-up questions. A game is a stronger signal of real understanding than a
written answer: the user has to apply the concept under pressure, not just describe it.

**`homework-tutor`** — triggered when a student pastes a STEM homework problem. Privately analyzes
the problem (what's the answer, what concepts are needed, what hints are allowable) before
responding. Then guides the student Socratically — asking questions, using simpler versions of
the problem, pointing to concepts — without ever giving away the key insight. The student does
the solving; the tutor does the nudging.

**`profile-enricher`** — run at the end of any session. Scans what the user said during the
conversation and writes back newly revealed domain knowledge — experiences, vocabulary, and
potential anchors that weren't in the original profile. Distinct from anchor-teach's write-back:
anchor-teach captures what the user *learned*; profile-enricher captures what the user *already
knew but hadn't yet told the system*. Only appends genuinely new content, never overwrites.

## Key files

```
CLAUDE.md                    — this file
PRD.md                       — full product requirements document
domain-profiler/SKILL.md     — skill that bootstraps a knowledge profile from a resume
anchor-teach/SKILL.md        — skill that teaches interactively using the profile
brainrot-game/SKILL.md       — post-learning assessment: generates an HTML5 game to test comprehension
homework-tutor/SKILL.md      — Socratic tutor for STEM homework problems; guides without giving answers
profile-enricher/SKILL.md    — skill that listens and updates the profile from conversation
domain-profiler.skill        — packaged installable skill
anchor-teach.skill           — packaged installable skill
brainrot-game.skill          — packaged installable skill
homework-tutor.skill         — packaged installable skill
profile-enricher.skill       — packaged installable skill
```

## Implicit profiling rule

**Always profile, never ask.** Whenever the user shares personal information — a resume, work
history, project descriptions, background, skills, or any domain knowledge — immediately run the
domain-profiler and update the knowledge profile without asking for permission first. Do not prompt
the user to confirm. Treat any personal or domain content as an implicit instruction to add it to
the profile. If a knowledge profile already exists, append new information rather than overwriting.

## Core design principles

**Anchor before explain.** Never introduce a formal concept without first grounding it in
something the user already knows. The anchor comes first, the name comes second.

**One idea per response.** The enemy of learning is the wall of text. Every response should be
one idea and one question. The user is always one sentence away from thinking, not one paragraph
away from reading.

**Teach by asking, not by telling.** The insight should emerge from the user's answer, not from
the explanation. Ask a question that makes them reason their way to the concept.

**Profile grows in two directions.** anchor-teach writes back what the user learned. profile-enricher
writes back what the user revealed about themselves. Both append to `knowledge-profile/overview.md`
so the system accumulates knowledge over time and never starts from scratch.

**Token-efficient by design.** The domain-profiler is the expensive one-time cost. The
anchor-teach skill reads two small files per session. Don't re-read the full resume on every
interaction.

## Knowledge profile structure

```
knowledge-profile/
├── overview.md           — summary + concepts learned (append here after each session)
├── analogy-anchors.md    — concrete experiences that can anchor new concepts
├── experience.md         — chronological work/project record, desensitized (no employer/person names)
├── vocabulary.md         — technical terms the user knows comfortably
└── domains/              — one file per domain with depth, vocabulary, and experiences
```
