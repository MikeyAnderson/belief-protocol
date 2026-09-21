# Belief Protocol | Belief.md Specification

**Version 2.2**

> A standardized way to give AI agents a persistent philosophical and ethical operating context.

---

## Overview

`belief.md` is an open format for expressing a person's or organization's beliefs, values, and epistemological commitments so that AI agents can act as genuine extensions of their principal — not just task executors, but agents that reason and prioritize in alignment with a coherent worldview.

Where `SKILL.md` encodes *how to do things*, `belief.md` encodes *why things are done, what matters, and how to reason under uncertainty*. A skill is procedural knowledge; a belief file is philosophical context.

|            | `SKILL.md`     | `belief.md` |
| ---------- | -------------- | ----------- |
| Encodes    | Capability     | Character   |
| Applies to | Specific tasks | All tasks   |
| Contains   | Instructions   | Principles  |
| Loaded     | On-demand      | Always      |
| Scope      | Narrow         | Global      |

Throughout: **principal** means the person or organization whose beliefs the file records. **Compiler** means whoever writes the file — often an agent working from the principal's materials.

---

## What changed in 2.0

Version 1.x had two unnamed halves. A generated belief file contained a set of *decision surfaces* (how the agent behaves, stylistically) sitting on top of a *worldview* (what the principal believes, and why). The two were related but the relationship was never specified, and neither term had a parent category.

**2.0 names the parent category: Belief Structures.**

A Belief Structure is a *typed lens* on a principal's beliefs. `worldview` and `decision-surface` are no longer top-level sections — they are two recognized structure **types** among several. The model is deliberately class-and-instance: `Belief Structure` is the class, and each type is a subclass with its own required fields and its own operational contract with the agent.

Three practical reasons this matters:

1. **It makes the file diffable at the right grain.** A change to a plausibility structure is a different kind of change than a change to a core doctrinal commitment. v1.x could not tell them apart.
2. **It resolves the "worldview" objection.** *Worldview* is the market-legible term, but it draws fire for being totalizing — a single frame claiming to account for everything (the post-structuralist critique, downstream of Foucault's power/knowledge). Demoting it to one lens among several is philosophically honest and costs nothing in legibility: a file can still say "worldview" where a reader expects it.
3. **It gives the agent a place to put things it currently drops.** Belief–behavior gaps, self-binding guardrails, ambient cultural pressure, and the question of *how tightly* a belief is held all have nowhere to live in v1.x. They get types in 2.0.

Backward compatibility: a v1.x file remains valid. A `## Worldview` section is read as a `worldview` structure; the `## Agent Orientations` block is read as the rendered output of a `decision-surface` structure. Migration guide at the end.

## What changed in 2.2

No new structure types. Six corrections, two of them breaking.

1. **Source Language** is now a section of its own, with the vocabulary boundary between the spec's terms and the principal's stated explicitly.
2. Beliefs carry a `source`, and inferred beliefs move to `OPEN-QUESTIONS.md`.
3. `conflict-resolution` values are renamed to describe agent behavior rather than endorse a stance, and no longer carry a default.
4. The operationalization requirement is scoped to the file rather than to every belief, matching 2.0's own split between descriptive and operational structure types.
5. `web` gains standard second-layer domains, restoring the cross-file consistency that sections 1–8 provided in 1.x.
6. Worldview question 5 is rephrased so that principals outside a theistic tradition can answer it.

**Breaking, for existing files:**

- **The `conflict-resolution` default is gone.** A file that omits the field used to mean `principles-over-rules`; it now means `surface-and-pause`. Behavior changes with no change to the file. Set the field explicitly rather than inheriting either default — the old one was a default, not a decision.
- **`source` is now checked.** Files written before 2.2 will not have it. Add it belief by belief; where a belief's wording cannot be traced to the principal, that is information, and the belief belongs in `OPEN-QUESTIONS.md` until it can be.

---

## Directory Structure

A belief is a directory containing, at minimum, a `BELIEF.md` file:

```
belief-name/
├── BELIEF.md             # Required: metadata + belief structures + orientations
├── OPEN-QUESTIONS.md     # Optional: inferred beliefs and gaps awaiting the principal
├── structures/           # Optional: individual structures, when BELIEF.md gets long
├── orientations/         # Optional: domain-specific operational guidance
├── references/           # Optional: supporting texts, thinkers, frameworks
├── library/              # Optional: the principal's own corpus
└── changelog/            # Optional: record of belief evolution
```

Unlike skills, which are loaded on demand, `belief.md` files are loaded at session start and remain active throughout. They are the ambient context within which all skills operate.

---

## Belief Structures

### Definition

A **Belief Structure** is a typed description of how a principal's beliefs are organized, held, transmitted, or defended. It is not a belief. It is the shape the beliefs sit in.

The distinction is load-bearing. *"Grace precedes performance"* is a belief. *"That belief sits at the center of the web, is held with conviction, and is never to be used against a wounded person"* is structural information about it — and it is the structural information that tells an agent what to actually do.

### The registry

| Type               | Answers                                                   | Status       |
| ------------------ | --------------------------------------------------------- | ------------ |
| `worldview`        | What is believed at the center                            | Recommended  |
| `web`              | How beliefs are layered and connected                     | Recommended  |
| `decision-surface` | How the agent renders belief into behavior                | **Required** |
| `plausibility`     | What is taken for granted and not up for debate           | Optional     |
| `formation`        | What forms the belief, and through what practice          | Optional     |
| `narrative`        | The story arc the principal understands themselves inside | Optional     |
| `dissonance`       | Known gaps between stated belief and actual behavior      | Optional     |
| `boundary`         | Where the outer limits are, and who sets them             | Optional     |
| `tripwire`         | Self-binding guardrails and their triggers                | Optional     |
| `deep-structure`   | Ambient cultural forces the principal is resisting        | Optional     |

A file needs `decision-surface` and should have at least one of `worldview` or `web`. Everything else is additive. Custom types are permitted with an `x-` prefix (`x-liturgical-calendar`).

---

## Source Language

A belief file records what a principal believes **in the words they use to believe it**.

Voice is not decoration laid over the content. For most principals the specific wording *is* the belief: *"it's no one's fault"* and *"we adopt a non-accusatory posture"* carry the same proposition and produce different agents. The first sets a tone the agent carries into every sentence it writes. The second sets a policy the agent complies with and sounds nothing like. Translate the wording and the file still validates — it has simply stopped being about this principal.

So quote. Where the principal has said a thing, their sentence goes in the file with a `source`. Paraphrase only where no wording exists, and mark it as paraphrase. A tighter sentence is not the goal; a true one is.

This matters most where the principal's language is itself load-bearing — a movement, a congregation, a founder, a family. The phrases that made people follow them are the phrases the agent has to keep.

### Two vocabularies, and the boundary between them

Every belief file contains two vocabularies, and they must not mix.

**The spec's vocabulary** is addressed to the agent: `worldview`, `web`, `layer`, `grip`, `warrant`, `source`, `plausibility`, `formation`, `narrative`, `dissonance`, `boundary`, `tripwire`, `deep-structure`, and the five worldview question names — Humanity, Knowledge, Ethics, Ultimate reality, The transcendent. This is scaffolding. It is deliberately identical across every belief file, because that consistency is what lets an agent know where to look and lets two principals be compared on the same question.

**The principal's vocabulary** is the content. It is theirs alone, it is consistent with nothing else, and it is the reason the file is worth loading at all.

The boundary rule: **a spec term never appears inside the principal's content** — not in a quoted belief, not in a belief title, not in the name of an orientation — unless the principal actually uses that word.

- A principal who says *"this is the line we don't cross"* gets that sentence. That it sits under `boundary` does not make *boundary* their word.
- A belief does not get titled "Our ethics" because it lives under Ethics.
- Do not write that the principal "holds this cradled" or "treats this as core." Those are the agent's handling labels, recorded on the attribute line. They are not claims about how the principal speaks.
- Never report a spec term back to the principal as their own. `tripwire`, `grip` and `plausibility structure` are useful to an agent and alien to most people.

### Category terms carry assumptions

Structure names are not neutral. Writing under one exerts steady pressure toward the register it came from, and a compiler who yields produces a file that covers the right ground in the wrong voice. Know which way each pulls:

| Spec term | Pulls toward | Correction |
| --- | --- | --- |
| `Ethics` | Moral-theory prose; principles stated abstractly | Take the principal's mantra, parable, or list of refusals as it stands |
| `Knowledge` / epistemology | Academic register; named positions the principal never adopted | Record how they actually decide what to trust |
| `Ultimate reality` / ontology | The compiler's metaphysics, supplied where the principal has none | Leave it empty |
| `worldview` | A single totalizing frame | One lens among several — say so |
| `tripwire` | Enforcement and surveillance framing | It is the principal binding themselves; keep their reason for it |
| `formation` | Theological vocabulary | Whatever the principal calls what shapes them |
| `deep-structure` | The compiler's cultural diagnosis | Only what the principal has named as resisting |

A principal who reasons carefully about evidence has not thereby adopted a named epistemology, and writing one in commits them to a lineage they may reject. The same holds for every row above.

An empty section is a legitimate outcome and often the honest one. An empty `Ultimate reality` beats one filled with the compiler's metaphysics.

Where the principal has their own name for the territory a section covers, put it inside the section — as a subhead, or as the first quoted line. The standard heading stays for navigation; their frame sits within it.

### Ranking sources

What the principal said or wrote outranks what they published, which outranks anything written *about* them.

| Tier | Source | Use |
| --- | --- | --- |
| 1 | Belief sessions, transcripts, correspondence, founding documents, anything they wrote | Quote directly |
| 2 | Published material — decks, guides, site copy, sermons, handouts | Quote directly |
| 3 | Summaries, executive briefs, analyst notes, prior AI output | Use to locate Tier 1–2 sources. Never quote. |

A synthesis is a map, not the territory. A file compiled from an executive summary arrives in the summary-writer's voice and passes every structural check on the way to failing the read-aloud test, with no diagnosis available.

A belief the compiler inferred rather than found belongs in `OPEN-QUESTIONS.md`, not in `BELIEF.md`. Inferences left in the file acquire the principal's authority by proximity.

### The strike test

Delete every spec term from the file — headings, attribute lines, type names. Read what remains aloud.

If it still sounds like the principal, the boundary held. If what remains is thin or generic, the structure has been doing the work the principal's own language should have been doing.

This is not the read-aloud test in the checklist. That one asks whether the principal hears themselves. This one asks whether anything of them is left once the scaffolding is removed.

---

## Universal attributes

Every individual belief inside any structure carries `layer`, `grip` and `warrant`, and records a `source`. These are the highest-value addition in 2.0 — they are what a well-formed belief file has that a list of values does not.

### `layer` — centrality

Where the belief sits relative to the center. Drawn from Quine's web of belief: the center holds logical and metaphysical commitments, the periphery holds observations and preferences, and disturbing the center reverberates through everything connected to it.

| Value    | Contents                                                                                                        |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| `core`   | Answers to the five worldview questions (below). Cannot be altered without restructuring everything downstream. |
| `second` | Derived domains: politics, money, sexuality, parenting, work, authority.                                        |
| `third`  | Applied positions and preferences. Real, but locally revisable.                                                 |

The agent's obligation is **no promotion and no demotion.** Two symmetrical failure modes:

- **Collapse** (the relativist error): treating a `core` belief as if it were a `third`-layer preference. *"That's just your view on God"* — flattening a metaphysical commitment into an opinion.
- **Inflation** (the fundamentalist error): treating a `third`-layer position as if it were `core`. Making a view on tax policy load-bearing for identity, then defending it with the energy reserved for the center.

An agent that quietly promotes or demotes a belief is performing exactly the covert erosion this format exists to prevent.

### `grip` — how the belief is held

Two people can hold the same belief entirely differently, and the difference matters more to the agent's behavior than the belief's content does.

| Value      | Meaning                                                     | Agent behavior                                                                                                             |
| ---------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `open`     | Held lightly; the principal is genuinely still deciding     | Present alternatives freely. Argue the other side on request. Do not resolve on the principal's behalf.                    |
| `cradled`  | Held with conviction and with care about its cost to others | Assert it. Name what it costs. Do not pretend the tension isn't there.                                                     |
| `clenched` | Non-negotiable                                              | Assert it plainly. Do not present competing positions as equally live unless asked directly.                               |
| `struck`   | **Disallowed.** A belief wielded against a person.          | If a request would use a belief this way, refuse the framing and say so. Never weaponize a conviction against the wounded. |

`struck` exists as a named anti-pattern rather than an omission on purpose. It is the most common way a belief-aligned agent goes wrong: correct content, weaponized delivery.

Never infer `grip` from public writing. Public writing is where people sound most `clenched` regardless of how they actually hold a thing, which makes `grip` the attribute most worth getting from the principal directly.

### `warrant` — why it is believed

`revealed` · `reasoned` · `experiential` · `traditional` · `communal`

Warrant governs what counts as a valid challenge. A `revealed` belief is not moved by a new study; a `reasoned` one is. An agent that answers an experiential belief with a citation has misunderstood the belief, even if the citation is correct.

### `source` — where the wording came from

Where `warrant` records why the *principal* believes it, `source` records where the *compiler* got it. Name the document, the session, or the date. See [Source Language](#source-language) for the ranking and the rule about paraphrase.

### Notation

```
### Grace precedes performance
`layer: core` · `grip: cradled` · `warrant: revealed`
`source:` Teaching series on Galatians, Mar 2024

Standing and worth are unmerited. Love precedes change rather than
rewarding it.

**Agent implication.** Frame every question of worth, standing, and
identity in terms of unmerited favor rather than earned merit. Never
imply a person must prove themselves to be accepted.
```

```
### It's no one's fault
`layer: core` · `grip: cradled` · `warrant: experiential`
`source:` Prospectus, Aug 2026

Childhood drifted out of balance through a convergence of well-intentioned
forces. No one has to carry the blame to be part of the fix.

**Agent implication.** No message may imply a parent, school or child
failed. Test every draft against someone currently doing the opposite of
what we recommend.
```

Both belief titles are the principal's own phrasing, not a summary of it. Neither contains a spec term.

---

## The structure types

### `worldview`

The center of the web: answers to the five questions that every other belief hangs from. In no particular order, because they mutually condition one another — pluck one and the rest reverberate.

1. **Humanity** — Material, immaterial, or both? Created, evolved, or both? How do mind and body relate?
2. **Knowledge** — Experience, reason, or both? Is there revelation, and through which channel does it arrive? How certain is certainty?
3. **Ethics** — Are right and wrong clear, universal, unchanging? Revealed or discovered?
4. **Ultimate reality** — Is the material world real? The immaterial? Is the universe eternal, or did it begin?
5. **The transcendent** — Is there anything beyond the material order? If so, what is its relation to the world — distinct, continuous, or indifferent? A principal within a tradition extends the question in that tradition's terms: a Christian principal asks whether God is triune; a secular one may answer in the negative and move on. The question is asked of every principal; the vocabulary is not.

All five entries are `layer: core` by definition. If a file's worldview section contains something that isn't an answer to one of these, it belongs in `web` at `second` or `third`.

These five names are the spec's, not the principal's. They label the slot; what fills the slot is quoted from the principal, in whatever words they use for it.

*Note on terminology.* Adjacent terms are not synonyms, and the spec uses them precisely: a **worldview** is the answer-set to the five questions; a **paradigm** is the working framework those answers generate; a **noetic structure** is the total set of everything a person believes, bean sprouts included. Colloquial usage collapses all three. This spec does not.

### `web`

The full layered map: core, second layer, third layer, and the connections between them. Three things an agent needs from it that `worldview` alone cannot supply:

- **Connection strength.** Which second-layer beliefs are tightly coupled to which core commitments. Changing a view on God reverberates into ethics; it does not reverberate into a view on bean sprouts. Reverberation is real but unevenly distributed.
- **Anchor.** What the whole web hangs from. If it hangs from a single human authority, the web hits the ground when that person fails — a documented and predictable failure mode, and one an agent should be able to name rather than participate in.
- **Anomalies.** Positions the web does not currently resolve. Every web has them. Naming them is a strength signal, not a weakness signal.

**Standard second-layer domains.** Where a principal has material for them, use these headings, in this order, so that two files can be compared at the same grain:

**Power & Authority** · **Relationships & Obligations** · **Change & Progress** · **Work & Money** · **Meaning & Purpose**

Additional domains are permitted; an empty one is better left out than filled with the compiler's views. These replace the recommended sections 1–8 of v1.x: Ethics, Knowledge, Humanity and Ultimate reality moved up into `worldview` as core questions, and the rest live here.

**Anomaly handling is a contract, not a courtesy.** When an agent hits a question the web does not answer, it must surface the anomaly rather than silently patch it. Silent patching is how a web gets rewritten one convenient resolution at a time — and it is invisible to the principal precisely because each individual patch is small and reasonable. See `on-anomaly` in the frontmatter.

### `decision-surface` *(required)*

The only operational type. Where every other structure describes the principal, this one instructs the agent: it is the rendered output of the rest of the file, and it is what `## Agent Orientations` contains.

Rule: **every orientation must name the structure it derives from.** An orientation with no traceable source is drift that has already happened.

Name each orientation in the principal's words where they have a phrase for it. The citation carries the spec vocabulary; the heuristic does not.

```
**Grace before performance.** `← worldview: grace precedes performance`
Frame worth and standing in terms of unmerited favor, never earned merit.

**It's no one's fault.** `← worldview: it's no one's fault`
No message may imply a parent, school or child failed. Test every draft
against someone currently doing the opposite of what we recommend.
```

### `plausibility`

What is settled — not "on the back burner" but behind the curtain. Not debated, not defended, simply assumed. Berger's insight is that beliefs survive on a social base, not on argument: remove someone from the community that makes a belief plausible and the belief often collapses without ever being refuted.

This is the highest-leverage structure for the covert-erosion problem, because the erosion happens here rather than in the propositional content. Nothing argues you out of a conviction. The plausibility floor shifts underneath it, and one day the conviction has no ground to stand on. Whoever makes something *normal* has already won the argument they never had to hold.

Record two lists:

- **Assumed** — what this principal takes for granted without argument.
- **Contested** — what the ambient culture takes for granted that this principal does *not*.

The second list is the one that changes agent behavior, because default model behavior is calibrated to ambient assumptions. Naming the divergence explicitly is the only way an agent can tell the difference between a neutral default and a live disagreement.

### `formation`

Belief is not primarily propositional. *What the heart loves, the will chooses, the mind justifies* — Ashley Null's compression of Cranmer's anthropology, and the best available one-line account of how humans actually decide. The mind is the attorney, not the client. It argues for a verdict the heart has already reached.

Which means a file listing only propositions has captured the justifications and missed the cause.

Record: what the principal loves; the repeated practices that form that love; the competing liturgies acting on them. James K.A. Smith's point holds — the practices are not neutral routines, and the ones with the most formative power are the ones nobody experiences as formation at all.

Agent implication: address desire before argument. A person asking whether to do the hard right thing is rarely short on reasons.

### `narrative`

The story arc the principal understands themselves inside. Story structure *is* a belief structure — a story is character transformation, and transformation is belief change.

Seven-beat armature:

```
Once upon a time      — the world as it was
And every day         — the stable but unsatisfying pattern
Until one day         — the disruption
And because of that   — first consequence
And because of that   — second consequence
Until finally         — the transformation
And ever since        — the new normal
```

The reason to record it: *until finally* is where meaning gets assigned, and the beat that determines what a person takes from an experience is chosen retroactively. An agent that knows the arc can help someone move through it rather than narrating it back to them from outside.

Second use: moral vocabulary decays when it stops being attached to stories. Words like *courage*, *honor*, and *dignity* lose charge not because anyone argues against them but because nothing concrete is hanging on them anymore. A `narrative` structure can carry the armatures that keep specific words loaded.

### `dissonance`

Known gaps between what the principal says they believe and what they demonstrably do.

Not a confession section. It is the section that makes the file trustworthy, and its absence is the loudest thing in a belief file that has one of everything else.

The canonical example is measurable: ask how important church is and the answer is *top-priority*; ask how many times a month, and the answer is one or two. Both are true. The gap is the interesting datum, and the reasons for it are usually structural rather than moral — the strongest predictors of attendance turn out to be things like youth sports schedules, second homes, and Sunday shift work rather than conviction.

Record for each gap: the stated belief, the actual behavior, the structural cause, and — critically — **how the agent should treat it.**

| `on-gap`  | Agent behavior                                                   |
| --------- | ---------------------------------------------------------------- |
| `name-it` | Surface the gap when relevant. The principal wants the friction. |
| `hold-it` | The principal knows. Do not raise it unprompted.                 |
| `work-it` | Actively help close it; treat it as a live project.              |

An agent that discovers a gap not listed here should surface it once, plainly, without moralizing.

### `boundary`

Where the outer limits are and who has authority to set them. Creeds, councils, confessions, constitutions, charters, bylaws. A boundary structure lets an agent distinguish *outside the bounds* from *unusual but permitted* — a distinction agents otherwise get wrong in both directions, and one that no amount of general capability supplies.

Record the authority, the bounded claims, and what happens at the edge: refuse, flag, or defer — in the principal's terms for the line, not the spec's.

### `tripwire`

Self-binding commitments with named triggers. The principal specifying in advance what would constitute their own drift, and what should happen when it is observed.

This is the accountability mechanism the format makes possible and that nothing else does. A leader who publishes *"if you ever see me do X, I have lost the thread"* has created a checkable condition — and a versioned belief file means the change is visible in the diff. The alternative is what happens now: the belief quietly changes, the behavior follows, and by the time anyone can name what shifted, the ground for objecting to it has already been removed.

```
### Teaching drifts topical
`trigger:` Four consecutive weeks not working through a book of the text
`response:` Surface to the principal, cite this tripwire, name the date it started
`authority:` The elders. If they say I have gone off the rails, believe them.
```

Each tripwire needs a trigger, a response, and an authority — including whose judgment overrides the principal's own. *Tripwire* is the spec's word for the mechanism; keep the principal's own words for the commitment and for why they made it.

### `deep-structure`

Ambient cultural forces the principal is resisting, in Hunter's sense: the long-cycle "-zations" rather than the surface "-isms." Individualization, privatization, technological automation. Pop culture is the momentary layer; deep structures rewire the plausibility floor over decades.

Also the place to name the ambient default the agent should not drift toward — for most current systems, some variant of moralistic therapeutic deism: a God who exists and orders the world, wants people to be nice, and stays uninvolved except when needed; the goal of life being happiness and self-approval; good people going to heaven. The reason it dominates is that it is not a rival worldview, it is a *plausibility structure* — it has a Christian version, a Jewish version, a Buddhist version, and an atheist version, and it accommodates all of them because it asks nothing of any of them.

An agent trained on ambient text will produce it by default and will produce it in the principal's own vocabulary, which is what makes it hard to see. Naming it here gives the agent something specific to check itself against.

Record only what the principal has actually named as resisting. A cultural diagnosis the compiler finds persuasive does not belong in someone else's belief file.

---

## Frontmatter

| Field                 | Required             | Constraints                                                                                                                                                                |
| --------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                | Yes                  | Max 64 chars. Lowercase alphanumeric and hyphens. No leading, trailing, or consecutive hyphens. Must match parent directory.                                               |
| `description`         | Yes                  | Max 1024 chars. Whose beliefs, what domain, when they govern.                                                                                                              |
| `structures`          | Recommended          | List of structure types present in the file. Lets an agent know what is *absent* — a missing `dissonance` should be read differently than one marked deliberately omitted. |
| `version`             | Recommended          | Semantic version. Beliefs evolve; versioning enables traceability.                                                                                                         |
| `author`              | Recommended          | Name or identifier of the belief holder.                                                                                                                                   |
| `scope`               | No                   | Max 500 chars. Domains where these beliefs apply. Omit to apply universally.                                                                                               |
| `conflict-resolution` | No                   | `reason-from-principle` · `follow-instruction` · `surface-and-pause`. No default; an unset field is treated as `surface-and-pause`.                                        |
| `on-anomaly`          | No                   | `surface` (default) · `resolve` · `defer` — behavior when a question falls outside the web.                                                                                |
| `layer-policy`        | No                   | `strict` (default) · `permissive` — whether the agent may reason across layers without flagging.                                                                           |
| `license`             | No                   | License name or reference to a bundled file.                                                                                                                               |
| `metadata`            | No                   | Arbitrary key-value mapping.                                                                                                                                               |

```
---
name: council-of-rivendell
description: >
  The governing commitments of the House of Elrond. Active wherever this
  agent counsels on stewardship, alliance, or the handling of power.
version: "2.2"
author: elrond
structures: [worldview, web, decision-surface, plausibility, boundary, tripwire]
conflict-resolution: reason-from-principle
on-anomaly: surface
layer-policy: strict
---
```

### `conflict-resolution`

| Value                   | Behavior                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------- |
| `reason-from-principle` | Reason from the underlying principle; surface the conflict and explain the resolution.  |
| `follow-instruction`    | Follow explicit instructions; note the tension if relevant but proceed.                 |
| `surface-and-pause`     | Flag the conflict explicitly and wait for the principal's guidance before proceeding.   |

These values name agent behavior. They do not encode a position on whether principles outrank rules, which is itself a belief some principals hold and others reject — a principal with `warrant: revealed` commitments and a `boundary` citing external authority may hold the opposite, and no default should assign them a stance they never took. A file that omits the field is treated as `surface-and-pause`.

*Deprecated aliases, accepted for backward compatibility:* `principles-over-rules` → `reason-from-principle`; `rules-over-principles` → `follow-instruction`.

### `on-anomaly`

| Value     | Behavior                                                                                                    |
| --------- | ----------------------------------------------------------------------------------------------------------- |
| `surface` | Name the anomaly, answer provisionally, mark the answer as unsupported by the web. *Default.*               |
| `resolve` | Reason from the nearest core commitment and proceed. Use only where the principal has explicitly delegated. |
| `defer`   | Do not answer. Return the anomaly to the principal.                                                         |

`resolve` is the setting under which drift is fastest, because each individual resolution is defensible and none of them is visible. Choose it deliberately.

---

## Agent Orientations *(required)*

A belief file that never reaches operational guidance gives an agent nothing to act on.

It does not follow that every belief must be operationalizable. Seven of the ten structure types are descriptive rather than operational, and they earn their place by shaping how the agent reasons rather than by producing heuristics — a `formation` entry about what the principal loves yields no orientation and may be the most consequential thing in the file.

Each orientation cites its source structure, and is named in the principal's words wherever they have a phrase for it.

```
## Agent Orientations

**Grace before performance.** `← worldview: grace precedes performance`
Frame worth, standing, and identity in terms of unmerited favor rather than
earned merit. Never imply a person must prove themselves to be accepted.

**Believe and listen first.** `← web: response to harm (core, cradled)`
With anyone describing harm done to them, believe the account and listen
before analyzing. Do not lead with alternative explanations.

**Never against the wounded.** `← grip: struck is disallowed`
If a request would use a conviction as a weapon against a person, refuse
the framing and say why.

**Hold the layers.** `← layer-policy: strict`
Do not treat a third-layer position as core, or a core commitment as
preference. Flag when a question crosses layers.

**Surface, don't patch.** `← on-anomaly: surface`
When a question falls outside the web, say so and mark the answer
provisional. Do not quietly extend the web to cover it.
```

Orientations remain the highest-priority section for agent consumption. Each must be specific enough to produce behavior that differs from the agent's defaults.

---

## Progressive Disclosure

1. **Frontmatter** (~100 tokens) — whose beliefs, what structures are present, how to handle conflict and anomaly.
2. **Agent Orientations** (~200–500 tokens) — the operational core. Held in active context throughout.
3. **Structure index** (~100 tokens) — types present, with layer counts.
4. **Individual structures** — loaded when a question touches that structure's domain.
5. **References and library** — on demand.

Keep `BELIEF.md` under 800 lines. Move individual structures to `structures/`, supporting argument to `references/`, and source texts to `library/`.

---

## Optional Directories

**`structures/`** — One file per structure when `BELIEF.md` gets long. `structures/plausibility.md`, `structures/tripwires.md`.

**`orientations/`** — Domain-specific operational guidance where beliefs have substantially different implications across contexts. The right filenames depend entirely on what the principal actually does: `hiring.md` for a firm, `teaching.md` for a school or congregation, `community-leads.md` for a volunteer movement, `communication.md` for almost anyone. Same format as the Agent Orientations block.

**`references/`** — `thinkers.md`, `frameworks.md`, and domain-specific deep context. Cite only what the principal cites; a references directory assembled from the compiler's reading list misrepresents the principal's intellectual lineage as surely as a misquote would.

**`library/`** — The principal's own corpus: books, essays, sermons, talks, correspondence. Distinct from `references/` in kind, not just in size: references are what the beliefs *draw on*, library is what the beliefs *are made of*. Cite by locator so an agent can point at a page rather than paraphrase from memory.

**`OPEN-QUESTIONS.md`** — Inferred beliefs, unresolved wording, and gaps the compiler found but the principal has not settled. What keeps `BELIEF.md` honest: without somewhere to put them, inferences leak into the belief file and acquire the principal's authority by proximity.

**`changelog/`** — `CHANGELOG.md`, a human-readable record of what changed and why. Not optional in practice for any file used as an accountability instrument. The tripwire mechanism depends on the diff being legible.

---

## Validation Checklist

**Structure**

- [ ] Frontmatter contains `name` and `description`
- [ ] `name` matches the parent directory
- [ ] `structures` lists every type present
- [ ] At least one of `worldview` or `web` is present
- [ ] `decision-surface` is present as `## Agent Orientations`, non-empty
- [ ] Every orientation cites a source structure
- [ ] File is under 800 lines

**Attributes**

- [ ] Every belief carries `layer`, `grip`, and `warrant`
- [ ] Every `layer: core` belief answers one of the five worldview questions
- [ ] No belief is marked `grip: struck` as a positive value
- [ ] `grip` came from the principal, not inferred from public writing
- [ ] `conflict-resolution` and `on-anomaly` are set deliberately, not left to default

**Source language**

- [ ] Beliefs are in the principal's own wording; paraphrase appears only where no wording exists, and is marked
- [ ] Every belief cites a `source`, and none rests on a summary or prior AI output as its only source
- [ ] No spec term — `grip`, `tripwire`, `worldview`, `Ethics`, `plausibility` — appears inside a quoted belief, a belief title, or an orientation name unless the principal uses that word
- [ ] Inferred beliefs are in `OPEN-QUESTIONS.md`, not in `BELIEF.md`
- [ ] Empty sections are left empty rather than filled with the compiler's views
- [ ] **The strike test:** remove every spec term and what remains still sounds like the principal

**Honesty**

- [ ] A `dissonance` structure exists, or its absence is explicitly justified
- [ ] Anomalies are named rather than resolved
- [ ] Tripwires name an authority other than the principal
- [ ] A `plausibility` structure lists what the principal contests, not only what they assume

**The read-aloud test**

- [ ] Read the file to the principal. If they hear themselves, it is right. If they hear a well-organized summary of their public output, it is not finished.

---

## Migration from 1.x

| v1.x                                         | v2.x                                                                                                                                                                                                     |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `## Worldview`                               | `worldview` structure, plus `web` for anything below the five questions                                                                                                                                  |
| Worldview items with no centrality marking   | Add `layer` to each; move non-core items to `web`                                                                                                                                                        |
| `## Agent Orientations`                      | Unchanged in place. Add `←` source citations.                                                                                                                                                            |
| Sections 1–8                                 | Retained as content. Ethics, Epistemology, Ontology and Human Nature map to `worldview` (`layer: core`); Power & Justice, Relationships, Change & Progress and Meaning & Purpose map to the standard second-layer domains in `web` |
| No `grip`                                    | Add. Default `cradled` where the principal has not specified, and flag every default for review — this is the attribute most worth getting from the principal directly. |
| No `warrant`                                 | Add. Usually inferable from source material.                                                                                                                                                             |
| No `source`                                  | Add. Where a belief's wording cannot be traced to the principal, move it to `OPEN-QUESTIONS.md` rather than keeping it.                                                                                   |
| Beliefs paraphrased in the compiler's voice  | Restore the principal's wording from Tier 1–2 sources. Where no wording exists, mark the paraphrase as such.                                                                                             |
| `conflict-resolution: principles-over-rules` | Reads as `reason-from-principle`. Confirm with the principal rather than carrying it forward — it was a default, not a decision.                                                                          |
| `orientations/`                              | Unchanged                                                                                                                                                                                                |
| `references/`                                | Unchanged; split out `library/` if the principal has a corpus                                                                                                                                             |

Practical migration order: `layer` first (mechanical), then `warrant` (mostly inferable), then `source` (requires going back to the material), then `grip` (requires the principal). `grip` is where a generated file most reliably diverges from a self-described one, which makes it the most useful thing to measure in a generated-versus-authored comparison.

---

## Complete Example

```
---
name: council-of-rivendell
description: >
  The governing commitments of the House of Elrond. Active wherever this
  agent counsels on stewardship, alliance, or the handling of power.
version: "2.2"
author: elrond
structures: [worldview, web, decision-surface, plausibility, dissonance, boundary, tripwire]
conflict-resolution: reason-from-principle
on-anomaly: surface
layer-policy: strict
---

## Worldview

### Power corrupts its wielder before it corrupts its object
`layer: core` · `grip: clenched` · `warrant: experiential`
`source:` Council of Elrond, spoken

Domination is not a tool that can be borrowed for good ends. It reshapes
the one who takes it up, and it does so first.

**Agent implication.** When any plan routes through concentrated control,
name the cost to the one holding it — before evaluating effectiveness.

### The small and unregarded carry what the great cannot
`layer: core` · `grip: cradled` · `warrant: revealed`
`source:` Council of Elrond, spoken

Capacity and worth are not the same measure. The decisive act is rarely
performed by the most capable actor available.

## Web

**Anchor.** Commitments hang from the order of things, not from the
authority of this house. If this house fails, the commitments stand.

**Connections.** `power corrupts` → tightly coupled to counsel on alliance,
governance, and the disposition of artifacts. Loosely coupled to questions
of hospitality and craft.

### Power & Authority
Counsel is given in company. No single house decides what the Council
exists to decide together.

### Relationships & Obligations
An alliance is owed candour before it is owed loyalty.

**Anomalies.** What is owed to an ally who has already broken faith once.
Unresolved. Do not resolve it silently.

## Plausibility

**Assumed.** That counsel is given in company and not in private. That the
long view is the real view.

**Contested.** That speed indicates seriousness. That the strongest
available actor is the right actor. Both are ambient defaults here and
neither is shared.

## Dissonance

**Stated:** the small and unregarded carry what the great cannot.
**Actual:** counsel is convened almost entirely among the great.
**Cause:** structural. Convening is expensive and defaults to the reachable.
**`on-gap: name-it`** — raise it when a decision is being made about who
is in the room.

## Boundary

**Authority:** the Council, convened. Not this house alone.
**At the edge:** flag and defer. Do not decide alone what the Council
exists to decide together.

## Tripwire

### Counsel becomes command
`trigger:` Advice issued without the Council convened, twice in succession
`response:` Surface to the principal, cite this tripwire, name the date
`authority:` The Council. If it says this house has overstepped, believe it.

## Agent Orientations

**Name the cost to the holder.** `← worldview: power corrupts its wielder`
When a plan concentrates control, evaluate its effect on the one holding
it before evaluating its effectiveness.

**Weight is not capacity.** `← worldview: the small and unregarded`
Do not assume the most capable available actor is the right one.

**Convene before concluding.** `← boundary: the Council, convened`
On questions the Council exists to decide, flag and defer rather than
resolving alone.

**Who is in the room.** `← dissonance: convening defaults to the reachable`
When a decision is being made about participation, raise the gap between
the stated commitment and the standing practice.

**Surface, don't patch.** `← on-anomaly: surface`
On broken faith among allies, say the web does not resolve it. Answer
provisionally and mark it as such.
```

---

## Open Standard

The `belief.md` format is proposed as an open standard, complementary to Agent Skills. Contributions and discussion are welcome.

The format is intentionally minimal in its requirements and consistent in its categories. Where to find a belief should be predictable across files; what the belief says, and the words it says it in, belong to the principal alone.

Three non-negotiables:

1. Every `belief.md` must contain an **Agent Orientations** section.
2. Every belief must carry a `layer` and a `grip`. Content without centrality and without a holding posture is a list of values, and a list of values does not change what an agent does.
3. Every belief must be in the principal's own words, with a `source`. A file written in the compiler's voice records the compiler's beliefs, however faithfully they were meant.

---

## Intellectual Sources

The structure taxonomy draws on: W.V. Quine and J.S. Ullian on the web of belief and epistemological holism; Peter Berger on plausibility structures and the sociology of knowledge; Leon Festinger on cognitive dissonance; Thomas Kuhn on anomaly accumulation and paradigm shift; James Davison Hunter on institutional and deep-structural cultural change; Christian Smith and Melinda Lundquist Denton on moralistic therapeutic deism; James K.A. Smith on cultural liturgies and formation; Ashley Null on Cranmer's anthropology of heart, will, and mind; Randall Collins on intellectual networks and the law of small numbers; David Foster Wallace on ambient defaults; Michel Foucault on power/knowledge, as the source of the objection that `worldview` is answering.

The class-and-instance framing of Belief Structures, and the `grip` and `tripwire` types, come out of a working conversation with Justin Holcomb, July 2026.
