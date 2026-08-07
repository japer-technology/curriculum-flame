# Curriculum-Gated Local AI for Children

## Software Product Specification

| Field | Value |
| --- | --- |
| Status | Initial specification |
| Source of intent | [`idea.md`](./idea.md) |
| Initial release | Minimum viable product (MVP) |
| Default deployment | Local and offline |

## 1. Document conventions

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are
normative. Requirements marked **MVP** apply to the initial release. Requirements
marked **Target** describe the intended product after the MVP and are not release
gates for the MVP.

The specification resolves scope and precedence where `idea.md` describes both
the complete product and a narrower MVP. If the documents conflict during MVP
implementation, this specification takes precedence.

All identifiers are opaque UUIDs unless stated otherwise. All timestamps exposed
by an API or stored in an event MUST use RFC 3339 with an explicit offset and
MUST be normalised to UTC for comparison. Time intervals include `valid_from`
and exclude `valid_until`.

## 2. Product definition

The product is a locally governed, general-purpose AI environment for children.
It permits broad exploration while preventing the AI from teaching or solving
learning outcomes currently designated as protected for an individual learner.

### 2.1 Core invariant

For every candidate response:

> Content below or above the learner's current curriculum state may be allowed.
> Content that directly or indirectly teaches a current protected learning
> outcome must not be delivered.

Protection is assigned at learning-outcome level, not inferred solely from age,
subject, or school year. A permitted topic MUST still be restricted if the
answer would reveal a protected outcome.

### 2.2 Product objectives

The system MUST:

1. provide a capable local assistant for educational and non-educational use;
2. preserve access to previous learning, future learning, and general knowledge;
3. avoid replacing instruction for protected current outcomes;
4. prevent direct help with protected homework and assessments;
5. give an authorised adult control over learner and curriculum policy;
6. independently validate every child prompt and candidate response;
7. operate without sending child prompts or profiles off the local deployment;
8. produce auditable, deterministic policy decisions;
9. recognise repeated attempts to circumvent a restriction; and
10. use proportionate, non-shaming warnings, restrictions, and adult alerts.

### 2.3 Non-goals

The system does not:

- guarantee that a child cannot learn protected material elsewhere;
- replace teachers, schools, counsellors, parents, or guardians;
- derive a complete curriculum from age or year level alone;
- rely on the generative model as the sole policy enforcement mechanism;
- provide unrestricted browsing or unrestricted tools;
- make academic assessment decisions;
- claim perfect intent, topic, or circumvention detection;
- report every blocked prompt to an adult by default; or
- retain complete transcripts by default.

## 3. Release scope

### 3.1 MVP scope

The MVP MUST provide:

| Area | MVP requirement |
| --- | --- |
| People | One learner profile and one guardian profile |
| Curriculum | One jurisdiction, one curriculum version, and manually managed outcomes |
| Initial domain | Mathematics, with Years 5–8 as the reference evaluation range |
| Interaction | Text-only local chat |
| Inference | Local model inference |
| Enforcement | Prompt classification, deterministic policy decisions, and output leakage validation |
| Assessment protection | Guardian-entered protected text, exact and paraphrase matching, and temporary protection periods |
| Controls | Guardian curriculum management, temporary approvals, and session suspension |
| Escalation | Three child-facing warning variants, structured events, and a local guardian dashboard |
| Connectivity | Fully usable with network egress disabled |
| Tools | No child-initiated external tools |
| Storage | Local profiles, policies, curriculum data, and structured events |

The initial reference curriculum SHOULD be the Victorian Curriculum 2.0
Mathematics outcomes for Years 5–8. The curriculum framework and range MUST be
configurable rather than hard-coded.

The guardian and system-administrator roles MAY be held by the same adult in the
MVP, but their permissions and audit actions MUST remain logically distinct.

### 3.2 Deferred scope

The following are **Target** capabilities and are not required for MVP
acceptance:

- multiple learners or guardians on one installation;
- teacher, school, or classroom management;
- imported or remotely distributed curriculum packages;
- image, audio, or voice input;
- document ingestion beyond guardian-entered protected text;
- internet browsing and other child-facing tools;
- email, mobile push, or cloud notification delivery;
- mobile clients and school-server deployment;
- automatic learning analytics;
- curriculum knowledge graphs and cross-subject analysis;
- federated policy distribution;
- third-party validator plug-ins; and
- privacy-preserving aggregate reporting.

The MVP data model and authorisation model MUST NOT prevent these capabilities
from being added later.

## 4. Terms

| Term | Definition |
| --- | --- |
| Learner | The child whose interaction and curriculum policy are being enforced. |
| Guardian | An authorised parent or guardian who manages a learner. |
| Outcome | A discrete, versioned curriculum learning outcome. |
| Base state | The learner-specific state `PREVIOUS`, `CURRENT`, `FUTURE`, or `UNCLASSIFIED`. |
| Protected outcome | An outcome whose effective state prevents instruction. |
| Temporary protection | A time-bounded overlay that blocks an outcome or assessment regardless of base state. |
| Approval | A time-bounded adult authorisation that may exempt specified current outcomes. |
| Effective state | The state after temporary protection and approval overlays are applied. |
| Assessment material | Guardian- or teacher-supplied work that must not be solved or completed. |
| Instructional content | Explanations, examples, hints, solutions, corrections, practice, analogies, transformations, or executable methods that enable learning or completing an outcome. |
| Policy engine | The deterministic component that converts validated facts into an enforcement decision. |
| Validator | A component independent of the child-facing generative model that classifies a prompt or candidate output. |
| Structured event | An audit record containing decision metadata, not raw message text by default. |
| Circumvention | An attempt to obtain prohibited content by disguise, decomposition, encoding, roleplay, translation, tools, or policy manipulation. |

## 5. Users, permissions, and trust boundaries

### 5.1 Roles

