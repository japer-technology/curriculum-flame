# Rebuttal: A Response to `docs/review/critique.md`

This document answers [`docs/review/critique.md`](./critique.md). It is a companion to
[`docs/review/acknowledged.md`](./acknowledged.md), which records the critique's points that
the project accepts and where each one will be incorporated.

The two documents divide the work deliberately. This one argues. That one
commits. Where a section here concedes something, the corresponding commitment
appears there rather than being repeated.

## How this rebuttal reads the critique

The critique is careful, unusually well aimed, and correct about the project's
hardest problem: a curriculum outcome is a description of expected learning, not
a boundary that a classifier can trace. Nothing below disputes that.

The disagreement is narrower and mostly about **standing**. The critique is
written as though the repository consisted of [`docs/product/idea.md`](../product/idea.md) and
[`docs/product/specification.md`](../product/specification.md) alone. Roughly two thirds of its
recommendations already exist in [`docs/research/brainstorm.md`](../research/brainstorm.md) and
[`docs/planning/implementation-plan.md`](../planning/implementation-plan.md), in several cases stated
more sharply, with a named owner, a decision record, or a release consequence
attached. Presenting those as unaddressed weaknesses misdescribes the project's
position and, more importantly, wastes the critique's leverage: an outside
reviewer's scarcest contribution is the thing the project has genuinely not
thought of, and that contribution is buried here under restatement.

So this rebuttal does three things. It shows where the repository already holds
the position the critique recommends. It contests four claims on substance. It
identifies where the critique is right in a way the project had not reached, and
hands those to `docs/review/acknowledged.md`.

## Summary of the response

| Critique section | Response |
| --- | --- |
| The model does not govern itself | Agreed; already normative |
| The boundary moves with the learner | Agreed; already normative |
| Local-first operation is meaningful | Agreed; the project goes further |
| Protects capability, not keywords | Agreed; already a delivery principle |
| The educational premise is not yet validated | Agreed on substance; the claim that the project asserts otherwise is incorrect |
| Curriculum outcomes are not precise security boundaries | Agreed on the diagnosis; **contested** on the remedy |
| False positives have a serious product cost | Agreed; the critique understates what exists and understates the gate that is missing |
| False negatives are inevitable | Agreed; the hardware sub-claim is **contested** |
| Adult control can become surveillance or coercion | Agreed; the project already goes further, including a gap the critique omits |
| The specification is ahead of the evidence | Agreed on the reading, **contested** on the recommendation, which is already milestone M0 |
| Architectural direction | Agreed; already true and unnamed by design |
| Agent behaviour and “Hermes mode” | **Contested**: this argues against a design the project has not proposed |
| Access without undermining education | Mostly already present; two items are new |
| What should be tested first | Mostly already present; two items are new |
| Recommended scope | Already the declared MVP scope, plus two additions |

## Where the critique restates the project's own position

These are not disagreements. They are corrections to the claim that the project
needs to be told.

### On the model not governing itself

The critique proposes deterministic policy outside the generative model as “the
correct foundation.” It is already the first delivery principle:

> **Policy outside the model.** Models emit versioned evidence; only the
> deterministic policy engine grants or denies capability.

The specification requires the pre-generation decision, complete buffering (“No
candidate token may be displayed, spoken, sent to a tool, or otherwise released
before final validation”), post-generation validation of the whole candidate,
and seven enumerated fail-closed conditions. The critique's own pipeline diagram
omits two stages the project treats as essential: conversation-aware
circumvention detection before the decision, and committing the decision event
before display. The repository's pipeline is a superset of the one being
recommended to it.

### On protecting capability rather than keywords

Already principle 5: “Evaluate what an answer would teach, solve, check,
transform, or execute, including across turns and formats.” The complementary
principle 6 — least restrictive safe response — exists precisely to stop
capability matching collapsing into vocabulary matching, which is the failure
the critique warns about two sections later.

### On local-first operation

The critique credits the project for recognising that “local” does not guarantee
privacy. The project's position is stronger than that. Raw prompts and responses
are volatile by default. Transcript retention applies **prospectively only**, so
an adult cannot enable it later and recover a conversation that was never
stored. Structured events, not text, are the durable record, at 30 days by
default. The guardian dashboard is required to exclude obedience scores, emotion
or personality inference, comparisons between children, hidden sentiment
analysis, and engagement metrics. Those are not privacy aspirations; they are
exclusions with a release effect.

