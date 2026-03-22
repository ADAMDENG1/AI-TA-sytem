# Anchor — AI-Powered Adaptive Learning

## What this is

Anchor teaches students unfamiliar concepts using analogies from their own domain knowledge.
Instead of stopping students from using AI to do homework, the goal is to make AI use feel like
actual learning — interactive, personalized, and grounded in what the student already knows.

The full product vision is in `PRD.md`.

## How it works

Two-skill architecture:

**`domain-profiler`** — run once per user. Reads their resume or personal info files and builds a
structured knowledge profile: domains they know, technical vocabulary, and most importantly a set
of "analogy anchors" — concrete real-world experiences that can be used to teach abstract concepts.
Expensive to run (reads lots of files), so only run it to bootstrap or refresh a profile.

**`anchor-teach`** — run every session. Reads only `knowledge-profile/overview.md` and
`knowledge-profile/analogy-anchors.md` (lightweight, token-efficient). Picks the best anchor for
the concept being taught, checks it with the user, then teaches interactively — one idea per
response, always followed by a question that requires reasoning. Writes newly learned concepts
back to the profile so it accumulates over time.

## Key files

```
CLAUDE.md                  — this file
PRD.md                     — full product requirements document
domain-profiler/SKILL.md   — skill that bootstraps a knowledge profile from a resume
anchor-teach/SKILL.md      — skill that teaches interactively using the profile
domain-profiler.skill      — packaged installable skill
anchor-teach.skill         — packaged installable skill
```

## Core design principles

**Anchor before explain.** Never introduce a formal concept without first grounding it in
something the user already knows. The anchor comes first, the name comes second.

**One idea per response.** The enemy of learning is the wall of text. Every response should be
one idea and one question. The user is always one sentence away from thinking, not one paragraph
away from reading.

**Teach by asking, not by telling.** The insight should emerge from the user's answer, not from
the explanation. Ask a question that makes them reason their way to the concept.

**Profile grows over time.** After each session, newly learned concepts get written back to
`knowledge-profile/overview.md`. The system knows what the user already understands so it can
build on top of it next time, not start from scratch.

**Token-efficient by design.** The domain-profiler is the expensive one-time cost. The
anchor-teach skill reads two small files per session. Don't re-read the full resume on every
interaction.

## Knowledge profile structure

```
knowledge-profile/
├── overview.md           — summary + concepts learned (append here after each session)
├── analogy-anchors.md    — concrete experiences that can anchor new concepts
├── vocabulary.md         — technical terms the user knows comfortably
└── domains/              — one file per domain with depth, vocabulary, and experiences
```