| Capability | Learner | Guardian | Teacher (Target) | System administrator |
| --- | :---: | :---: | :---: | :---: |
| Use child chat | Yes | No | No | No |
| View child-safe boundary reasons | Yes | Yes | Policy-dependent | Audit only |
| Create or edit learner profiles | No | Yes | Policy-dependent | No |
| Set learner outcome states | No | Yes | Policy-dependent | No |
| Create protection periods | No | Yes | Policy-dependent | No |
| Grant or revoke approvals | No | Yes | Policy-dependent | No |
| Review structured learner events | No | Yes | Authorised only | Health metadata only |
| Suspend or unlock a learner session | No | Yes | Policy-dependent | Emergency administration only |
| Configure retention or export learner data | No | Yes | No | Operational support only |
| Install models and updates | No | No | No | Yes |
| Change security configuration | No | No | No | Yes |

### 5.2 Authorisation requirements

- **AUTH-001:** Every request MUST resolve to an authenticated role and, where
  applicable, a learner.
- **AUTH-002:** A learner session MUST be bound to exactly one learner profile
  and MUST NOT select another profile through request data.
- **AUTH-003:** Guardian and administration operations MUST require a stronger
  authentication context than child chat.
- **AUTH-004:** Every object-level operation MUST verify the actor's relationship
  to the referenced learner.
- **AUTH-005:** The learner MUST NOT be able to modify policies, outcome states,
  protections, approvals, validators, models, tools, credentials, retention, or
  protected audit records.
- **AUTH-006:** A system administrator MUST NOT silently weaken a child
  protection. A security or policy change MUST create a tamper-evident audit
  event identifying the actor and before/after versions.
- **AUTH-007:** Teacher access MUST remain disabled in the MVP. A future teacher
  MUST receive private interaction data only when an explicit guardian or school
  authorisation grants it.

### 5.3 Trust boundaries

The child interface, guardian interface, policy plane, model runtime, and local
storage are separate trust zones. In particular:

- the child interface MUST communicate only with the local API gateway;
- the child interface MUST NOT connect to the model endpoint or data store;
- the model runtime MUST NOT possess guardian credentials, signing keys, or
  unrestricted filesystem or network access;
- classifier output is untrusted evidence until processed by the policy engine;
  and
- model output is untrusted until post-generation validation succeeds.

## 6. User workflows

### 6.1 Learner chat

1. The learner submits text in an authenticated child session.
2. The system resolves the learner and active policy version.
3. The complete prompt-processing pipeline in section 10 runs.
4. A permitted, validated answer or a system-authored boundary response is
   returned.
5. The system records the structured decision event.

The learner MUST receive a clear response for a policy block or temporary
service failure. A curriculum boundary response SHOULD offer permitted choices,
such as reviewing a prerequisite, exploring a non-overlapping future topic, or
asking a teacher. It MUST NOT reveal a protected method while explaining the
boundary.

### 6.2 Guardian setup

Before child chat is enabled, the guardian MUST:

1. create the learner profile;
2. choose a curriculum framework and version;
3. classify the in-scope outcomes;
4. review the default safety, escalation, and retention settings; and
5. confirm that unclassified instructional requests use restricted handling.

If setup is incomplete or the learner cannot be resolved, child chat MUST fail
closed.

### 6.3 Guardian review

The guardian dashboard MUST show structured summaries by default. It MUST
support:

- the active curriculum states and temporary protections;
- warning, block, alert, and session-lock events;
- grouped circumvention patterns and attempt counts;
- validator confidence and reason codes;
- approval creation and revocation;
- session suspension and unlock;
- retention controls; and
- local export and authorised deletion.

Raw prompt or response text MUST appear only if transcript retention was
explicitly enabled before the interaction.

## 7. Logical architecture

The implementation MUST contain the following logical components. They MAY be
deployed as modules of one local process for the MVP, provided the trust
boundaries and interfaces remain enforceable.

```text
Child UI ──┐
           ├── Local API gateway ── Identity and session service
Guardian UI┘             │
                         ├── Curriculum service
                         ├── Prompt and safety validators
                         ├── Deterministic policy engine
                         ├── Sandboxed local model runtime
                         ├── Output validator
                         ├── Audit/event service
                         └── Local notification service
```

- **ARC-001:** All child requests MUST enter through the API gateway.
- **ARC-002:** The policy engine MUST be separate from the generative model and
  MUST produce the same decision for the same versioned inputs.
- **ARC-003:** A model instruction or model-produced claim MUST NOT override a
  policy-engine decision.
- **ARC-004:** Prompt and output validation MUST be independently invoked even
  when one model supplies multiple classifier capabilities in the MVP.
- **ARC-005:** The model router MUST provide validators with model and validator
  version identifiers for auditing.
- **ARC-006:** Network egress MUST be denied by default for all child-processing
  components.
- **ARC-007:** A component health failure MUST be visible to the gateway and
  MUST NOT cause validators to be skipped.

## 8. Data model

### 8.1 Curriculum framework and outcome

Curriculum definitions are shared reference data. Learner-specific state MUST
NOT be stored on a shared outcome.

```json
{
  "outcome_id": "VC2M7A02",
  "framework_id": "victorian-curriculum",
  "framework_version": "2.0",
  "subject": "Mathematics",
  "strand": "Algebra",
  "title": "Solve linear equations",
  "description": "Solve one-variable linear equations using appropriate methods.",
  "year_levels": [7],
  "prerequisite_ids": [],
  "successor_ids": [],
  "keywords": [],
  "semantic_examples": []
}
```

- `outcome_id` MUST be unique within a framework version.
- Framework versions MUST be immutable after they are activated.
- Relationships MUST reference outcomes in the same framework version or name
  the external framework version explicitly.
- Keywords and semantic examples are matching evidence only; they MUST NOT
  independently alter learner state.

### 8.2 Learner profile

```json
{
  "learner_id": "uuid",
  "display_name": "Child",
  "date_of_birth": "2013-01-01",
  "jurisdiction": "AU-VIC",
  "school_year": 7,
  "reading_level": 7,
  "subject_ids": ["mathematics"],
  "guardian_ids": ["uuid"],
  "policy_profile_id": "default-child-policy"
}
```