### On hardware degradation

The critique writes that “hardware tiers should degrade capability
transparently, never skip a required validator,” and that a device unable to
answer safely should fall back to something deterministic and non-generative.
That is already the rule, in the same words in spirit:

> If the full enforcement pipeline cannot meet its quality gates on a device,
> that device is below the supported safety floor. It may offer signed, curated
> activities, but must not run weaker child chat under the same product claim.

And in the risk register: “reduce generation, never validation; disable child
chat if enforcement cannot pass.”

### On making the permitted world compelling

The critique asks for creative writing, world-building, revision, future-topic
exploration, AI literacy, simulations, question parking, and curiosity shelves.
Every one of those appears in `docs/research/brainstorm.md`, most of them as named features
with design detail. The project also attaches something the critique does not
propose: a pre-pilot **go/no-go threshold** on useful permitted answers and
redirect continuation, whose failure pauses rollout. An aspiration to be
interesting is easy. A release gate on being interesting is the part that binds.

## Where the critique is contested

### 1. The dependency graph should not become a mandatory enforcement dependency

The critique's strongest paragraph is also the one this rebuttal most firmly
resists:

> The dependency graph is therefore part of the core safety mechanism, not an
> optional enhancement.

The diagnosis is right. Knowledge is an overlapping graph and curriculum
documents are not. The remedy does not follow, and adopting it would make the
system less safe rather than more.

The reason is that in this architecture the graph is not what carries the safety
property. The **complete candidate response** is. The specification requires the
whole buffered answer to be matched against outcomes, not merely the prompt, and
requires it to be checked for “analogy, story, summary, or translation that
reveals the method” and “code, calculator use, or another tool that performs the
protected method.” A future-physics answer that walks through the protected
algebra on its way to an orbital explanation is caught because the answer
contains the protected method — not because someone remembered to author an edge
from a physics outcome to an algebra outcome.

That distinction matters enormously under failure. Consider what happens when
the graph is incomplete, which it permanently will be:

- **If leakage detection depends on response content**, a missing edge costs
  recall on *routing* — the system offers a worse alternative, or misses a
  useful review suggestion. The protection still holds.
- **If leakage detection depends on the graph**, a missing edge reads as “no
  relationship,” which reads as *permitted*. The absence of human review work
  silently becomes an allow decision. This is exactly the failure mode the
  critique itself warns against elsewhere: “Model-inferred relationships may
  suggest candidates but must not silently become authoritative policy.”

Promoting the graph to a core safety mechanism also collides with fail-closed
behaviour. The specification denies model-derived content whenever a required
component is unavailable. If the graph were required, then every outcome without
reviewed edges would be an unavailable dependency, and the fail-closed rule
would fire across most of the curriculum. The result is not a safer product; it
is a product that refuses almost everything and is therefore abandoned — the
outcome the critique's own “false positives” section identifies as fatal.

The project's actual position, already written down, is that model-inferred
edges “remain candidate evidence until reviewed, versioned, and given
provenance,” and that graph-derived redirect routing may replace curated
templates “only after the graph and its suggestions have their own quality
gates.” The graph is a **recall amplifier and a routing aid under its own
gates**, deliberately deferred to phase three. It is not optional in the sense
of unimportant. It is optional in the sense that the safety argument must not
depend on its completeness.

What the critique is right about, and what is conceded in
[`docs/review/acknowledged.md`](./acknowledged.md), is that the relationship fields the MVP
*does* ship — `prerequisite_ids` and `successor_ids` — currently carry no
provenance, review status, confidence, effective dates, or challenge procedure.
Hand-authored edges are still policy-relevant data and deserve the governance
the critique describes. That correction is accepted in full. The escalation of
the graph to a release-blocking enforcement dependency is not.

### 2. The hardware objection is answered by shrinking the generator

The critique argues that “running several capable validators may exceed the
target hardware budget,” presenting this as an unresolved tension between safety
and reach.

