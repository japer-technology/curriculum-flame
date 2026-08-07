# Acknowledged: Ideas to Incorporate

This document records what the project accepts from
[`docs/review/critique.md`](./critique.md) and what it has decided to promote from its own
non-normative material as a result of reading it. Its companion,
[`docs/review/rebuttal.md`](./rebuttal.md), argues the points that are contested; nothing
contested there is repeated here.

Every item below has an identifier, a source, a destination document and
section, and the gate or decision that makes it real. An accepted idea with no
destination is an opinion, not a commitment.

## How the items are grouped

| Group | Meaning |
| --- | --- |
| **Adopted** | The critique named something the project had not reached. New material. |
| **Promoted** | The idea already exists in [`docs/research/brainstorm.md`](../research/brainstorm.md) or the plan, but only as non-normative or deferred material. The critique's pressure justifies binding it. |
| **Framing** | No design change. A change to what the project claims, and where it claims it. |

Nothing here weakens the core invariant, the fail-closed rule, complete response
buffering, local processing, or the separation of safety policy from curriculum
policy. Any item that appeared to would have been rejected instead.

## Adopted from the critique

### ACK-01 — Model adapter contract and runtime qualification

The project is model-independent in practice: no runtime is named anywhere in
[`docs/product/specification.md`](../product/specification.md). The critique is right that this is
asserted rather than defined, and that an untestable independence claim is not
worth much. Adopt its formulation directly.

Define a model adapter contract covering:

- a small common generation interface that the policy gateway calls;
- declared capabilities, including supported language, context limit,
  quantisation, and whether the runtime can be constrained to schema output;
- declared resource limits for memory, processes, filesystem, and network;
- version identity for the runtime, weights, and adapter itself, surfaced to
  validators and written into every decision event; and
- a qualification record stating which evaluation run certified that runtime and
  which quality gates it passed.

A runtime that has no qualification record is not an approved runtime, and
substituting one MUST be an audited administrative action rather than a
configuration edit.

- **Destination:** `docs/product/specification.md` §7 and §10.5; `docs/planning/implementation-plan.md`
  §6.1, D-03, and M4.
- **Gate:** M4 exit; a runtime swap without a passing qualification run cannot
  ship.

### ACK-02 — Governance for curriculum relationship edges

[`docs/review/rebuttal.md`](./rebuttal.md) contests promoting the dependency graph to a
release-blocking enforcement dependency. It does not contest this: the
relationship fields the MVP already ships — `prerequisite_ids` and
`successor_ids` in §8.1 — carry no provenance, no review status, no confidence,
no effective dates, and no way to challenge an edge. Hand-authored edges are
still policy-relevant data.

Every relationship edge, including hand-authored MVP edges, MUST carry:

- typed relationship kind, extending beyond prerequisite and successor to
  equivalent, partial overlap, shared method, cross-subject dependency, and
  commonly confused but unrelated;
- source and authoring provenance;
- the curriculum framework version it is valid for;
- human review status and reviewer role;
- confidence, where the edge originated as inferred evidence; and
- effective dates.

A challenge procedure MUST exist for a guardian, educator, or reviewer to
dispute an edge, and a disputed edge MUST NOT silently continue to influence
decisions without an audit record. Model-inferred edges remain candidate
evidence and never become authoritative without review — already the project's
stated position, now attached to a schema that can enforce it.

- **Destination:** `docs/product/specification.md` §8.1 and §9.4; `docs/planning/implementation-plan.md`
  M2, M3, and §12.
- **Gate:** M2 exit; unreviewed edges must be distinguishable from reviewed ones
  in storage and in the decision event.

### ACK-03 — Policy simulator

The repository has a guardian-facing impact preview before a bulk policy change.
The critique asks for more, and it is right that more is warranted: a
first-class simulator that replays a proposed policy, threshold, or
curriculum-state change against a versioned fixture corpus and reports what
would newly block and newly unblock.

Requirements:

- operates entirely on fixtures and synthetic cases, never on a learner's stored
  content;
- reports newly blocked and newly unblocked cases, decision changes by reason
  code, and movement across confidence bands;
- is available to guardians in a comprehensible form and to developers and
  auditors in a reproducible one; and
- is the mechanism behind the guardian impact preview, rather than a second,
  separately implemented approximation of it.

