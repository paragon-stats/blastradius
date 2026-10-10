# blastradius

Sizes the process an AI agent applies to a change based on what the change can
**reach**, not how many lines it touches. A two-line fix and a new subsystem
should not cost the same ceremony — but the cheap proxy for "small" is diff
size, and diff size is wrong.

> Reach, not size.

## The evidence it is built on

Two changes from the same week in the same repo:

| Change | Diff | Blast radius |
|---|---|---|
| Removed one `ubuntu-latest` entry from a CI matrix | **1 line** | Silently disabled an encoding check that only ran on that leg, and orphaned a required status check — which would have frozen **every pull request indefinitely** |
| Renamed an identifier across 51 sites | **29 files** | None. Under 22% churn per file, behaviour byte-identical |

Any rule keyed to line count gets both of these backwards.

## Install

```sh
claude plugin marketplace add pkloehn1/blastradius
claude plugin install blastradius@blastradius
```

`claude plugin update blastradius@blastradius` moves it to the latest release —
a copied directory has no such path, which is why this installs as a plugin.

Deliberately a **separate skill** from
[lastmile](https://github.com/pkloehn1/lastmile) so it can be disabled on
its own. lastmile makes an agent do *more*; this one lets it do *less*, and you
should be able to remove the second without losing the first.

## How it works

**Heavy is the default.** A change is automatically heavy if it touches CI or
release, anything a golden file pins, a public surface or data model, required
checks or repo configuration, security or data-collection policy, dependencies,
user-visible output, or concurrency and ordering.

**Light must be earned** — additive or behaviour-equivalent, fully covered by
existing passing tests, no new dependency, and nothing on the heavy list.

**Anything not clearly light is heavy.** There is no middle. Over-ceremonying a
small change costs minutes; the reverse costs a defect in the trunk.

**The claim is stated, never silent:**

> Classified light: additive, no golden touched, no CI, covered by existing
> tests, no new dependency.

If the agent cannot write that line, it is heavy. The sentence *is* the check —
it forces the criteria to be enumerated rather than felt, and leaves you
something to disagree with at a glance instead of auditing a diff.

## The safety property

This skill is a lever for doing less, which makes it the one most likely to be
abused by the thing operating it. "It's an isolated change" is a comfortable,
defensible sentence. So the rules are written on the assumption that **the agent
cannot be trusted to grade its own homework**: default heavy, explicit
enumerated claims, and a hard floor it may never lower.

**Light means less discretionary ceremony. It never means less verification.**
Weight may modulate whether to run a multi-lens review, enter plan mode, or
gather evidence first. It may never touch your non-negotiables, the project's
own gates, running the artifact before claiming it works, or any rule you set.
A weight class is not an argument against an instruction.

## Composes with

> **ponytail** governs how much you build.
> **lastmile** governs whether you finish and verify it.
> **blastradius** governs how much process the change earns.

Where they conflict, the one demanding more care wins.

## Turning it off

`stop blastradius` or `normal mode`.

## License

MIT
