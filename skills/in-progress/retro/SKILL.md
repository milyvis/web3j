---
name: retro
description: Retrospective on how a session went — mine friction into CODING_STANDARDS.md, prune what no longer applies, and land it as a PR.
disable-model-invocation: true
argument-hint: "Nothing, to retro this session — or point it at a past transcript"
---

# Retro

Stub: v1 of the **steering hygiene** lens only, the one specified in enough detail to build. Three more lenses were sketched while designing this skill and aren't built yet — see the bottom of this file.

This is the compound-engineering promise — the process improves over time — without compound engineering's failure mode: models are bad at improving their own behavior unsupervised. So `retro` never edits a steering file directly. It proposes, on a branch, as a PR. A human reads the diff before any standard changes.

## Friction

The raw material of a retro pass: a place in this session where you corrected the agent, re-explained something already true, or the agent visibly floundered — retried a tool call, misread a file, took a wrong turn before self-correcting. Mine the conversation for these first. Don't invent friction that didn't happen.

## Routing a piece of friction

Not every finding is a `CODING_STANDARDS.md` candidate. Picking the wrong destination just adds noise to the wrong document:

- **CODING_STANDARDS.md** — a rule about how code should be written or reviewed, enforceable by `/code-review` at review time. The default destination, and the only one this skill should propose without asking.
- **AGENTS.md / CLAUDE.md** — a navigation miss, not a behavior miss: the agent burned turns finding something a pointer would have handed it directly. Flag it, don't write it — these files stay hand-curated, a few highways, not a log.
- **CONTEXT.md** — a domain-language gap. Flag it for a `domain-modeling` pass rather than writing it inline; that skill's active build/sharpen discipline is a different shape of work than a retro proposal.

If a piece of friction fits none of the three, drop it. A retro that force-fits everything into `CODING_STANDARDS.md` is the CLAUDE.md-bloat failure mode this skill exists to avoid.

## Process

1. Mine friction from the current conversation (see above).
2. Route each finding per the section above. Only the `CODING_STANDARDS.md` candidates get written up as a diff; anything else becomes a short list for the human to act on separately, not part of the PR.
3. Read the repo's `CODING_STANDARDS.md` in full, not just the new candidates, and run the deletion test against every existing rule — call the Skill tool with "codebase-design" for its exact wording. A rule earns removal when deleting it changes nothing: the behavior it once corrected doesn't recur, or a newer rule already subsumes it. Propose deletions alongside additions in the same diff. A retro that only adds becomes the mess a standards file turns into when nothing is ever cut.
4. Draft the diff. Branch, commit, open the PR, and hand the human a summary of what's added, what's cut, and why for each. Never write directly to `CODING_STANDARDS.md` — the whole point of routing everything through review is that the model is the one proposing its own constraints, so the human is the gate, not a formality.
5. List any navigation or domain-language findings that survived step 2 after the PR link. They are not part of the diff.

## Not yet built

- **Tool economy** — read the session transcript for expensive or repeated tool calls and propose cheaper tool setups. No reference vocabulary exists yet for this anywhere in the repo.
- **Navigation audit** — go further than flagging (see Routing above) and actually draft the `AGENTS.md`/`CLAUDE.md` highway pointers, rather than leaving them for the human.
- **Multi-session mode** — fan out sub-agents across several past transcripts in the same project before mining friction, so one session's idiosyncrasy doesn't get promoted to a standing rule. The shape: parallel Explore sub-agents, one per transcript, findings reconciled before routing.