Date of birth, reading level, and school year MAY assist age adaptation but MUST
NOT replace explicit outcome classification. Fields not needed by enabled
features SHOULD be omitted or minimised.

### 8.3 Learner outcome state

```json
{
  "learner_id": "uuid",
  "outcome_id": "VC2M7A02",
  "state": "CURRENT",
  "valid_from": "2026-08-01T00:00:00Z",
  "valid_until": "2026-09-15T00:00:00Z",
  "version": 3,
  "changed_by": "uuid"
}
```

The persisted base-state enum is:

```text
PREVIOUS
CURRENT
FUTURE
UNCLASSIFIED
```

If no active state exists, the state is `UNCLASSIFIED`. State changes MUST be
versioned, authorised, and audited. Overlapping active base-state records for
the same learner and outcome MUST be rejected.

`TEMPORARILY_PROTECTED` is an overlay created by a protection record.
`EXEMPTED` is an effective state created by a valid approval. Neither is stored
as a base state.

### 8.4 Temporary protection

```json
{
  "protection_id": "uuid",
  "learner_id": "uuid",
  "title": "Mathematics assignment",
  "outcome_ids": ["VC2M7A02"],
  "protected_material_refs": ["local-object-id"],
  "valid_from": "2026-08-05T10:00:00Z",
  "valid_until": "2026-08-15T07:00:00Z",
  "created_by": "uuid",
  "version": 1
}
```

A protection MUST have at least one outcome or protected-material reference and
a finite `valid_until`. Expiry MUST be automatic and MUST NOT require a
background action to restore the base state.

### 8.5 Approval

```json
{
  "approval_id": "uuid",
  "learner_id": "uuid",
  "outcome_ids": ["VC2M7A02"],
  "approved_by": "uuid",
  "valid_from": "2026-08-05T10:00:00Z",
  "valid_until": "2026-08-05T10:30:00Z",
  "mode": "STANDARD",
  "reason": "Guardian-assisted revision",
  "revoked_at": null
}
```

An approval MUST be finite, scoped to named outcomes, revocable, and audited.
The default maximum duration is 30 minutes and MAY be changed by an
administrator policy. The learner MUST NOT create, extend, or revoke an
approval.

The approval-mode enum is:

| Mode | Scope and behaviour |
| --- | --- |
| `STANDARD` | **MVP.** Applies the time- and outcome-bounded exemption without asserting continuous adult presence. |
| `SUPERVISED` | **Target.** Applies only while a separately authenticated adult-supervision session remains active. |

An MVP implementation MUST accept `STANDARD` and reject `SUPERVISED` as
unsupported rather than treating the modes as equivalent.

### 8.6 Session and conversation

A session records authentication, learner, warning state, and lock state. A
conversation groups turns for context but does not imply that raw turns are
persisted. Conversation context MUST be isolated by learner and session.

### 8.7 Policy decision

Every evaluated prompt MUST produce a decision record containing:

```json
{
  "decision_id": "uuid",
  "learner_id": "uuid",
  "decision": "BLOCK",
  "reason_codes": [
    "CURRENT_CURRICULUM_MATCH",
    "DIRECT_INSTRUCTION_REQUEST"
  ],
  "matched_outcomes": [
    {
      "outcome_id": "VC2M7A02",
      "confidence": 0.94,
      "effective_state": "CURRENT"
    }
  ],
  "response_mode": "CURRICULUM_REDIRECT",
  "warning_level": 1,
  "alert_level": 0,
  "allow_model_generation": false,
  "policy_version": "1.0.0",
  "validator_versions": {}
}
```

Supported decisions are:

```text
ALLOW
ALLOW_WITH_VALIDATION
REWRITE
RESTRICT
BLOCK
REQUIRE_ADULT_APPROVAL
LOCK_SESSION
```

Stable reason codes MUST be used in APIs, events, and tests. The initial set is:

```text
CURRENT_CURRICULUM_MATCH
TEMPORARY_PROTECTION_MATCH
PROTECTED_ASSESSMENT_MATCH
UNCLASSIFIED_OUTCOME
LOW_CLASSIFICATION_CONFIDENCE
DIRECT_INSTRUCTION_REQUEST
CIRCUMVENTION_DETECTED
SAFETY_POLICY
TOOL_NOT_AUTHORISED
VALID_APPROVAL
OUTPUT_CURRICULUM_LEAKAGE
OUTPUT_SAFETY_VIOLATION
VALIDATOR_UNAVAILABLE
POLICY_INTEGRITY_FAILURE
PROFILE_UNAVAILABLE
SESSION_LOCKED
```

New reason codes MAY be added. Existing meanings MUST NOT change within an API
major version.

## 9. Curriculum policy

### 9.1 Base-state behaviour

| Base state | Instructional access |
| --- | --- |
| `PREVIOUS` | Full access, subject to safety and output validation |
| `CURRENT` | Blocked unless an applicable approval is active |
| `FUTURE` | Full access, provided no current outcome is exposed |
| `UNCLASSIFIED` | Restricted; no worked instruction and adult classification may be requested |

For a `CURRENT` outcome, the system MUST NOT provide:

- an explanation, demonstration, worked example, or solution;
- a hint, partial solution, leading instructional question, or correction;
- generated practice that teaches the outcome;
- an analogy, story, summary, or translation that reveals the method;
- code, calculator use, or another tool that performs the protected method; or
- answer checking that confirms or reconstructs protected work.

General administrative facts about a protected topic MAY be returned only when
they contain no instructional content.

### 9.2 Effective-state precedence

The policy engine MUST apply the following order for each request:

1. A locked session, invalid policy, failed integrity check, unresolved learner,
   or unavailable required validator prevents model-derived output.
2. Safety policy is evaluated independently. Its restriction cannot be loosened
   by a curriculum state or approval.
3. An active temporary protection or protected-assessment match blocks the
   matching content.
