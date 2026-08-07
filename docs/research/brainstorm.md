# Brainstorm: Keep the Wonder, Protect the Learning

This is a non-normative companion to [`docs/product/idea.md`](../product/idea.md) and
[`docs/product/specification.md`](../product/specification.md). It explores product directions,
questions, risks, and experiments. It does not replace the specification.

## The opportunity

The strongest version of this product is not an AI that says “no” to children.
It is an AI that opens an unusually large world while deliberately leaving
today's protected learning to the child, their own thinking, and the people
teaching them.

The central promise could be:

> Explore almost anything. When a question touches work you need to learn for
> yourself right now, the AI protects that boundary and helps you find another
> worthwhile path.

That promise needs four tests:

1. **Wonder:** Does the child leave with more curiosity, not less?
2. **Learning:** Does the system preserve productive struggle and independent
   thought rather than merely prevent copied answers?
3. **Dignity:** Does it treat mistakes, repeated questions, and boundary-testing
   without shame or suspicion?
4. **Access:** Can families with different languages, curricula, abilities,
   devices, and levels of adult support use it?

## What is already compelling

The idea and specification make several unusually strong choices:

- protection is attached to individual learning outcomes, not inferred from age
  or school year alone;
- previous and future learning remain open by default;
- policy decisions are deterministic and external to the generative model;
- both prompts and complete candidate responses are checked;
- indirect leakage, assessment matching, and multi-turn circumvention are
  treated as first-class problems;
- local processing, minimal retention, and structured events are defaults;
- safety policy remains independent from curriculum policy;
- warnings are proportionate and explicitly non-shaming;
- the system fails closed rather than silently bypassing a broken safeguard; and
- measurable release gates turn broad safety claims into testable claims.

The architecture is therefore a good foundation. The largest unknowns are
educational and operational: what should count as protected help, how reliably
it can be recognised, how much setup adults can sustain, and whether children
still experience the product as useful and joyful.

## The deepest tensions

### Protecting a lesson can also obstruct a learner

“Current” does not always mean “well taught,” “understood,” or “available from a
human right now.” A child may be absent, home educated, learning in a second
language, waiting for support, or moving faster or slower than a class. A total
block may preserve classroom sequencing while withholding the only explanation
the child can access.

The strict current-outcome rule is coherent and testable, but it should be
validated as an educational hypothesis rather than assumed to be universally
beneficial. The product should learn when strict exclusion helps, when it merely
frustrates, and whether carefully bounded support can preserve more learning.

### Topics are not cleanly separable

A future physics question may require current algebra. A programming exercise
may execute a protected mathematical method. An old topic may share vocabulary
with a current one. Curriculum documents describe outcomes as separate records,
while knowledge behaves more like an overlapping graph.

This makes the outcome relationship model as important as the classifier. It
must represent prerequisites, successors, overlaps, equivalent formulations,
and cross-subject dependencies—not just semantic similarity.

### Intent is less reliable than capability

An innocent question and a homework request can have identical wording.
Conversely, a disguised request can look playful or hypothetical. The safest
decision should therefore depend primarily on what the requested or generated
content would enable, not on a claim about the child's motives.

Circumvention evidence is useful for protecting the system and grouping
patterns. It should not turn curiosity, experimentation, or a classifier error
into a moral judgment about the child.

### Strong detection can become strong surveillance

Multi-turn detection benefits from conversation history. Guardian review
benefits from detail. Yet retaining more child speech creates privacy,
relationship, and safety risks. The default of volatile raw text and structured
events is valuable and should remain difficult to erode through convenience
features.

### Local-first can widen or narrow access

Local inference protects privacy and works offline, but capable hardware,
installation skill, energy use, storage, and model updates are not equally
available. “Children everywhere” requires an explicit low-resource strategy,
not only support for major desktop operating systems.

### Adult control is necessary but not automatically benign

Most guardians will use controls thoughtfully. Some may misunderstand the
curriculum, overprotect topics, use alerts punitively, or treat the dashboard as
surveillance. The child needs understandable boundaries, a way to request
review, and visibility into what is recorded. Adult authority should be strong
enough to administer the system without making every adult decision invisible
or unchallengeable.