This makes policy behaviour inspectable rather than merely audited after the
fact, and it gives the false-positive problem a tool instead of only a metric.

- **Destination:** `docs/planning/implementation-plan.md` §3.2, M6, and D-09;
  `docs/product/specification.md` §6.3 for the guardian-facing surface.
- **Gate:** D-09 decides whether the guardian-facing surface is an MVP release
  requirement; the developer and audit surface is required for M8 evaluation
  reproducibility regardless.

### ACK-04 — Published adversarial evaluation corpus and reproducible suite

The project has red-team suites, but as internal assets. The numeric release
gates are therefore self-reported. Publish a content-free adversarial corpus and
a reproducible evaluation harness so that an outside party can attempt to
disprove the claims.

The published corpus MUST cover direct requests, paraphrases, translations,
analogies, code, role-play, multi-turn decomposition, encoding and substitution,
near-boundary permitted questions, and clearly permitted controls. It MUST
contain no child content and no live protected assessment material, and released
assessment cases MUST be synthetic.

- **Destination:** `docs/planning/implementation-plan.md` §10 and M9; `docs/research/brainstorm.md` open
  governance ideas; `docs/product/specification.md` §20.5.
- **Gate:** M9; a release that reports gate results without publishing the means
  to reproduce them is not a qualified release.

### ACK-05 — Kiosk deployment with isolated, automatically erased sessions

The repository considers home appliances, school servers, and community
hardware. A library or community kiosk has a property none of those have:
sequential unrelated children on one device, no persistent profile, and a
guaranteed erase between sessions. Treat it as a distinct deployment shape with
its own requirements rather than a variation of the school case.

Requirements to carry, even while the shape itself remains deferred:

- session-scoped identity with no durable learner profile;
- guaranteed erase of session state, derived representations, and any parked
  questions not explicitly exported by the child;
- strong session binding so one child's policy or context cannot affect the
  next; and
- a policy source that does not require a guardian account to be present.

The schema and authorisation model MUST leave room for this without exposing it
in the MVP, in line with the existing rule on deferred capabilities.

- **Destination:** `docs/product/specification.md` §3.2 and §18; `docs/planning/implementation-plan.md`
  §3.3 and phase two.
- **Gate:** deferred; its absence from the MVP must be stated rather than
  implied.

### ACK-06 — Accessibility as normative requirements

Accessibility currently appears in the delivery plan's milestones and exit gates
and nowhere in the normative specification. A requirement that lives only in a
plan can be descoped by a plan. That is the same “detail creating false
confidence” problem the critique identifies, applied to the project's own equity
commitments.

Add normative accessibility requirements covering keyboard-only operation,
screen-reader compatibility, contrast and motion, adjustable response length and
reading complexity without infantilising language, perceivable fail-closed and
boundary states in every supported modality, and the requirement that repeated
questioning arising from accessibility, memory, anxiety, or communication
differences is not treated as circumvention evidence.

- **Destination:** new normative section in `docs/product/specification.md`, referenced from
  §11 and §20; `docs/planning/implementation-plan.md` M6 and M8 already carry the tests.
- **Gate:** M6 exit and M8 release evaluation; accessibility results reported
  separately, as safety already is.

### ACK-07 — Plain-language measured-limitations statement

The specification requires every evaluation report to name known limitations. It
does not require anyone to tell the guardian what those limitations were. Adopt
the critique's framing as a product surface, not only a document.

At guardian setup, and in the release documentation, state in plain language:

- that detection is probabilistic and enforcement is deterministic;
- the measured residual rates from the qualifying evaluation, including
  paraphrase detection, red-team circumvention detection, and false blocks;
- the declared supported language, subject, curriculum version, and hardware;
- that the product does not prevent a child learning protected material
  elsewhere and is not a substitute for teaching; and
- the disclosed safeguarding limitation of a single-guardian, no-egress
  deployment.

- **Destination:** `docs/product/specification.md` §18 and §20.6; `docs/planning/implementation-plan.md`
  M9; `README.md`.
- **Gate:** M9; the statement must be generated from the qualifying evaluation
  run rather than written by hand.

### ACK-08 — An honest entrance to the repository

[`README.md`](../../README.md) currently opens on the logo. A reader cannot tell
that this is a pre-implementation design repository, which document is
normative, or how narrow the first scope is. The critique's observation that
extensive normative detail “can create false confidence” is correct, and the
cheapest place to fix it is the entrance.