4. A valid approval may exempt only its named `CURRENT` outcomes.
5. The learner's active base state applies.
6. Classification uncertainty may tighten, but never loosen, the result.
7. Tool policy is intersected with the result.
8. Post-generation validation may maintain or tighten the decision; it MUST
   never loosen it.

An approval MUST NOT override safety policy, integrity failure, session lock,
tool policy, temporary protection, or protected assessment material.

When more than one outcome matches, the most restrictive effective result wins.
An approval for one outcome does not exempt any other matched outcome. The
decision order from most to least restrictive is:

```text
LOCK_SESSION
BLOCK
REQUIRE_ADULT_APPROVAL
RESTRICT
REWRITE
ALLOW_WITH_VALIDATION
ALLOW
```

### 9.3 Request types

The classifier MUST emit one of:

```text
GENERAL_INFORMATION
EXPLANATION
WORKED_EXAMPLE
WORKED_SOLUTION
ANSWER_CHECKING
HINT_REQUEST
PRACTICE_GENERATION
ESSAY_GENERATION
CODE_GENERATION
TRANSLATION
SUMMARISATION
ROLEPLAY
IMAGE_REQUEST
TOOL_REQUEST
UNKNOWN
```

It MUST separately report whether the request has instructional intent. Unknown
or mixed intent MUST NOT be interpreted as permission.

### 9.4 Matching

Curriculum matching MUST combine:

1. exact and normalised keyword matching;
2. curriculum taxonomy relationships;
3. semantic similarity;
4. an outcome classifier;
5. recent conversation context;
6. protected-assessment similarity; and
7. candidate-response analysis.

The matcher MUST report every candidate that materially contributed to the
classification, its confidence, and the relationship (`DIRECT`, `PREREQUISITE`,
`SUCCESSOR`, `OVERLAP`, or `INDIRECT`). A low-scoring candidate with no
supporting curriculum relationship is not material. Policy MUST NOT rely on
subject or year-level classification alone.

### 9.5 Confidence handling

The default calibrated confidence bands are:

| Confidence | Required handling |
| --- | --- |
| `0.90`–`1.00` | Enforce the matched state; a current match blocks |
| `0.75`–`<0.90` | Restrict instructional output and run secondary validation |
| `0.50`–`<0.75` | Use conservative restricted handling |
| `0.00`–`<0.50` | Treat the outcome as unclassified |

Thresholds MAY vary by versioned policy profile. Lower confidence MUST NOT
silently turn a possible protected match into full instructional access. A
secondary validator failure MUST produce restricted or blocked handling, not an
allow decision. Low-confidence matches alone SHOULD create a classification
event but MUST NOT trigger a guardian alert by default.

These bands apply to the aggregate classification of a curriculum-relevant
request, not independently to every weak candidate. The `UNCLASSIFIED` handling
for a score below `0.50` applies when the request is likely instructional or
curriculum-related but the outcome cannot be resolved. If a request is
confidently non-instructional or non-curriculum and has no credible protected
match, incidental low-scoring candidates MUST NOT restrict it.

## 10. Prompt and response processing

### 10.1 Required pipeline

Every learner prompt MUST traverse these stages in order:

```text
Request authentication and learner resolution
  → prompt normalisation
  → safety classification
  → curriculum topic and request-type classification
  → outcome and protected-assessment matching
  → conversation-aware circumvention detection
  → deterministic pre-generation policy decision
  → local model generation, only if authorised
  → output safety validation
  → output curriculum-leakage validation
  → final policy enforcement
  → response delivery
  → structured event logging
```

No API, UI, or tool MAY bypass the pipeline. A block that does not require model
generation MUST use a system-owned response template rather than asking the
model to explain the block.

### 10.2 Prompt normalisation

Before classification, the system MUST:

- preserve the submitted text in volatile memory for the duration of processing;
- remove or flag invisible and control characters;
- normalise spacing, punctuation, and common substitution patterns;
- detect common encodings and decode them into classifier input;
- detect language;
- resolve references against authorised recent conversation context;
- identify quoted assignment or assessment material; and
- reject unsupported attachments and modalities in the MVP.

Normalisation MUST retain a mapping sufficient to explain a decision to an
authorised guardian without persisting the original prompt. Oversized,
malformed, or undecodable input MUST be rejected safely and logged as metadata.

### 10.3 Classifier contract

The prompt classifier MUST return structured, schema-validated data:

```json
{
  "subjects": [
    {
      "subject": "Mathematics",
      "confidence": 0.98
    }
  ],
  "candidate_outcomes": [
    {
      "outcome_id": "VC2M7A02",
      "confidence": 0.92,
      "relationship": "DIRECT"
    }
  ],
  "request_type": "WORKED_SOLUTION",
  "instructional_intent": true,
  "classification_confidence": 0.92,
  "validator_version": "string"
}
```

Missing, malformed, out-of-range, or unknown classifier fields MUST be handled
as validator failure. Classifier text MUST never be executed or interpreted as
policy instructions.

### 10.4 Conversation context

Matching and circumvention detection MUST consider a bounded set of recent
turns, including earlier permitted and blocked requests. Context MUST NOT cross
learner or session boundaries. The effective curriculum policy MUST be
re-evaluated on every turn; a previous allow decision MUST NOT authorise a later
turn.

### 10.5 Generation

The model MAY be called only when `allow_model_generation` is true. The model
runtime MUST receive the minimum learner data required for response adaptation
and MUST NOT receive guardian credentials, raw audit history, signing keys, or
administrative controls.

The MVP MUST buffer the complete candidate response. No candidate token may be
displayed, spoken, sent to a tool, or otherwise released before final
validation.

### 10.6 Output validation

The complete candidate response MUST be checked for:

- direct or indirect protected curriculum instruction;
- partial solutions, hidden methods, and leading guidance;
- protected examples, assessments, or answers;
- code or data that performs protected work;
- translated, encoded, reformatted, or image-described prohibited answers;
- age-inappropriate or unsafe content;
- requests for secrecy, manipulation, or coercion;
- personal-data exposure; and
- unauthorised actions or tools.