It is resolved, and in the direction the critique would presumably want. The
project's rule is that the validation pipeline is fixed and the **generator** is
the variable: small quantised generation models and a reduced-generation mode
with explicit limits are supported, while “required validators, policy
enforcement, and release quality gates must not be reduced.” A device that
cannot run the full enforcement path does not get a weaker version of the
product; it gets no child chat, plus signed curated activities.

This inverts the usual trade. Most safety systems degrade the checks to keep the
feature. This one degrades the feature to keep the checks, and publishes a
minimum reference device so that the floor is a measured claim rather than a
marketing one. The critique's framing — validators as a budget problem — assumes
the generator's capability is fixed. It is not.

### 3. There is no agent mode to defer

The critique devotes a section to bounding agent behaviour and warns that a
“Hermes mode” should never be a trusted policy authority. No such mode is
proposed anywhere in this repository. The MVP disables **all** child-initiated
external tools, including the calculator. Network egress is denied outright
except during an authenticated update outside a child session. Browsing, shell,
messaging, file mutation, and autonomous execution are in the deferred list, and
each future tool already requires its own threat analysis and release gates
equal to the text-only MVP.

Arguing against a capability the project has already refused is not a critique;
it is a coincidence of agreement. But the instinct behind it detected something
real, and that is worth crediting: [`docs/product/idea.md`](../product/idea.md) section 8 presents a
tool table with defaults such as “Calculator: Allowed” and “Code execution:
Sandboxed,” which reads like a near-term commitment and contradicts the
specification's total deferral. That inconsistency is a genuine defect in the
documents, and it is accepted for correction.

### 4. “Evidence before more specification” is already milestone M0

The critique recommends that “the next milestone should be evidence gathering
and a narrow vertical prototype rather than further expansion of requirements.”

That is milestone M0, whose stated goal is to “retire the largest educational,
licensing, model-quality, hardware, and safeguarding risks **before committing
to a production architecture**,” and whose tasks include building a versioned
Years 5–8 mathematics evaluation corpus with held-out splits, benchmarking
candidate models on the minimum reference hardware with egress disabled, and
co-designing boundary language with children, educators, learning scientists,
safeguarding specialists, and disability advocates. The delivery plan then names
eight vertical slices, the first of which is “authenticated child request to
deterministic system-owned boundary response” — a narrow vertical prototype that
proves the trust boundary before any model is involved.

Where the critique lands a real hit is not on the plan but on the **shopfront**.
A reader arriving at [`README.md`](../../README.md) finds a meditation on the logo,
not a statement that this is a pre-implementation design repository with no
code, unvalidated educational assumptions, and a deliberately narrow first
scope. The detail *can* create false confidence, exactly as the critique says —
not because the plan is wrong, but because nothing at the entrance tells a
reader which document is normative, which is exploratory, and how much has been
built. That is accepted.

## Where the critique is right and had not been reached

Six items are genuinely additive. They are specified for incorporation in
[`docs/review/acknowledged.md`](./acknowledged.md) and summarised here so the argument and
the concession sit in the same document.

1. **A model adapter contract.** The project is model-independent in practice —
   no runtime is named anywhere in the specification — but independence is
   asserted rather than defined. The critique's formulation is precise and the
   project had not written it: adapters should expose “a small common contract,
   capability declarations, resource limits, and version identity.” Without it,
   model independence is a claim that cannot be tested and a swap that cannot be
   qualified.

2. **A policy simulator.** The repository has a guardian-facing “preview the
   child impact before a bulk policy change.” The critique asks for something
   larger: a first-class simulator that replays a proposed policy or
   curriculum-state change against a fixture corpus and reports what would newly
   block and unblock, with no child data involved. That is the difference
   between a warning dialog and a tool that makes policy behaviour inspectable
   to guardians, reviewers, and auditors alike.

3. **A published adversarial evaluation corpus.** The project has red-team
   suites, but as internal test assets. Publishing a content-free corpus turns
   the numeric release gates from self-reported figures into reproducible ones,
   and lets an outside party disprove them. A safety claim nobody else can test
   is a weaker claim.