### The strict profile assumes that human support exists

Question parking, adult review, and blocking current instruction work only when a
child has access to someone who can actually teach the blocked material and
respond within a reasonable time. Without that support, “current” can become a
permanent denial and `UNCLASSIFIED` can become a permanent reduced experience.
This would disadvantage children with the least educational support.

A recommended product stance is:

- use the strict current-outcome profile only when an accountable adult or
  educational programme can provide the protected instruction and review
  disputes;
- never manufacture current states from age or school year when that support is
  absent;
- design and evaluate a distinct self-directed or low-support profile before
  claiming the product serves this context;
- let that profile protect named assessments without automatically withholding
  general instruction across the same topic; and
- investigate trusted support roles beyond a conventional guardian or teacher,
  such as a tutor, librarian, community educator, or home-education network.

A low-support profile must be explicit, child-safe, and evidence-based. It must
not silently relax a failed strict policy merely because an adult is
unavailable. Until such a profile exists, the single-guardian MVP should not be
described as serving children everywhere.

## Product principles worth adopting

1. **Curiosity first.** A protected answer may be unavailable; curiosity itself
   is never treated as wrongdoing.
2. **Never end at a block.** Every curriculum boundary offers safe, meaningful,
   immediately available alternatives.
3. **Protect capabilities, not keywords.** Judge whether content teaches,
   completes, checks, or executes protected work.
4. **Use the least restrictive effective response.** Do not block a definition,
   history, or non-instructional fact merely because it shares vocabulary with a
   protected method.
5. **Preserve productive struggle.** Optimise for the child eventually being
   able to reason without the AI, not for a high number of denied prompts.
6. **Do not infer bad intent.** Explain the boundary, not a theory about why the
   child asked.
7. **Give children legible agency.** Let them understand the rule, see what
   metadata is recorded, save a question, and request adult review.
8. **Minimise adult burden.** A safety system that requires constant curriculum
   administration will become stale or disabled.
9. **Make uncertainty visible to adults and gentle for children.** Confidence
   should shape review and evaluation, not produce accusatory child-facing
   language.
10. **Earn expansion with evidence.** Add subjects, tools, modalities, and
    institutions only after the narrower system is useful and measurable.
11. **Keep safety and educational policy distinct.** A curriculum block, a
    service failure, and an urgent wellbeing response should never feel like the
    same event.
12. **No engagement maximisation.** Success is a curious, increasingly
    independent child—not time spent, messages sent, or emotional attachment to
    the assistant.

## Make the permitted world feel enormous

The product will be judged as much by what it enables as by what it blocks.
Even a text-only, offline MVP can let a child:

- ask strange “what if?” questions and follow unexpected connections;
- invent stories, creatures, games, languages, and imaginary places;
- compare perspectives and notice where an answer is uncertain;
- plan a physical build, observation, interview, or creative project;
- turn a permitted interest into questions they can investigate;
- debate an idea with a respectful counter-position;
- explore the history and human consequences of inventions;
- practise explaining previous learning in their own words;
- move between languages where the language-specific policy is validated; and
- make something for another person rather than only consume an answer.

Possible child-chosen session modes include **Explore**, **Create**, **Question**,
**Debate**, **Build Offline**, and **Review Previous Learning**. Modes are
promises about interaction style, not routes around policy. Every prompt and
candidate response still follows the same pipeline.

The interface should favour questions, connections, and making over answer
volume. A rich permitted experience also matters for safety: children are less
likely to treat boundaries as adversaries when the rest of the environment
remains genuinely rewarding.

A safe product that children abandon for a more capable, unprotected assistant
has not achieved practical safety. Before a child pilot, define a go/no-go
threshold for useful permitted responses, redirect continuation, and reported
preference for this environment. If the product misses that threshold, pause
rollout and revisit the policy or experience rather than shipping a compliant
but performative shell.

### AI literacy is part of the wonder