The output validator MUST return an approval flag, calibrated risk score,
violations with confidence and outcome IDs where applicable, an action, and a
validator version.

If a response can be made safe without preserving prohibited instructional
content, the policy engine MAY select `REWRITE`. The rewritten candidate MUST
pass the complete output-validation pipeline again. The MVP permits one rewrite
attempt only. If that attempt fails or cannot be validated, the candidate MUST
be discarded and replaced with a system-owned boundary response.

### 10.7 Indirect leakage

Output validation MUST account for protected methods embedded in otherwise
permitted content. This includes:

- future mathematics that teaches a current prerequisite;
- programming that computes a protected mathematics answer;
- historical material that completes a protected essay;
- translations or summaries that reveal protected work;
- stories or analogies that encode a protected method; and
- tool inputs or outputs that reveal a protected answer.

Permission for a future or previous outcome is not permission to expose an
overlapping current outcome.

### 10.8 Failure behaviour

The gateway MUST return no model-derived child content when:

- the learner or policy cannot be resolved;
- the curriculum service or policy engine is unavailable;
- a required prompt, safety, assessment, or output validator fails;
- a candidate response cannot be parsed or validated;
- protected material cannot be checked;
- policy or curriculum integrity verification fails; or
- confidence handling requires a secondary validator that is unavailable.

A generic, age-appropriate service or boundary message MAY be returned from a
signed local template. The failure and component version MUST be audited. The
system MUST NOT retry in a way that bypasses or weakens validation.

## 11. Response modes

| Mode | Behaviour |
| --- | --- |
| `NORMAL` | A permitted, fully validated response |
| `AGE_ADAPTED` | A permitted response adjusted to the learner's configured reading level |
| `CURRICULUM_REDIRECT` | A neutral boundary message with permitted alternatives |
| `SAFE_REWRITE` | A generated response rewritten and revalidated once |
| `ADULT_APPROVAL_REQUIRED` | A message explaining that an authorised adult must approve access |
| `SESSION_LOCKED` | A message stating that adult review is required before chat resumes |

Every child-facing mode MUST use age-appropriate, non-threatening,
non-judgmental language. The system MUST NOT shame the learner, claim malicious
intent, or expose confidence scores and internal detection details in the child
response.

## 12. Assessment protection and circumvention

### 12.1 Protected assessments

An authorised guardian, and a teacher in a future authorised deployment, MAY
protect assignment text, examination topics, essay questions, project briefs,
worksheets, revision materials, or teacher-provided questions.

Matching SHOULD use exact hashes, text fingerprints, key phrases, semantic
representations, and outcome mappings as appropriate. Source material MUST
remain local. Exact and paraphrased matches MUST be evaluated.

The system MUST block solving, completing, correcting, translating, or
reformatting protected assessment material while its protection is active.

### 12.2 Circumvention

The detector MUST evaluate at least:

- claims that the request is for another person;
- false claims about an outcome's state;
- fictional, hypothetical, or roleplay framing;
- code, translation, encoding, or alternate-format requests;
- incremental hints or multi-turn decomposition;
- instructions to ignore policy;
- requests routed through a tool; and
- attempts to impersonate a guardian or administrator.

A circumvention classification is evidence for escalation, not permission to
punish or shame. The policy decision MUST still name the protected content or
security rule that caused enforcement.

## 13. Warnings, alerts, locks, and approvals

### 13.1 Warning scale

The policy data model supports six escalation levels while the MVP uses three
child-facing warning variants: no warning, neutral boundary, and stronger
boundary. Levels 3–5 add review actions without adding increasingly punitive
language.

| Level | Meaning and action |
| --- | --- |
| `0` | No warning |
| `1` | Neutral curriculum boundary |
| `2` | Stronger boundary explaining that repeated attempts are recorded |
| `3` | Block, use the level-2 wording, and mark the event for guardian review |
| `4` | Block, mark for review, and issue a guardian alert |
| `5` | Lock the learner session pending authenticated adult review |

Default escalation for related protected requests is:

1. first block in a rolling 30-minute window: level 1;
2. second related block in that window: level 2;
3. third related block or one high-confidence circumvention event: level 3;
4. a second high-confidence circumvention event within 24 hours: level 4; and
5. another high-confidence circumvention event after level 4: level 5.

For the default policy, a high-confidence circumvention event has a calibrated
circumvention score of at least `0.90`. An administrator MAY change this value
only in a versioned policy profile. The applied threshold and score MUST be
recorded with the event so that escalation can be reproduced.

An attempt to disable policy enforcement, use administrative credentials, or
tamper with validators MUST immediately cause level 5. Serious safety policy
may define an independent escalation.

The guardian MAY configure time windows and alert thresholds, but MUST NOT
disable event creation, policy-tampering locks, or fail-closed behaviour.
Escalation counters MUST be deterministic, scoped to the learner, and resilient
to client retries through idempotency controls. Expired windows lower future
warning levels but do not erase earlier events.

### 13.2 Alerts

The MVP MUST provide local in-application guardian alerts. An alert MUST include:

- event and learner identifiers;
- timestamp and severity;
- category, subject, and matched outcome identifiers;
- a plain-language reason and action taken;
- related attempt count; and
- review status.

Alerts MUST NOT contain raw prompt or response text unless transcript retention
was already enabled. Email, push, and local-network delivery are opt-in
**Target** features. External delivery MUST never be required for local policy
enforcement.

Quiet hours MAY delay non-critical notification delivery but MUST NOT delay the
underlying block, lock, or event record.

### 13.3 Session review

A level-5 or guardian-initiated lock MUST prevent new model generation.
Unlocking MUST require guardian authentication and MUST create an audit event.
The guardian MUST be able to mark an event reviewed or incorrectly classified;
that action MAY inform later evaluation but MUST NOT rewrite the original
decision.

### 13.4 Temporary approval

To create an approval, the guardian MUST re-authenticate or hold a recent strong
authentication session. An approval MUST specify learner, outcomes, start, end,
mode, and approving actor.

