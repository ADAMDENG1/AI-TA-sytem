# AI TA System

This repository is the early scaffold for an AI teaching assistant system built around one core idea: people learn faster when new concepts are explained through things they already understand.

Right now, the repository contains the first concrete building block for that system: a Claude skill called `domain-profiler`. Its job is to read a person's resume, background notes, project writeups, or other personal information and turn that material into a structured knowledge profile. That profile can later be used by a teaching agent to explain unfamiliar concepts through familiar analogies.

## What This Repo Is

This is not a finished app yet. It is a design and skill repository for an adaptive AI tutor.

The current repo includes:

- `PRD.md`: the product direction for the teaching system
- `domain-profiler/SKILL.md`: the skill definition for extracting domain knowledge
- `domain-profiler/evals/evals.json`: eval cases for the skill
- `domain-profiler.skill`: packaged skill artifact

## Why We Have This

Students already use AI to get answers. The problem is that most AI tools optimize for completion, not understanding.

This project exists to push AI in a different direction:

- identify what a learner already knows
- use that prior knowledge as an anchor
- teach interactively instead of dumping answers
- make the learner do the reasoning

The `domain-profiler` exists because personalized teaching depends on having a good model of the learner. Before an AI tutor can explain calculus through backend latency, or probability through queue failures, it needs a structured map of the student's real domains, vocabulary, and experiences.

## How It Works

The current workflow is:

1. Collect source material about the learner.
   Examples: resume files, project summaries, interview answers, personal notes.

2. Run the `domain-profiler` skill against those materials.

3. The skill reads everything first, then extracts:
   - an overall learner summary
   - domain-specific knowledge files
   - a vocabulary list
   - analogy anchors based on real experiences

4. Save the results into a `knowledge-profile/` folder.

That output is meant to become the memory layer for a future tutoring system. Instead of teaching with generic examples, the tutor can map new concepts to the learner's strongest mental models.

## Installation

There is no standalone application to install yet. This repo is currently a Claude skill/project workspace.

### Prerequisites

- Git
- Claude Desktop or Claude Code with project/workspace skill loading

### Clone The Repo

```bash
git clone https://github.com/ADAMDENG1/AI-TA-sytem.git
cd AI-TA-sytem
```

### Open It In Claude

Clone the repository:

```bash
git clone https://github.com/ADAMDENG1/AI-TA-sytem.git
cd AI-TA-sytem
```

Then in Claude, open or select this repository as the project/workspace. Claude will load the skill files from the repo.

The skill source lives here:

```text
domain-profiler/SKILL.md
```

### Use The Skill

Once the repo is open in Claude, ask for learner profiling work, for example:

```text
Analyze my background files and build a knowledge profile using the domain-profiler skill.
```

The expected output is a `knowledge-profile/` folder containing:

- `overview.md`
- `domains/`
- `analogy-anchors.md`
- `vocabulary.md`

## Current Status

This repository is in an early stage.

- The product vision is defined in `PRD.md`
- The learner profiling skill is implemented
- The full interactive teaching system is not yet built

## Next Likely Steps

- build the teaching agent that consumes `knowledge-profile/`
- generate concept-to-analogy mappings from the profile
- add session memory and student progress tracking
- evaluate whether the tutor improves retention and transfer