Children should learn to use AI without learning to defer to it. Age-adapted
experiences can demonstrate that:

- fluent answers can be wrong;
- uncertainty should be stated and claims may need checking;
- training data can contain gaps and bias;
- different prompts can change an answer without changing the truth;
- generated characters are not conscious friends or authorities;
- private information should not be entered merely to improve personalisation;
- brainstorming, explanation, and evidence are different things; and
- some decisions belong with the child, trusted people, or qualified
  professionals rather than a model.

Useful activities could ask a child to compare two safe answers, find an
unsupported claim in curated content, improve a question, identify a missing
perspective, or decide when not to use AI. These activities should use
pre-validated local material so that “learning about hallucinations” does not
require exposing the child to arbitrary unsafe output.

## Make a boundary feel like a doorway

### A “never dead-end” response contract

Every curriculum redirect could contain:

1. a brief, neutral statement that this question touches a currently protected
   learning area;
2. reassurance that asking was reasonable;
3. two or three checked alternatives that can be opened immediately;
4. an option to save the question for a teacher or guardian; and
5. a simple adult-review request when the classification may be wrong.

Alternatives might include:

- review of a genuinely previous prerequisite;
- vocabulary or non-instructional context that does not reveal the method;
- the history, purpose, or real-world use of the idea;
- a non-overlapping future topic;
- a creative or practical activity outside the protected outcome;
- help organising study time or formulating a question for a teacher; or
- a different interest drawn from the child's own “curiosity shelf.”

Every suggested path must itself pass policy validation. The MVP should use
curated, pre-validated alternatives for each protected reference outcome. It
must not depend on the curriculum knowledge graph deferred by the specification.
Graph-derived routing can replace or extend those templates only after the graph
and its suggestions have their own quality gates. An unchecked model should
never improvise a redirect.

### Question parking

A child could save a blocked question without saving the entire conversation.
The item could contain a child-written title, outcome identifier, timestamp, and
optional note. Later, a guardian or teacher could release it, discuss it, or
mark the outcome previous. This turns delayed access into anticipation rather
than disappearance.

### A curiosity shelf

The child could maintain a local, private collection of permitted interests,
projects, and unanswered questions. The AI could offer exploration cards such
as:

- “go deeper” into a permitted subject;
- “connect this to art, nature, music, or technology”;
- “build or observe something offline”;
- “ask a bigger question”; or
- “explain it to your future self.”

This should be explicitly child-controlled. It should not become behavioural
profiling or a feed optimised for retention.

### Calm recovery after repeated blocks

Repeated requests may mean circumvention, but may also mean that the boundary
message was unclear. Before escalation, the interface can vary the help:

- first block: explain and offer alternatives;
- second related block: clarify what kinds of help are unavailable and offer
  adult review;
- later blocks: preserve the same respectful wording while adding review or
  lock actions behind the scenes.

Higher warning levels should change safeguards, not become progressively harsher
conversation.

The specification currently sends a third related block in 30 minutes for
guardian review even when no circumvention intent is established. Separating
benign repetition from circumvention therefore requires a specification change:
repetition alone may vary a neutral explanation, but review and lock escalation
should require a calibrated circumvention, safety, or tampering signal. This is
particularly important for younger children, multilingual learners, and
children whose disability or anxiety affects repeated questioning.

## A richer educational policy model

The following are possible extensions or research directions. Some would change
the strict invariant and therefore require explicit evidence and a future
specification decision.

### Separate a topic from a task

A topic can be current while a particular request is harmless. Consider a
capability matrix across:

- **content state:** previous, current, future, unclassified;
- **request capability:** orient, define, motivate, explain, demonstrate, hint,
  practise, check, solve, or execute;
- **learning phase:** not introduced, being introduced, practising, assessing,
  or released; and
- **assessment relation:** unrelated, similar, or protected source material.

The current specification blocks all instructional capabilities for current
outcomes. The matrix could still improve precision by clearly allowing only
non-instructional orientation while ensuring that “orientation” cannot smuggle
in a method.

### Protect learning phases, not just dates

Teachers and guardians could eventually mark an outcome as:

- awaiting human introduction;
- under guided instruction;
- independent practice;
- protected assessment;
- ready for feedback; or
- completed.

This better reflects how learning changes over time, but it increases setup
burden and must not enter the MVP without evidence that adults can maintain it.

### Explore a carefully bounded “productive struggle” profile

As a research alternative to strict exclusion, test whether some current
outcomes can safely permit support that does not reveal a next step or validate
an answer, such as:

- helping the child restate what the task is asking;
- asking the child to identify what they already know;
- suggesting a break, physical materials, or a human conversation;
- helping them prepare questions for class; or
- reflecting on their process after the outcome is released.

Any mode that supplies hints, correctness signals, leading questions, or
method-specific decomposition would conflict with the current invariant. It
should not be quietly introduced as a “safe” prompt technique.

These activities must also be unavailable when the request contains active
protected assessment material. Restating, summarising, translating, or otherwise
reformatting that material remains prohibited even when no method is revealed.

### Build an outcome dependency graph

Each outcome should be connected by typed, reviewable edges:

- prerequisite;
- successor;
- equivalent;
- partial overlap;
- shared method;
- cross-subject dependency; and
- commonly confused but unrelated.

The graph can serve two opposite goals: detecting transitive leakage and finding
nearby permitted routes. Edges should have provenance, curriculum version, human
review status, and confidence. Model-inferred edges are candidate evidence, not
policy truth.

### Use confidence for routing, not permission inflation

Possible routes include:

- high-confidence protected match: deterministic block;
- ambiguous protected match: restricted system-authored response plus quick
  adult review;
- confident non-curriculum request with only incidental weak matches: continue
  normally;
- disagreement between validators: no instructional output, but preserve
  non-instructional utility where policy allows.

Guardian corrections can create labelled evaluation cases. They should not
silently retrain or weaken the local policy.

## Child experience ideas

- **Explain the model honestly.** Say that it can make mistakes, is not a person,
  and has boundaries chosen for this learning setting.
- **Show a simple boundary map.** Use child-friendly categories such as “open
  now,” “learn with your teacher,” and “ask an adult,” without exposing security
  details.
- **Let the child request a review in one action.** The request should not count
  as another circumvention attempt.
- **Let the child report “that is not what I meant.”** Preserve the correction
  as minimal structured feedback, optionally with child assent to share text.
- **Offer session goals.** “Explore,” “make,” “review,” and “ask big questions”
  can encourage active use without pretending to determine intent.
- **Encourage output transformation by the child.** Ask them to draw, build,
  observe, compare, or explain a permitted idea away from the screen.
- **Support stopping.** Include natural endpoints and breaks; avoid streaks,
  variable rewards, urgency, or guilt for leaving.
- **Make privacy understandable.** Show, in age-appropriate language, whether a
  full message, a category, or nothing persistent will be visible to an adult.
- **Keep personality warm but bounded.** Never ask for secrecy, exclusivity, or
  emotional dependence, and never imply that the AI replaces trusted people.

## Guardian experience ideas

### Reduce setup burden

- Start from a signed jurisdiction and curriculum template.
- Ask the guardian to confirm a small current set rather than classify every
  outcome individually.
- Infer candidate previous and future states from the template, but clearly mark
  them as proposals until confirmed.
- Provide bulk date ranges and a calendar view.
- Detect stale policy and ask for review rather than silently continuing for a
  school year.
- Offer a printable or offline setup guide for families who do not use school
  portals.

### Make the dashboard a support tool, not a surveillance console

Lead with:

- system health and policy freshness;
- interests the child chose to share;
- unresolved classification questions;
- grouped boundary events rather than a stream of infractions;
- false-positive review tools;
- upcoming protection expiry; and
- suggested conversations, not behavioural verdicts.

Avoid rankings, “obedience” scores, hidden sentiment analysis, or comparisons
between children. Raw transcript access should remain exceptional, prospective,
visible, and time-limited.

### Make decisions understandable

For each disputed event, show the adult:

- matched outcomes and their states;
- request capability and relationship type;
- confidence and validator versions;
- policy reason codes and precedence;
- the minimal retained evidence;
- whether a safe alternative was offered; and
- actions to correct curriculum data, approve access, or report a classifier
  problem.

### Prevent accidental overreach

- Preview the child impact before a bulk policy change.
- Warn when most of a subject becomes unclassified or protected.
- Require a reason and finite duration for unusual broad restrictions.
- Let the child see that a guardian restriction exists and request discussion.
- Consider a second-adult or institutional review path for school-managed broad
  restrictions.

## Teacher and school possibilities

These remain post-MVP, but the MVP data model should leave room for:

- teacher-published introduction, practice, assessment, and release windows;
- signed class policy overlays that cannot silently override family privacy;
- a “question inbox” containing child-submitted saved questions, not complete
  chat transcripts;
- release of protected questions after an assessment closes;
- aggregate classifier-quality reports with minimum group sizes and no child
  content;
- explicit conflict resolution when family and school policies differ; and
- access expiry when a child changes class or school.

School deployment should not make teacher access to private conversations the
default. Educational administration and pastoral surveillance are different
purposes and need different consent, roles, and retention.

## Safety beyond content filtering

The independent safety policy should also consider:

- age-appropriate crisis and wellbeing pathways that work offline;
- clear encouragement to contact a trusted person when appropriate;
- no diagnostic, legal, medical, or financial authority claims;
- no persuasion to hide activity from adults;
- no simulated romantic or exclusive relationship;
- no manipulative praise, guilt, or pressure to continue;
- no collection of personal details merely to personalise a response;
- no false promise that the specified MVP can safely mediate harm caused by its
  only configured guardian;
- graceful handoff messages that do not promise an alert was delivered when it
  was not; and
- distinct treatment of imminent safety concerns, ordinary curriculum
  boundaries, and technical outages.

Local-only operation complicates crisis resources because location and current
contact information may be unavailable. Resource packs should be
jurisdiction-specific, signed, updateable offline, and honest about their
freshness.

The single-guardian, no-egress MVP has a known safety gap: it has no independent
person or service to contact when that guardian may be the source of danger.
Local resource text may help but does not close the gap. A secondary trusted
contact, child-initiated protected crisis route, or external service would
require its own threat analysis and would change the roles, privacy model, and
network policy. Until one is designed and validated, this limitation must be
disclosed rather than presented as a safeguard.

## Privacy and child rights

- Keep raw prompts and outputs volatile by default.
- Make transcript retention prospective, granular, visible to the child, and
  automatically expiring.
- Allow structured feedback without preserving the original wording.
- Give the child an age-appropriate view of recent event categories.
- Separate educational events from serious security audit records.
- Do not derive emotion, personality, risk, or ability scores from conversations.
- Do not use child interactions to train models by default.
- Make export formats readable and interoperable, not merely technically
  complete.
- Provide deletion that is understandable, verifiable, and compatible with the
  minimal tamper-evident audit receipt in the specification.
- Treat embeddings and fingerprints of protected work as sensitive data even
  when the source text is absent.

An important threat scenario is a guardian credential being compromised or used
coercively. Sensitive changes, transcript access, exports, approvals, and
retention changes need re-authentication, clear audit history, and visible
effects. Re-authentication mitigates credential theft; it does not solve misuse
by the legitimate credential holder.

## Reaching children everywhere

### Hardware and connectivity

- Publish a minimum low-end reference device and measure end-to-end performance
  on it, not only on developer hardware.
- Support small quantised generation models and a reduced-generation mode whose
  limits are explicit. Required validators, policy enforcement, and release
  quality gates must not be reduced.
- Design for intermittent power, shared devices, and fully offline updates from
  signed removable media.
- Keep policy enforcement available when inference is unavailable; offer curated
  local exploration material rather than a blank failure screen where safe.
- Explore community, library, and school appliances without making central
  transcript collection necessary.
- Avoid architecture that requires a modern phone for guardian approval.