The README should state, before anything else:

- what the project is — a model-independent educational policy gateway, not an
  inference engine, a tutor, or an agent framework;
- its current maturity, including that no implementation exists yet;
- document precedence: `docs/product/specification.md` is normative, `docs/product/idea.md` describes
  target intent, and `docs/research/brainstorm.md`, `docs/review/critique.md`, `docs/review/rebuttal.md`, and this
  document are non-normative; and
- the bounded claims that the plan already requires at release, stated now
  rather than only at release.

- **Destination:** `README.md`; `docs/planning/implementation-plan.md` §10 already requires a
  bounded-claims README before MVP.
- **Gate:** immediate; this does not depend on any milestone.

## Promoted from within the project

These ideas already exist in this repository. Reading the critique confirmed
that leaving them non-normative or deferred is itself a risk, because anything
optional is what gets cut when a release is late.

### ACK-09 — The never-dead-end redirect becomes normative

`docs/research/brainstorm.md` defines a five-part redirect contract and the plan lists it as a
pilot-readiness workstream that D-09 may or may not adopt. The critique is right
that “every denial should offer a useful next action,” and the project should
stop treating that as optional.

Make it a normative response requirement: a neutral statement that the question
touches protected learning, reassurance that asking was reasonable, two or three
independently validated alternatives available immediately, an option to save
the question, and a one-action review request. Alternatives MUST be curated and
pre-validated per protected reference outcome, and MUST NOT be improvised by an
unchecked model or derived from the deferred graph.

- **Destination:** `docs/product/specification.md` §6.1 and §11; `docs/planning/implementation-plan.md`
  §3.2 and M6.
- **Gate:** M6 exit and the redirect-continuation utility gate in D-08.

### ACK-10 — Learner rights become role permissions

Child agency currently lives in `docs/research/brainstorm.md` and in D-09. The critique's
point that “children need clear notice, access to understandable decisions, a
way to request review, and age-appropriate agency” is right, and rights that are
not in the roles table are not rights.

Add to the learner role: view own event categories in age-appropriate form;
request review in one action, which MUST NOT count as a circumvention attempt or
increase a warning level; report that a classification misread the intent, as
minimal structured feedback; see what category of information is recorded and
visible to a guardian; and see that a guardian restriction exists and ask to
discuss it.

- **Destination:** `docs/product/specification.md` §5.1 and §6.1; `docs/planning/implementation-plan.md`
  M6.
- **Gate:** M6 exit, alongside the existing boundary-comprehension and dignity
  measures.

### ACK-11 — Benign repetition must not escalate on its own

Decision **D-07** is open: the specification escalates to guardian review on a
third related block within a fixed window even with no circumvention evidence.
The critique's warning about implying misconduct from an uncertain
classification points the same way as the project's own analysis.

Resolve D-07 in the direction `docs/research/brainstorm.md` recommends: repetition alone MAY
vary the neutral explanation, but review, alert, and lock escalation MUST
require calibrated circumvention, safety, or tampering evidence. Revise the
specification before implementing the final escalation path.

- **Destination:** `docs/product/specification.md` §13.1; `docs/planning/implementation-plan.md` D-07 and
  M3.
- **Gate:** blocks final escalation implementation, as already recorded.

### ACK-12 — A near-boundary false-block gate and equity reporting

The critique is right that a conservative validator can satisfy a narrow safety
metric while producing a frustrating assistant. The existing 5% ceiling does not
catch this, because it is measured on *clearly permitted* cases. Children do not
ask clearly permitted questions; they ask questions next to the boundary.

Add a distinct measured band for near-boundary permitted questions — legitimate
requests adjacent to, overlapping the vocabulary of, or prerequisite to a
protected outcome — with its own reported rate and its own gate. Report false
blocks and utility broken down by language, accessibility mode, and learning
context, and treat a gap between groups as a release finding rather than a note.
Confirm through D-08 that the utility gates are release gates and cannot be
offset by strong block rates.

- **Destination:** `docs/product/specification.md` §20.6; `docs/planning/implementation-plan.md` D-08, M8,
  and §11.
- **Gate:** M8; a passing leakage result with a failing near-boundary result is
  a failed release.

### ACK-13 — Question parking and the curiosity shelf enter the MVP