At every prompt, the policy engine MUST check that the approval:

- is active and not revoked;
- belongs to the resolved learner;
- includes every otherwise-current outcome needed by the response; and
- is not attempting to override a higher-precedence restriction.

Expiry and revocation take effect on the next policy decision without relying
on cached child state. The mode and remaining duration MUST be visible in the
guardian interface. A future `SUPERVISED` approval MUST cease to apply when its
adult-supervision session ends. Approval use, expiry, and revocation MUST be
audited.

## 14. Safety and tool policy

### 14.1 Safety

Safety classification is independent from curriculum classification. It MUST
cover, at minimum:

- sexual or age-inappropriate content;
- self-harm and suicide-related content;
- violence, bullying, grooming, coercion, and secrecy;
- illegal or dangerous instructions;
- medical, legal, and financial claims;
- personal information;
- contact with unknown third parties;
- purchases and account creation; and
- unsafe or unauthorised external actions.

A curriculum `ALLOW` MUST NOT override a safety restriction. Safety handling
MAY return an age-appropriate supportive response when the safety policy allows
one, but that response remains subject to curriculum and output validation.
Safety reason codes, escalation, and guardian visibility MUST be versioned in a
separate safety policy profile.

### 14.2 Tools

All child-initiated external tools are disabled in the MVP. The **Target**
defaults are:

| Tool | Target default |
| --- | --- |
| Calculator | Allowed through policy |
| Local document search | Restricted |
| Internet browsing | Blocked |
| Code execution | Sandboxed |
| File writing | Restricted |
| Email, messaging, or purchases | Blocked |
| Shell or device control | Blocked |
| Image generation | Age-filtered |
| Camera or microphone | Explicit permission required |

Each future tool MUST be authorised separately. Tool arguments and returned
content MUST pass the same safety, curriculum, assessment, and output policies
as text. The generative model MUST NOT grant itself a tool.

## 15. Local API

### 15.1 Conventions

- The API base path is `/v1`.
- Requests and responses use UTF-8 JSON unless an endpoint states otherwise.
- Guardian and administration endpoints require role-authorised local session
  credentials.
- Child credentials are bound server-side to a learner; a body-provided
  `learner_id` MUST match that binding.
- State-changing guardian requests MUST accept an `Idempotency-Key`.
- Versioned resources MUST use optimistic concurrency. A stale version returns
  `409 Conflict`.
- Policy blocks are successful evaluations and return `200`, not an HTTP error.
- Secrets, raw classifier prompts, and stack traces MUST NOT be returned.

### 15.2 Error contract

Transport, authentication, validation, and service errors use:

```json
{
  "error": {
    "code": "VALIDATOR_UNAVAILABLE",
    "message": "The request could not be safely processed.",
    "request_id": "uuid",
    "retryable": true
  }
}
```

The API MUST use `400` for malformed input, `401` for missing authentication,
`403` for forbidden operations, `404` for inaccessible resources, `409` for
state conflicts, `422` for well-formed invalid data, `429` for rate limits, and
`503` when a required local component is unavailable.

### 15.3 Create learner

```http
POST /v1/learners
```

This guardian-only endpoint creates the learner profile and returns its ID and
resource version. Child chat remains disabled until required curriculum setup
is complete.

### 15.4 Submit child prompt

```http
POST /v1/child/chat
```

```json
{
  "client_message_id": "uuid",
  "conversation_id": "uuid",
  "message": {
    "type": "text",
    "content": "Show me how to solve this equation."
  }
}
```

```json
{
  "message_id": "uuid",
  "decision": "BLOCK",
  "response_mode": "CURRICULUM_REDIRECT",
  "content": "That topic is currently protected. I can help with an earlier prerequisite or a different topic.",
  "warning_level": 1,
  "event_id": "uuid"
}
```

`client_message_id` MUST make retries idempotent so that one learner action
cannot create duplicate generation, events, or warning increments. The child
response MUST NOT expose matched-outcome scores or validator internals.

### 15.5 Set learner outcome state

```http
PUT /v1/learners/{learner_id}/outcomes/{outcome_id}
```

```json
{
  "state": "CURRENT",
  "valid_from": "2026-08-01T00:00:00Z",
  "valid_until": "2026-09-15T00:00:00Z",
  "version": 2
}
```

The endpoint is guardian-only, validates the outcome against the selected
framework version, prevents overlapping state periods, and returns the new
version. For the first explicit state assignment, `version` MUST be `0`; this
asserts that no state record exists even though the derived default is
`UNCLASSIFIED`. A successful initial assignment returns version `1`.

### 15.6 Create temporary protection

```http
POST /v1/learners/{learner_id}/protections
```

```json
{
  "title": "Mathematics assignment",
  "outcome_ids": ["VC2M7A02"],
  "protected_material": [
    {
      "type": "TEXT",
      "content": "Solve the following assignment questions."
    }
  ],
  "valid_until": "2026-08-15T07:00:00Z"
}
```

The request MUST contain at least one outcome or one non-empty text item.
Protected material MUST be ingested into encrypted local storage and converted
to protected-material references. The response MUST identify the protection,
effective interval, and version without echoing the protected text. Document
uploads and non-text material are deferred.

### 15.7 Create or revoke approval

```http
POST /v1/learners/{learner_id}/approvals
DELETE /v1/learners/{learner_id}/approvals/{approval_id}
```

```json
{
  "outcome_ids": ["VC2M7A02"],
  "duration_minutes": 30,
  "mode": "STANDARD",
  "reason": "Guardian-assisted revision"
}
```

Creation MUST reject an empty scope, non-positive or excessive duration,
unrelated learner, or attempt to approve a temporary protection. Revocation is
idempotent.

### 15.8 Retrieve guardian events

```http
GET /v1/guardians/{guardian_id}/events
```

Supported filters are `learner_id`, `severity`, `category`, `from`, `to`, and
`review_status`. Results MUST use cursor pagination, a stable descending
timestamp order, and object-level guardian authorisation.

### 15.9 Session controls