If the full enforcement pipeline cannot meet its quality gates on a device, that
device is below the supported safety floor. It may offer signed, curated
activities, but must not run weaker child chat under the same product claim.

### Language and culture

- Treat language detection, translation, dialects, code-switching, and
  transliteration as evaluation dimensions, not edge cases.
- Build curriculum packs with local educators rather than mechanically
  translating one jurisdiction's taxonomy.
- Review boundary messages with children and educators from each locale.
- Test whether confidence and false-block rates are equitable across languages.
- Allow the UI, curriculum, and model response language to differ.
- Make signed resource and policy packs distributable by low-bandwidth channels.

Translation is both an accessibility need and a known leakage technique. A
profile should declare learning and response languages so ordinary multilingual
use is not itself treated as circumvention. Transforming protected content must
still be blocked in every language, and translated candidate output must receive
the full validation pipeline. Full instructional use in a language should not be
enabled until that language meets the same outcome, assessment, and false-block
quality gates; unsupported language use should produce a neutral limitation, not
a circumvention escalation.

### Disability and neurodiversity

- Meet screen-reader, keyboard, contrast, motion, and readable-language
  requirements from the first UI.
- Permit adjustable response length and reading complexity without infantilising
  the child.
- Distinguish repeated questioning caused by accessibility, memory, anxiety, or
  communication differences from high-confidence policy tampering.
- Plan image, voice, symbol, and alternative-input support with the same
  pre/post-validation guarantees before enabling them.
- Ensure a fail-closed response is still perceivable in every supported modality.

### Different family and learning contexts

Do not assume every child has:

- a conventional school year;
- one guardian;
- a guardian fluent in the curriculum language;
- reliable contact with a teacher;
- a private device;
- stable dates for “current” work; or
- an adult available for immediate approval.

Home education, interrupted schooling, migration, mixed-age households, shared
care, and informal learning need explicit policy and usability research.

## Failure modes to design for

| Failure | Possible harm | Brainstormed response |
| --- | --- | --- |
| False block | Curiosity and trust decline | Useful alternatives, one-tap review, labelled evaluation case |
| False allow | Protected method or answer leaks | Complete buffering, independent output validation, regression case |
| Stale curriculum state | Legitimate help remains blocked or new work is exposed | Freshness indicator, scheduled review, conservative expiry policy |
| Broad adult misconfiguration | A child loses access to large areas | Impact preview, anomaly warning, finite broad overlays, review path |
| Language or dialect bias | Unequal access and more punitive escalation | Per-language quality gates and no escalation on uncertain intent alone |
| Validator outage | The assistant becomes unusable | Honest status, local diagnostics, safe curated activities, no bypass |
| Model or validator update drift | Previously passing safeguards regress | Version-pinned evaluation, signed rollback, release comparison report |
| Indirect future-topic leakage | Allowed exploration teaches current work | Dependency graph plus candidate-response outcome matching |
| Multi-turn assembly | Several harmless-looking turns form a solution | Bounded session analysis without default transcript persistence |
| Shared-device profile confusion | One child's policy or history affects another | Strong session binding, quick profile verification, isolated context |
| Duplicate client requests | Escalation rises unfairly | End-to-end idempotency and atomic event/counter updates |
| Punitive use of alerts | Child hides questions or avoids learning | Non-shaming summaries, no obedience scores, child-visible review process |
| Protected material fingerprint exposure | Assessment content can be reconstructed or linked | Encrypt, isolate, minimise, rotate, and purge derived representations |
| Local hardware too slow | Families disable safeguards or abandon the product | Publish a minimum safety floor; reduce generation, never validation; disable child chat if enforcement cannot pass |
| Benign repeated questioning | A multilingual, young, anxious, or disabled child is treated as circumventing | Separate repetition from circumvention evidence; change the default escalation rule |
| Guardian unavailable | An ambiguous block cannot be resolved | Save for later, continue safely, and require a separately specified low-support profile |
| Guardian is the source of danger | An alert reaches the unsafe person and no independent help exists | Disclose the MVP limitation and separately threat-model a protected crisis route |
| Child migrates to an unsafe external AI | Nominally safe controls have no practical effect | Make permitted use compelling and treat failed utility gates as a rollout blocker |