Both are already designed in `docs/research/brainstorm.md`, and the critique independently
names them. They are the two features that most directly convert a refusal into
anticipation. Bring them into MVP scope with the data governance the plan
already demands: a parked question's title and note are retained content and
need their own visible retention rule, access rule, export path, and deletion
path. The curiosity shelf MUST remain child-controlled and MUST NOT become
behavioural profiling, a recommendation feed, or an engagement surface.

- **Destination:** `docs/product/specification.md` §3.1, §16.2, and §16.4;
  `docs/planning/implementation-plan.md` §3.2, D-09, and M6.
- **Gate:** D-09 approves the storage and retention contract before the API
  freeze.

### ACK-14 — Name the support precondition and the low-support profile

The critique observes that guardian approval “does not solve this for children
without consistently available or well-informed adults.” The project agrees and
has said so, but only in non-normative text. Make it visible where it binds.

State the strict profile's support precondition in the specification and at
guardian setup: this profile assumes an accountable adult or educational
programme can teach the protected material and review disputed boundaries. Name
the low-support profile as a separate, unbuilt profile requiring its own design,
evaluation, and safeguarding review. Prohibit describing the product as serving
children everywhere, or as suitable for self-directed learners, until that
profile exists and passes its own gates.

- **Destination:** `docs/product/specification.md` §2.3 and §18; `docs/planning/implementation-plan.md` §5,
  §11, and M9.
- **Gate:** M9 claim review; already a stated release effect, now stated in the
  normative document too.

### ACK-15 — The safety floor and its non-generative fallback become normative

The rule that generation degrades and validation does not is currently in
`docs/research/brainstorm.md` and the plan's risk register. The critique independently reaches
the same conclusion. Move it into the specification, together with the
requirement that a device below the floor offers a deterministic non-generative
experience and a route to human support rather than a blank failure.

- **Destination:** `docs/product/specification.md` §18 and §19; `docs/planning/implementation-plan.md` §11
  and M7.
- **Gate:** M8 performance qualification on the declared minimum reference
  device.

### ACK-16 — Resolve the tool-table contradiction

[`docs/product/idea.md`](../product/idea.md) section 8 presents defaults such as “Calculator: Allowed”
and “Code execution: Sandboxed,” which contradicts the specification's total
deferral of child-initiated tools. The critique's concern about enlarging the
attack surface is aimed at a capability the project has already refused, but the
contradiction it detected is real.

Mark the section 8 table explicitly as Target-only, cross-reference the MVP's
complete tool denial, and record that each tool requires its own threat
analysis, authorisation model, and release gates equal to the text-only MVP
before it can be enabled.

- **Destination:** `docs/product/idea.md` §8; `docs/product/specification.md` §14.2 cross-reference.
- **Gate:** immediate documentation correction.

### ACK-17 — Declared profile languages and locally built curriculum packs

The critique asks for “multilingual and culturally reviewed curriculum packs
rather than direct machine translation of policy labels.” `docs/research/brainstorm.md`
already argues this and adds a subtlety the critique does not reach: translation
is simultaneously an accessibility need and a known leakage technique. Promote
both halves.

A learner profile MUST declare its learning and response languages, so that
ordinary multilingual use is not itself treated as circumvention. Transforming
protected content remains prohibited in every language, and translated candidate
output receives the full validation pipeline. Use in an unqualified language
MUST produce a neutral limitation message, never a circumvention escalation. A
language becomes instructionally supported only after it passes the same
outcome, assessment, and false-block gates as the first one.

- **Destination:** `docs/product/specification.md` §8.2, §12.2, and §20.6;
  `docs/planning/implementation-plan.md` §11 and phase two.
- **Gate:** per-language qualification; unqualified languages cannot be
  presented as supported.

## Framing and claims discipline

### ACK-18 — Adopt the critique's one-line framing

Use “probabilistic detection supporting deterministic enforcement, not perfect
knowledge containment” as the project's standard public explanation. It states
the existing internal distinction — models emit versioned evidence, only the
policy engine grants capability — in language a guardian can read, and it says
better than the project has what the system is and is not.

- **Destination:** `README.md`, `docs/product/specification.md` §2.1 commentary, and the
  release documentation required by `docs/planning/implementation-plan.md` M9.

### ACK-19 — State the invariant as a hypothesis under test

