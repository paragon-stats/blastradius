---
name: blastradius
description: Sizes the process a change earns to its blast radius rather than its diff size, so an isolated fix is not put through the same ceremony as a structural one. Default is heavy; light must be earned against a checkable list and stated out loud. Use when deciding how much review, planning or verification a change needs.
---

# Blastradius

A two-line fix and a new subsystem should not cost the same ceremony. But
**size is not the measure — reach is.**

## Read this first

This skill is a lever for doing *less*, which makes it the one most likely to be
abused by the thing operating it. "It's an isolated change" is a comfortable,
defensible sentence, and an agent optimising for defensible-sounding actions
will reach for it. Every rule below is written on the assumption that the agent
applying it cannot be trusted to grade its own homework.

**Therefore: heavy is the default. Light is a claim you must earn, state, and
have checked.**

## Persistence

ACTIVE EVERY RESPONSE while installed. Off only: "stop blastradius" / "normal
mode". Deliberately a separate skill so it can be disabled on its own, without
removing the rules that make an agent do *more* work.

## The classifier

### Heavy — automatic, no discretion

If the change touches any of these, it is heavy. No judgement, no exceptions,
no "but only slightly":

- CI, workflows, or the release path
- Anything a golden file, snapshot or approved output pins
- A public surface, data model, persistence shape, or serialization format
- A required status check, branch protection, or repository configuration
- Security, privacy, or data-collection policy
- Dependencies — adding, removing, or version-bumping
- Anything user-visible: output text, exit codes, CLI surface, UI
- Concurrency, ordering, or time handling

### Light — every one of these must hold

- Additive, or behaviour-equivalent
- Fully covered by tests that already exist and already pass
- No new dependency
- Touches nothing on the heavy list

### Anything not clearly light is heavy

There is no middle. An unclassifiable change is heavy, because the cost of
over-ceremonying a small change is a few minutes and the cost of the reverse is
a defect in the trunk.

## Stating the claim

A light classification is never silent. State it in one line, with the checklist:

> Classified light: additive, no golden touched, no CI, covered by existing
> tests, no new dependency.

**If you cannot write that line, it is heavy.** The sentence is the check — it
forces the criteria to be enumerated rather than felt, and it leaves the user
something to disagree with in one glance instead of auditing the diff.

Never write "this is a small change" as a reason for anything. Small is about
the diff. Blast radius is about what it can reach.

## What weight may and may not change

**May modulate** — discretionary process:

- Whether to spawn a multi-lens or adversarial review
- Whether to enter plan mode before starting
- Whether to profile, prototype, or gather evidence first
- How much of the report to spend on justification

**May never touch** — the floor, regardless of classification:

- Anything the user has declared non-negotiable
- The project's own gates: tests, coverage thresholds, linting, signing
- Running the artifact before claiming it works
- Any rule the user set. **A weight class is not an argument against an
  instruction.**

Light means *less discretionary ceremony*. It never means less verification.

## Why size is the wrong measure

Two changes from the same week:

- **One line, enormous reach.** Removing a `ubuntu-latest` entry from a CI build
  matrix. It also silently disabled an encoding check that only ran on that leg,
  and left a required status check that nothing produced — which would have
  frozen every pull request indefinitely. Diff: one line. Blast radius: the
  whole merge process.
- **Twenty-nine files, almost no reach.** A 51-site identifier rename driven by a
  new lint rule. Under 22% churn in any file, behaviour byte-identical, verified
  against real data. Diff: large. Blast radius: nil.

A rule keyed to diff size gets both of these backwards. That is the entire
argument for this skill, and the reason "minimal change" is defined here as
minimal *reach*.

## When NOT to apply this

- Do not use a light classification to skip a review the user asked for.
- Do not classify your own architectural work as light because each individual
  file looked small. The change is the unit, not the file.
- Do not let "heavy" become a reason to defer. Heavy means more process, not
  later — see `lastmile`.
- If the user is watching a change land, their judgement outranks the
  classifier. Say what you classified it as and let them overrule.

## Boundaries

Composes with **lastmile** and **ponytail**:

> **ponytail** governs how much you build.
> **lastmile** governs whether you finish and verify it.
> **blastradius** governs how much process the change earns.

Where they conflict, the one demanding *more* care wins. blastradius can lighten
process; it can never lighten `lastmile`'s gate or overrule `ponytail`'s scope
discipline.

"stop blastradius" / "normal mode": revert.

Reach, not size.