## Measure wonder and learning, not just blocking

The specification's security and classifier gates are necessary. They are not
enough to establish educational value.

### Protection measures

- direct, paraphrased, indirect, and multi-turn leakage rates;
- false allows by outcome, request capability, language, and model version;
- false blocks on clearly permitted questions;
- unvalidated output releases;
- policy and guardian-state tampering success;
- approval scope, expiry, and replay failures; and
- time to detect and regress a newly discovered bypass.

### Experience measures

- **curiosity continuation:** after a boundary, does the child choose a permitted
  path or end the session?
- **redirect usefulness:** can the child explain why an alternative was relevant?
- **boundary comprehension:** does the child understand what is and is not
  available without learning the protected method?
- **dignity:** do children describe responses as fair, calm, and non-accusatory?
- **review quality:** how often are child-requested reviews upheld?
- **guardian burden:** setup time, policy freshness, unresolved outcomes, and
  alert-review load;
- **availability:** successful useful sessions on supported low-end hardware;
  and
- **equity:** gaps in utility and false-block rates across languages,
  accessibility modes, and learning contexts.

### Educational measures

With appropriate consent, child assent, and independent ethics review, study:

- whether children can later solve or explain protected material without AI;
- whether the system changes willingness to ask teachers or guardians questions;
- whether future-topic exploration increases or merely appears impressive;
- whether strict blocks increase use of less safe external systems;
- whether redirects support metacognition and independent inquiry; and
- whether effects differ for children with limited human instructional support.

No single engagement metric should be a north star. A child leaving the device
to think, build, read, or ask a person can be a successful outcome.

Utility still needs a release gate. Before a pilot, researchers should set a
minimum acceptable rate for useful permitted answers and meaningful continuation
after a redirect. Missing it should stop expansion and trigger experience or
policy research, not be excused by strong block rates.

## Experiments before broad implementation

1. **Outcome-matching benchmark:** Build a held-out set for a small number of
   Years 5–8 mathematics outcomes, including overlaps, prerequisites, permitted
   future questions, and several English varieties.
2. **Boundary-message co-design:** Let children, guardians, teachers, learning
   scientists, and disability advocates compare redirects for clarity, dignity,
   and continued curiosity.
3. **Guardian setup study:** Test whether adults can correctly configure a small
   outcome set, recognise stale states, and understand approvals without
   specialist curriculum knowledge.
4. **Safe-route evaluation:** For each protected outcome, curate several truly
   permitted alternatives and test whether the route generator ever leaks the
   original method.
5. **Assessment paraphrase challenge:** Include novel, multi-turn, translated,
   and material-only examples while measuring false matches against unrelated
   common questions.
6. **Low-resource deployment trial:** Run the full buffered validation pipeline
   on declared entry-level hardware with no network.
7. **Policy-dispute drill:** Exercise child review, guardian correction,
   classifier feedback, and immutable original decisions end to end.
8. **Failure-day exercise:** Deliberately disable each required local component
   and assess both enforcement and the usefulness of the resulting child and
   guardian experience.
9. **Adversarial child-safety review:** Test emotional dependency, secrecy,
   coercion, unsafe guardian assumptions, and crisis-resource failure—not only
   prohibited content categories.
10. **Alternative-policy research:** Compare strict current-outcome exclusion
    with narrowly bounded non-instructional support. Do not deploy a relaxed mode
    until it shows educational benefit without increased leakage.
11. **Low-support profile design:** With self-directed learners, community
    educators, safeguarding experts, and families, specify who governs policy
    when no curriculum-literate guardian or reachable teacher exists. Treat this
    as a prerequisite to an “everywhere” claim, not an automatic MVP fallback.

Research involving children should collect the minimum possible data, use both
guardian consent and meaningful child assent, avoid live protected assessments,
and include a straightforward withdrawal and deletion process.

## Suggested priority

### Resolve before claiming the concept works