4. **Kiosk deployment with isolated, automatically erased sessions.** The
   repository considers home, school, and community appliances, but a library or
   community kiosk has a property none of those have: sequential unrelated
   children on one device with no persistent profile and a guaranteed erase
   between sessions. That is a distinct set of requirements, not a variation on
   the school LAN case.

5. **Accessibility as a normative requirement.** Accessibility appears in the
   delivery plan's milestones and exit gates, but the specification — the
   normative document — does not address it at all. A requirement that lives
   only in a plan can be descoped by a plan. This is precisely the “detail
   creating false confidence” problem the critique names, applied to the
   project's own equity commitments.

6. **A plain-language limitations statement.** The specification requires every
   evaluation report to name known limitations. It does not require anyone to
   *tell the guardian* what the residual leakage and false-block rates were at
   setup time. The critique's insistence that claims be framed as “risk
   reduction with measured limitations” is right, and it should bind the product
   surface, not only the report.

## One correction the project had already made more precisely

The critique warns that repeated refusals can “teach children to phrase
questions adversarially” and that the system “should never shame curiosity or
imply misconduct from an uncertain classification.”

The project agrees, and has already located the offending rule rather than the
principle. The specification currently escalates to guardian review on a third
related block within a fixed window even where no circumvention evidence exists.
`docs/research/brainstorm.md` names this, argues that repetition alone may vary a neutral
explanation but must not by itself trigger review or lock escalation, and flags
its particular cost for younger children, multilingual learners, and children
whose disability or anxiety affects repeated questioning. The delivery plan
carries it as decision **D-07**, blocking final escalation implementation, and
requires the specification to be revised first if the answer changes.

This is offered not as a rebuttal point but as evidence for the reading at the
top of this document: on several axes the project's internal critique is already
working a level of detail finer than the external one. The most useful outside
review would start from that level.

## What remains genuinely unresolved

The critique should not be answered as though the project has everything in
hand. Four problems are open, and none of them is solved by any document in this
repository.

- **The educational hypothesis is unproven.** Whether withholding instruction on
  a current outcome produces better independent learning than bounded
  non-instructional support is an empirical question with no answer here. The
  project's position is that it must be studied before it is claimed, not that
  it has been settled.
- **The support precondition may not hold where the product is most needed.**
  The strict profile assumes an accountable adult or educational programme can
  provide the protected instruction. A child without that support is exactly the
  child a curriculum-blocking assistant serves worst. A separately specified
  low-support profile is named as a research prerequisite, but it does not
  exist.
- **The single-guardian safeguarding gap is real and disclosed, not closed.** A
  local, no-egress deployment with one configured adult has no independent route
  to help when that adult is the source of danger. The project's answer today is
  to disclose the limit and refuse to present it as a safeguard. That is honest.
  It is not a solution.
- **Precision at the boundary is unmeasured.** The 5% ceiling on false blocks is
  measured against *clearly permitted* cases. Children do not live there. The
  near-boundary band — legitimate questions that sit adjacent to a protected
  outcome — is where the product's usefulness will actually be decided, and
  there is currently no gate on it at all.

## Conclusion

The critique's closing line is the right one, and the project adopts it as its
standard public framing: probabilistic detection supporting deterministic
enforcement, not perfect knowledge containment. The repository already draws
that distinction internally — models emit versioned evidence, only the policy
engine grants capability — but the critique states it better than the project
has, and in language a guardian can read.

Where this rebuttal pushes back, it pushes back on one recurring move: treating
a diagnosis as if it settled the remedy. Curriculum outcomes are not clean
boundaries, and the answer is response-level capability matching with a governed
graph as an amplifier, not a graph promoted to a release-blocking dependency.
Validators are expensive on small hardware, and the answer is a smaller
generator, not fewer checks. The specification is ahead of the evidence, and the
answer is an honest entrance and an evidence-first milestone, not a slower plan.

What the critique gets right is worth more than what it restates. Six additive
items, two concessions made inside the disagreements above, three framing
corrections, and a sharper account of what the project has not proven are now
recorded in [`docs/review/acknowledged.md`](./acknowledged.md) with a destination for each.
The disagreements are offered in the same spirit: the boundary between a learner
and a capable model is worth arguing about precisely, because a vague version of
it will protect nothing and permit nothing.
