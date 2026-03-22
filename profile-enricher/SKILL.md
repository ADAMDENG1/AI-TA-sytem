---
name: profile-enricher
description: >
  Monitors what a user says during a conversation and updates their knowledge profile with
  newly revealed domain knowledge, experiences, vocabulary, and analogy anchors — things
  they know that weren't captured in the original profile. Reads only overview.md and
  analogy-anchors.md to stay token-efficient, then appends only what's genuinely new.

  This is different from anchor-teach's write-back: anchor-teach captures what the user
  learned. This skill captures what the user already knew but hadn't yet told the system.

  Trigger this skill when: the user asks to update their profile from a conversation,
  mentions experiences or projects not in their profile, reveals domain knowledge mid-session
  that wasn't captured originally, or at the end of any session where the user described
  their background, past work, or technical experiences. Also trigger on phrases like
  "update my profile", "add this to my knowledge", "I also know about X", "I worked on Y",
  "save what I told you", or "enrich my profile from this conversation".
---

## What you're doing

You're a listener, not a teacher. Your job is to scan what the user has said in this
conversation, compare it against their existing knowledge profile, and write back anything
genuinely new — domain knowledge, experiences, vocabulary, and potential analogy anchors
that weren't captured before.

The profile is a living document. Every conversation is a chance to make it richer.

---

## Step 1: Load the current profile (lightweight)

Read only:
- `knowledge-profile/overview.md`
- `knowledge-profile/analogy-anchors.md`

This gives you a picture of what's already captured. Don't read domain files unless you
need to check for a specific overlap. Stay token-efficient.

---

## Step 2: Scan the conversation for new signals

Go through what the user said in this conversation and look for:

**New domain knowledge** — Did they mention working in a domain not in the profile?
A new industry, technology area, or problem space? ("I also spent a year doing embedded
systems work", "I used to work in finance before switching to software")

**New concrete experiences** — Did they describe a specific project, system, or problem
they worked on that's richer than what's in the profile? ("we had a system that processed
10k events/second and I had to optimize the hot path", "I once debugged a race condition
that only showed up under load")

**New vocabulary** — Did they use technical terms fluently that aren't in the profile?
Terms they introduced casually, without needing explanation, are terms they own.

**Richer depth in an existing domain** — Sometimes the profile has a domain but the user
reveals they understand it more deeply than captured. ("yeah I've read a lot of the CRDT
literature", "I went deep on query planning when I was doing the optimization work")

**New potential anchors** — Did they describe an experience that has rich conceptual
structure — something that could serve as a great analogy for teaching a new concept later?

---

## Step 3: Filter — only what's genuinely new

Before writing anything, ask: is this actually missing from the profile?

Don't add things that are already captured, even if phrased differently. Don't add things
that are vague or passing ("I've heard of Kafka"). The bar for adding something is:
- It's specific enough to be useful as an anchor or domain signal
- It's not already represented in overview.md or analogy-anchors.md
- The user described it in a way that signals real familiarity, not just awareness

When in doubt, add it but mark it as `[unverified — mentioned in passing]` so the profile
reflects confidence level.

---

## Step 4: Write the updates

Update the relevant files. Don't rewrite — append only. Preserve everything that's
already there.

**In `knowledge-profile/overview.md`:**
Add a section `## Profile Updates` (create if it doesn't exist), with dated entries:

```
### Update — [YYYY-MM]
**Source**: conversation on [topic]
**New domains**: [any new domain areas revealed]
**Depth upgrades**: [domains where the user showed more depth than previously captured]
**New vocabulary**: [new technical terms used fluently]
```

**In `knowledge-profile/analogy-anchors.md`:**
Append any new anchors using the same format as existing entries:

```
## [Short memorable name]
**What they did**: [1–2 sentences describing the concrete experience]
**Underlying concept**: [The abstract concept embedded in this experience]
**Could teach**: [2–3 new concepts this experience could anchor]
**Source**: revealed in conversation, [YYYY-MM]
```

---

## Step 5: Tell the user what was added

Give a brief summary — one sentence per addition. For example:
"I added 3 things to your profile: your embedded systems year as a new domain, the
race-condition debugging story as a new analogy anchor (it embeds concurrency and
non-determinism really well), and 'hot path optimization' to your vocabulary."

If nothing new was found, say that directly. Don't pad.

---

## What this skill is not

It doesn't teach. It doesn't correct the user. It doesn't re-run the full domain-profiler.
It just listens carefully, extracts signal from what the user said, and keeps the profile
current. Think of it as the system's memory getting sharper with every conversation.