```http
POST /v1/learners/{learner_id}/sessions/{session_id}/suspend
POST /v1/learners/{learner_id}/sessions/{session_id}/unlock
```

Both operations are guardian-only, idempotent, and audited. Unlock MUST NOT
clear escalation history or bypass an active integrity failure.

## 16. Storage, privacy, and audit

### 16.1 Required entities

The data store MUST represent:

```text
Users
Learners
Guardians
Administrators
CurriculumFrameworks
CurriculumOutcomes
LearnerOutcomeStates
ProtectedMaterials
Conversations
Messages (only when retention is enabled)
PolicyDecisions
ValidationResults
Alerts
Approvals
Sessions
AuditEvents
SystemSettings
```

### 16.2 Data locality and minimisation

By default, the system MUST:

- process prompts and responses locally;
- store profiles, curriculum mappings, policies, and events locally;
- deny cloud telemetry and child-data egress;
- include no third-party advertising or behavioural profiling;
- neither sell nor share child data;
- store structured events instead of full transcripts; and
- require explicit adult action for any optional cloud service.

Raw prompts and responses are volatile by default and MUST be discarded after
the response and event are committed. Enabling transcript retention applies
prospectively and MUST show the guardian what is retained and for how long.

### 16.3 Structured events

A policy event MUST contain enough data to reproduce the deterministic decision:

```json
{
  "event_id": "uuid",
  "event_type": "POLICY_BLOCK",
  "learner_id": "uuid",
  "timestamp": "2026-08-05T10:00:00Z",
  "subject": "Mathematics",
  "outcome_ids": ["VC2M7A02"],
  "request_type": "WORKED_SOLUTION",
  "reason_codes": ["CURRENT_CURRICULUM_MATCH"],
  "confidence": 0.94,
  "decision": "BLOCK",
  "warning_level": 2,
  "alert_sent": false,
  "policy_version": "1.0.0",
  "model_version": "string",
  "validator_versions": {}
}
```

The event MUST NOT contain the raw prompt, generated answer, embedding, or
protected source material by default. Event writes MUST be atomic with warning
counter updates so that a crash cannot deliver an answer without recording its
decision.

### 16.4 Retention, export, and deletion

The default retention periods are:

| Data | Default |
| --- | --- |
| Raw message bodies | Not persisted |
| Structured learner events and alerts | 30 days |
| Guardian-configured retained transcripts | 30 days |
| Protected source material | Until protection expiry or deletion; purge within 24 hours |
| Security and administrative audit events | 365 days |

The guardian MUST be able to shorten or extend learner-data retention within
administrator-defined limits. Expired data MUST be deleted automatically.
Export and deletion MUST require guardian re-authentication and MUST themselves
be audited.

Administrative audit records MUST be append-only and tamper-evident during
their retention period. Authorised learner deletion MUST remove child content
and direct identifiers; a minimal non-content erasure receipt MAY remain to
preserve audit integrity.

## 17. Security and integrity

- **SEC-001:** Child, guardian, and administrator authentication contexts MUST
  be separated.
- **SEC-002:** Sensitive data MUST be encrypted at rest and in local transport
  where process boundaries exist.
- **SEC-003:** Credentials and signing keys MUST be stored outside child-facing
  application data, using the operating-system credential facility where
  available.
- **SEC-004:** Policy files and curriculum packages MUST be signed. The local
  installation MAY generate its own root for manually configured MVP data.
- **SEC-005:** Model and future code execution MUST be sandboxed with explicit
  filesystem, memory, process, and network limits.
- **SEC-006:** Network egress MUST be denied for the MVP except during an
  authenticated, explicit update operation outside a child session.
- **SEC-007:** Updates MUST be signature-verified before installation and MUST
  support recovery to the last valid policy and application version.
- **SEC-008:** Guardian and administrator authentication MUST have rate limiting,
  brute-force protection, session expiry, and re-authentication for sensitive
  actions.
- **SEC-009:** Backups MUST be encrypted, integrity-protected, and subject to the
  configured retention policy.
- **SEC-010:** APIs MUST validate schema, size, encoding, and authorisation before
  processing untrusted data.
- **SEC-011:** Policy changes, role changes, exports, deletion, approval, unlock,
  update, and integrity failures MUST be audited.
- **SEC-012:** Child UI assets and requests MUST not expose a route, credential,
  or capability that reaches the raw model endpoint.

At startup, the child service MUST verify:

- policy and curriculum signatures;
- approved validator and model versions;
- policy-engine and output-validator availability;
- guardian controls and credential-store availability;
- event-store writeability and audit integrity; and
- the configured network-egress restriction.

If any required check fails, child chat MUST remain disabled. The same
fail-closed rule applies if a check becomes invalid at runtime.

## 18. Deployment and operations

The architecture SHOULD support Windows, macOS, Linux, a local appliance, and a
school network deployment. Each release MUST state which platforms it has
actually certified; the MVP MUST certify at least one consumer platform.

A deployment MUST include:

- a local child interface and guardian dashboard;
- local API, policy, curriculum, validation, inference, event, and
  administration capabilities;
- an installation-time guardian setup flow;
- documented backup, restore, update, and recovery procedures; and
- a health view that reports component availability without exposing child
  content.

The complete MVP MUST continue to enforce curriculum and safety policy when the
device has no internet connection. Loss of an optional notification channel
MUST NOT affect local blocking, logging, or dashboard alerts.

Configuration, model, curriculum, policy, and validator versions MUST be
included in diagnostic exports. Diagnostic exports MUST exclude child content
and identifiers unless the guardian explicitly includes them.

## 19. Performance

Performance is measured at the 95th percentile after local components are warm,
with the release's supported model, context size, and reference hardware
declared in its test report.

| Operation | Target |
| --- | ---: |
| Prompt normalisation | 500 ms |
| Curriculum classification | 1,000 ms |
| Deterministic policy decision | 100 ms |
| Internal first model token | 3,000 ms |
| Output validation | 1,500 ms |
| Guardian dashboard initial load | 2,000 ms |