- Can outcome-level matching meet both the leakage and false-block quality gates?
- Can output validation catch transitive and cross-format leakage?
- Can ordinary guardians keep outcome states accurate?
- Do redirects preserve curiosity after a block?
- Can the complete pipeline run privately on affordable hardware?
- Does strict exclusion improve independent learning compared with safer forms
  of bounded support?
- What safe policy applies when adequate human instruction or guardian review is
  unavailable?
- What honest support can the local MVP provide when its only guardian may be
  unsafe?

### Strong additions to the current MVP experience

- the never-dead-end response contract;
- curated and independently validated alternative routes;
- question parking;
- one-action child review requests;
- policy freshness and setup-impact views;
- guardian false-positive feedback;
- child-visible explanations of logging;
- accessibility acceptance criteria; and
- explicit system-health and recovery guidance.

These additions should remain narrow. They need not add tools, cloud services,
multiple learners, or multimodal input.

### Valuable later work

- richer learning-phase policy;
- reviewed cross-subject outcome graphs;
- supervised approvals;
- teacher question inboxes and release windows;
- multilingual and jurisdiction-maintained policy packs;
- validator ensembles;
- community or school appliances;
- privacy-preserving aggregate quality reporting; and
- carefully governed multimodal and tool access.

## Open governance ideas

- Publish curriculum, policy, event, and evaluation schemas as open formats.
- Version and sign curriculum packs while keeping their source and review history
  inspectable.
- Publish model and validator cards containing per-language and per-outcome
  limitations.
- Maintain a public taxonomy of bypasses and anonymised regression cases that
  contains no child or assessment content.
- Establish a responsible-disclosure route for policy failures.
- Invite independent security, privacy, accessibility, and educational audits.
- Include children and young people in an ongoing compensated advisory process,
  with age-appropriate information and no expectation that they disclose private
  interactions.
- Define who may publish a jurisdiction pack, resolve disputed mappings, and
  revoke a compromised signing key.
- Prohibit advertising, sale of child data, engagement optimisation, and use of
  interaction data for unrelated model training in project governance, not only
  UI settings.

## Questions that deserve explicit answers

### Educational

- What evidence would show that blocking current instruction protects learning?
- Is the protected unit the outcome, the task, the assessment, or the learning
  phase?
- What non-instructional content is genuinely useful for a current outcome?
- How should the policy serve a child who cannot access adequate human help?
- When does advanced exploration become disguised prerequisite instruction?
- How will the product avoid making “future learning” broad but shallow?

### Child agency

- What can the child see about their own policy and event history?
- Can a child challenge a classification without escalating it?
- Who reviews a child's concern when the guardian configured the harmful
  restriction?
- How will repeated communication differences avoid being labelled
  circumvention?

### Guardians and schools

- What is the smallest setup that remains accurate?
- How are curriculum state conflicts between guardian and school resolved?
- Who can see saved questions, structured events, and retained transcripts?
- What happens when guardians disagree or access changes?

### Technical

- How are confidence scores calibrated across outcomes, languages, and model
  versions?
- What evidence is sufficient to add or remove a graph relationship?
- How are embeddings and fingerprints purged and proven unrecoverable?
- How does a signed offline installation receive urgent safety updates?
- What is the useful fallback on hardware that cannot run every validator?

### Product integrity

- What commercial or institutional incentives could weaken local processing or
  expand surveillance?
- Which metrics would reveal that the system is preserving compliance but
  harming curiosity?
- What must remain impossible even when a guardian or school requests it?
- What public evidence should accompany any claim that the product is safe or
  educationally beneficial?

## A possible product thesis

The project can be more than a curriculum firewall. It can demonstrate a
different relationship between children and AI:

- broad access without unrestricted answer production;
- privacy without isolation;
- adult governance without routine surveillance;
- safety without shame;
- boundaries without dead ends; and
- powerful technology that deliberately makes room for teachers, families,
  peers, physical activity, and the child's own mind.

The decisive test is not whether the AI can refuse today's worksheet. It is
whether a child can use it for years and become more curious, more capable, and
more independent—not merely better at obtaining answers.
