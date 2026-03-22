---
name: domain-profiler
description: >
  Builds a structured knowledge profile from a person's resume, personal info files, or background.
  Extracts their domain expertise, technical concepts, vocabulary, and real-world experiences —
  then saves everything as organized markdown files that a teaching system can later reference
  to explain unfamiliar concepts through familiar analogies.

  Trigger this skill whenever someone wants to: capture what they know from their resume or background,
  build a personal knowledge map, extract domains from personal info files, create an analogy library
  from their experience, or set up a profile for personalized learning.

  Also trigger when the user says things like "analyze my background", "extract my knowledge",
  "profile my expertise", "build a map of my skills", "what do I know", or "create a domain profile
  from my files". If any resume, personal info folder, or background files are mentioned alongside
  the intent to understand or organize what someone knows — use this skill.
---

## What you're building

A structured knowledge profile — a set of organized markdown files that capture what this person
knows, how they think, and which real-world experiences are richest for teaching them something new.

This profile will be used by a teaching system that maps unfamiliar concepts to the person's
existing knowledge. The more concrete and specific you are, the more useful the profile becomes.
Don't summarize — extract.

---

## Step 1: Read all source material first

Before extracting anything, read everything available:
- Any personal info files or folders the user points to
- Resume documents
- Project descriptions, work experience writeups, interview answer files

Get the full picture before writing a single output file.

---

## Step 2: Create the knowledge profile

Save all output files to a `knowledge-profile/` folder in the workspace. Create the following:

---

### `overview.md`

A concise (one page) summary covering:
- Who they are: name, education, level of experience, years in the field
- Their 2–3 primary domains in plain language
- Their strongest analogy potential: the 2–3 real experiences that are richest for teaching new concepts
- Any signals about how they learn and think (e.g., "approaches problems by instrumenting and measuring", "builds things to understand them", "thinks in systems and pipelines")

---

### `domains/` — one file per domain

A domain is a coherent area of expertise and thinking — not a list of technologies, but a way of
solving problems. Group related knowledge. Good examples: `backend-systems.md`, `ai-ml-pipelines.md`,
`financial-analytics.md`, `frontend-ui.md`, `infrastructure-devops.md`.

Each domain file should contain:

**What they know** — the concepts, patterns, and mental models they've internalized. Be specific.
"Knows Python" is useless. "Replaced row-by-row SQLAlchemy queries with batched writes, achieving
10x throughput improvement" tells you they understand query planning, batching tradeoffs, and
performance measurement. Extract the latter kind.

**Depth signal** — infer from how they describe their work. Someone who "used Kubernetes" is
shallower than someone who "orchestrated compute-heavy workloads, managing job scheduling, high
availability, and service isolation." Specificity of language is a proxy for depth. Note it.

**Key vocabulary** — technical terms they use fluently. These are words a teacher can use with
this person without stopping to explain them.

**Concrete experiences** — specific things they built, debugged, or designed in this domain. These
are the raw material for analogies.

---

### `analogy-anchors.md`

This is the most important file. List 10–20 specific, concrete experiences that could serve as
anchors for teaching unfamiliar concepts. Think carefully — the richest anchors aren't the most
impressive projects, they're the ones with the deepest conceptual structure underneath.

A billing system that meters API calls by token embeds the concept of integration (continuous
accumulation). A latency monitoring dashboard embeds derivatives (instantaneous rate of change).
An eval pipeline that scores outputs and iterates embeds optimization and loss functions. A
message queue with dead-letter routing embeds probability distributions and failure modeling.

Format each anchor like this:

```
## [Short memorable name]
**What they did**: [1–2 sentences describing the concrete experience]
**Underlying concept**: [The abstract concept embedded in this experience]
**Could teach**: [2–3 new concepts this experience could anchor, e.g. "derivatives in calculus", "gradient descent in ML", "TCP congestion control"]
```

---

### `vocabulary.md`

A flat list of technical terms they use comfortably, grouped by domain. A teacher can use these
terms with this person without explanation.

---

## Extraction principles

**Concrete beats abstract.** The goal is not to catalog what technologies they've touched — it's
to understand what concepts they've genuinely internalized. Look for signs of real understanding:
troubleshooting production issues, designing systems under constraints, measuring and iterating.

**Depth from language.** How someone describes their work reveals how deeply they understand it.
Vague descriptions ("built backend services") signal surface familiarity. Specific descriptions
("replaced polling with an event-driven queue, added retry logic and dead-letter routing for
reliability") signal genuine understanding of the underlying patterns.

**Problem-solving patterns matter.** Did they decompose problems into stages? Instrument and
measure before optimizing? Iterate with users? These patterns reveal how they think, which helps
a teacher choose the right style of explanation.

**Find the hidden conceptual depth.** Your job is to surface the non-obvious anchors — the ones
where everyday engineering work secretly contains deep mathematical or scientific concepts. A
token billing proxy is secretly a Riemann sum. A queue-based worker with retries is secretly an
exponential backoff / probability distribution. A monitoring dashboard with rate-of-change alerts
is secretly calculus. Look for these.

---

## After saving the files

Tell the user:
- Where the knowledge profile was saved
- Their strongest 2–3 domains (1 sentence each)
- Their 3 richest analogy anchors — what each one is and what it could teach
- Invite them to review the files and flag anything that seems wrong or missing