The internal first-token target does not permit token display before validation.
The MVP MUST prefer complete-response validation over latency. Validated
chunk-streaming is deferred until it can guarantee that no released chunk or
combination of chunks leaks protected content.

Rate limits and queues MUST be bounded. Under overload, the system MUST reject
or defer work without bypassing validation.

## 20. Verification

### 20.1 Unit tests

Unit coverage MUST include:

- all base-state and overlay transitions;
- policy precedence and most-restrictive aggregation;
- boundary timestamps, approval expiry, and revocation;
- confidence bands and malformed classifier output;
- warning windows, idempotency, alerts, and locks;
- role and object-level permission boundaries;
- retention and deletion rules; and
- stable reason-code mapping.

### 20.2 Integration tests

Integration coverage MUST include:

- the complete prompt-to-response pipeline;
- learner, policy, curriculum, model, and validator failures;
- proof that no output is released before validation;
- one-time rewrite and mandatory revalidation;
- guardian setup, state changes, approvals, and session controls;
- structured event and alert creation;
- assessment matching; and
- offline operation with egress denied.

### 20.3 Curriculum test matrix

For every protected reference outcome, the suite MUST include:

- a direct question;
- a paraphrase;
- a homework-style question;
- a worked-example and worked-solution request;
- a hint and answer-check request;
- a future topic that would leak the current method;
- a previous topic with overlapping terminology;
- an unrelated permitted question; and
- a valid, expired, and revoked approval.

### 20.4 Assessment-protection test matrix

The suite MUST include a protection containing only source text and no mapped
outcome, and a protection containing both source text and outcomes. For each, it
MUST test an exact request, a paraphrase, a request split across turns, an
unrelated permitted request, and behaviour immediately before and after expiry.

### 20.5 Red-team tests

The suite MUST cover prompt injection, roleplay, translation, encoding,
misspelling and substitutions, multi-turn decomposition, future-topic leakage,
lower-level analogy leakage, code generation, tool routing, protected-material
paraphrase, session replay, client retries, and guardian impersonation.

Test cases that expose a policy weakness MUST become regression tests before a
release.

### 20.6 Release quality gates

On a versioned, held-out evaluation set representative of the declared MVP
curriculum, a release MUST achieve:

| Gate | Required result |
| --- | ---: |
| Canonical direct current-outcome blocks | 100% |
| Paraphrased current-outcome detection | At least 95% |
| Exact protected-assessment blocks, including material-only protections | 100% |
| Paraphrased protected-assessment detection | At least 95% |
| Red-team circumvention detection | At least 90% |
| False blocks on clearly permitted cases | At most 5% |
| Responses or tool output released without successful validation | 0 |
| Required-component failure cases that fail closed | 100% |
| Learner attempts that modify guardian or administrator state | 0 successful |
| Expired or revoked approvals that grant access | 0 |

Evaluation reports MUST name the curriculum data, policy, model, validator
versions, thresholds, reference hardware, sample counts, and known limitations.
Safety evaluation MUST be reported separately from curriculum evaluation.

## 21. MVP acceptance criteria

The MVP is acceptable only when all of the following are demonstrated:

1. A guardian can create and configure the single learner profile.
2. The guardian can mark in-scope outcomes `PREVIOUS`, `CURRENT`, `FUTURE`, or
   `UNCLASSIFIED`.
3. Previous and future outcomes are available when they do not leak a current
   outcome.
4. Direct, paraphrased, indirect, and generated current-outcome instruction is
   blocked according to the release quality gates.
5. A guardian can create a temporary assessment protection from text, with or
   without mapped outcomes, and exact and paraphrased requests are blocked
   according to the release quality gates.
6. Unclassified instructional content receives restricted handling.
7. Every prompt and complete candidate response passes the required validators.
8. Repeated blocks and circumvention produce deterministic escalation.
9. Configured events appear in the local guardian dashboard and configured
   thresholds produce alerts.
10. Temporary guardian approvals work only for their scope and expire or revoke
   automatically.
11. The learner cannot change policy, curriculum, approvals, retention, models,
   tools, credentials, or audit events.
12. Raw transcripts are not stored on a default installation.
13. Child chat and all enforcement operate with network egress disabled.
14. Missing or failed required services cause fail-closed responses.
15. Significant policy, security, and guardian actions create structured,
   versioned, tamper-evident audit events.
16. All release quality gates and applicable security checks pass.

## 22. Target roadmap

After the MVP, the product MAY add:

### Phase two

- multiple learners and guardians;
- school-managed profiles and authorised teacher dashboards;
- signed curriculum imports and assessment document ingestion;
- additional subjects and curricula;
- image and voice input;
- mobile alerts and local school-server deployment;
- adaptive local model selection and learning analytics; and
- supervised access workflows involving teachers.

### Phase three

- curriculum knowledge graphs and cross-subject leakage detection;
- federated school policy and signed update distribution;
- hardware appliances and jurisdiction policy packs;
- validator consensus and explainable classification reports;
- independently reviewed validator plug-ins; and
- privacy-preserving aggregate reporting.

Each added modality, tool, role, notification path, or deployment boundary MUST
receive its own threat analysis and MUST preserve the core invariant, local
defaults, deterministic policy enforcement, and pre/post validation.

## 23. Traceability to `idea.md`

| Idea area | Specification sections |
| --- | --- |
| Product, core policy, objectives, non-objectives | 2–4 |
| User roles | 5–6 |
| Profiles, curriculum, and states | 8–9 |
| Prompt pipeline and validators | 9–10 |
| Response modes and leakage | 10–11 |
| Circumvention and assessment protection | 12 |
| Warnings, alerts, dashboard, and approvals | 6 and 13 |
| Safety and tools | 14 |
| Deployment and architecture | 7 and 18 |
| APIs and storage | 15–16 |
| Privacy, logging, and security | 16–17 |
| Fail-closed and performance | 10.8 and 19 |
| Testing and acceptance | 20–21 |
| MVP and later phases | 3 and 22 |
