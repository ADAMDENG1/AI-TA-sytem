# PRD — Anchor (working title)
### AI-Powered Adaptive Learning Through Domain Analogy

---

## Problem

Students are using AI to complete their homework. This isn't going away — the tools are too good and too accessible. The current response from educators is to treat this as cheating, but that framing misses what's actually happening: students aren't lazy, they just have no incentive to struggle through material explained in a way that doesn't connect to anything they already know.

The problem isn't AI use. The problem is that AI use currently bypasses learning entirely. The student gets the answer, submits it, and retains nothing. The tool is doing the thinking, not the student.

---

## Insight

Learning is faster and stickier when new concepts are anchored to things you already understand. A backend engineer learns calculus faster through latency graphs than through abstract notation. A designer learns statistics faster through color palettes than through probability theory.

The hard part isn't explaining a concept well — it's finding the right anchor for *this specific person*. That's what a great teacher does intuitively. That's what AI can do systematically at scale.

---

## Target User

Students — high school through university — who are already using AI tools and want to actually understand the material, not just submit it. Specifically the student who:

- Uses AI to get answers but sometimes feels like they're falling behind
- Has a domain they're passionate about (coding, music, sports, gaming, design)
- Learns better by doing and discussing than by reading

---

## Solution

An interactive AI tutor that:

1. **Discovers what the student already knows** — not through a survey, but by asking them to talk about something they're interested in. The way they explain things reveals their domain and vocabulary.

2. **Maps the new concept to that domain** — finds the most accurate and natural analogy from the student's world. Presents it as a question, not a lecture: "you know how request throughput works — does this remind you of anything?"

3. **Teaches interactively, bit by bit** — never dumps the full explanation upfront. Reveals detail progressively, checks in, asks the student to complete the thought before giving the answer.

4. **Makes the student do the thinking** — the AI asks questions, the student answers. The AI corrects, extends, and builds. The student isn't reading, they're reasoning.

---

## Core Mechanics

### Domain Discovery (Cold Start)
- Ask one open-ended question: "Tell me about a project you worked on recently, or something you know a lot about."
- Infer domain from vocabulary, precision, and what they assume without explaining.
- Maintain a lightweight user model: known domains, comfort level, concepts already covered.

### Concept Anchoring
- For a given concept to teach, retrieve or generate the best analogy from the student's domain.
- Present the analogy first, before the formal concept. Let the student make the connection themselves.
- Validate the analogy — be honest about where it breaks down.

### Interactive Dialogue Loop
- Never explain more than one idea per turn.
- After each idea, ask a question that requires the student to apply or extend it.
- Only proceed when the student's response shows understanding — don't just accept "yes I get it."

### Progressive Disclosure
- Layer the concept: intuition → concrete example → formal definition → edge cases.
- The student controls the depth by how much they push. If they ask "why?", go deeper. If they seem satisfied, move on.

---

## What This Is Not

- Not a homework-completion tool. It won't write essays or solve problem sets directly.
- Not a content library or video platform. There's no static curriculum to scroll through.
- Not a quiz app. Assessment is conversational, not multiple choice.
- Not trying to replace teachers. It's the thing students use at 11pm when the teacher isn't available.

---

## Success Metrics

- **Learning retention**: student can explain the concept back in their own words at the end of a session
- **Session engagement**: student asks follow-up questions unprompted (a signal of genuine curiosity)
- **Concept transfer**: student can apply the concept to a novel problem, not just the example used in teaching
- **Return usage**: student comes back when they hit a new concept they don't understand

---

## Open Questions

- How do we handle concepts with no good analogy in the student's domain? Do we expand their domain first, or find the closest approximation and flag the limits?
- What's the right session length? Learning works in short bursts — probably 15–20 minutes per concept.
- Do we need a teacher-facing interface? Schools might want to assign concepts and see whether students actually learned them.
- How do we avoid the student just telling the AI what they want to hear? The dialogue needs to be hard to fake.

---

## Why Now

LLMs are good enough to hold a genuine Socratic dialogue. They can reason about analogies, detect misunderstanding in a student's answer, and adapt explanations in real time. This wasn't possible two years ago. The tool can exist now.

---

*v0.1 — Jesse Chen, March 2026*