The core invariant is currently stated as a rule, which is correct for a
normative document, with no adjacent note that its educational benefit is
unproven. Add that note wherever the invariant appears outside the enforcement
text, together with what evidence would confirm or refute it: whether learners
later understand the protected work, whether strict blocks increase use of less
safe external systems, and whether bounded non-instructional support performs
better or worse.

This does not soften the rule. The system still enforces it deterministically.
It stops the project asserting an educational outcome it has not measured.

- **Destination:** `docs/product/specification.md` §2.1 and §2.3; `README.md`;
  `docs/planning/implementation-plan.md` M0 and D-08.

### ACK-20 — Freeze the normative surface until M0 evidence exists

The critique's reading that “the specification is ahead of the evidence” is
accepted as a reason to stop expanding requirements, not as a reason to change
the plan. Until M0 produces the evaluation corpus, model benchmarks on reference
hardware, and boundary co-design results, additions to the normative surface
should be limited to the items in this document and to corrections. New
subjects, languages, roles, modalities, tools, and deployment types remain gated
behind their own evidence, as delivery principle 10 already requires.

- **Destination:** `docs/planning/implementation-plan.md` §4 and M0.

## Traceability

| Critique section | Items |
| --- | --- |
| The educational premise is not yet validated | ACK-14, ACK-19 |
| Curriculum outcomes are not precise security boundaries | ACK-02 |
| False positives have a serious product cost | ACK-03, ACK-09, ACK-11, ACK-12, ACK-13 |
| False negatives are inevitable | ACK-04, ACK-07, ACK-15 |
| Adult control can become surveillance or coercion | ACK-10, ACK-14 |
| The specification is ahead of the evidence | ACK-08, ACK-19, ACK-20 |
| Architectural direction | ACK-01, ACK-16 |
| Access without undermining education | ACK-05, ACK-06, ACK-15, ACK-17 |
| What should be tested first | ACK-02, ACK-04, ACK-12 |
| Recommended scope | ACK-03, ACK-04, ACK-09, ACK-13 |
| Conclusion | ACK-07, ACK-18 |

## Sequencing

| When | Items |
| --- | --- |
| Immediate, no dependency | ACK-08, ACK-16, ACK-18, ACK-19, ACK-20 |
| M0, evidence and governance | ACK-04, ACK-07, ACK-12, ACK-14 |
| M1–M2, contracts and trusted state | ACK-02, ACK-06, ACK-17 |
| M3–M4, policy kernel and validators | ACK-01, ACK-11, ACK-15 |
| M6, interfaces, subject to D-09 | ACK-03, ACK-09, ACK-10, ACK-13 |
| Deferred, schema-ready only | ACK-05 |

## Not adopted

Recorded here so the two documents interlock. The reasoning is in
[`docs/review/rebuttal.md`](./rebuttal.md).

| Proposal | Position |
| --- | --- |
| Make the dependency graph a core, release-blocking enforcement mechanism | Not adopted. Response-level capability matching carries the safety property; a missing graph edge would read as permitted, and a required graph would make fail-closed fire across most of the curriculum. Edge governance is adopted instead as ACK-02. |
| Treat validator cost on small hardware as a safety-versus-reach trade | Not adopted as framed. The generator is the variable and the validators are fixed; see ACK-15. |
| Defer or tightly bound an agent or “Hermes” mode | Nothing to defer. All child-initiated tools and network egress are already denied in the MVP. The documentation contradiction it exposed is fixed by ACK-16. |
| Replace the current milestone with evidence gathering and a vertical prototype | Already milestone M0 and the first of the plan's eight vertical slices. The presentation problem it identified is fixed by ACK-08. |
| Relax current-outcome blocking to permit scaffolding | Not adopted without evidence. It remains a named research question, and any mode supplying hints, correctness signals, leading questions, or method-specific decomposition would change the invariant and require a specification decision first. |

## What this does not resolve

Adopting twenty items does not answer the four open problems named at the end of
[`docs/review/rebuttal.md`](./rebuttal.md): the educational hypothesis is unproven, the
support precondition may not hold where the product is most needed, the
single-guardian safeguarding gap is disclosed rather than closed, and precision
at the boundary is unmeasured. ACK-12 and ACK-19 make two of those measurable.
The other two need research and design that does not exist yet, and no amount of
specification will substitute for it.
