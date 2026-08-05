# Curriculum-Gated Local AI for Children: Implementation Plan

## 1. Purpose and source authority

This document turns the repository's product intent into an ordered delivery plan.
It uses the sources in this order:

1. [`SPECIFICATION.md`](./SPECIFICATION.md) is normative and takes precedence for
   the MVP.
2. [`IDEA.md`](./IDEA.md) defines the broader product intent and target
   capabilities.
3. [`BRAINSTORM.md`](./BRAINSTORM.md) supplies non-normative risks, experiments,
   experience improvements, and questions that must be resolved explicitly.

No implementation shortcut, model behaviour, usability feature, or operational
fallback may weaken the specification's core invariant. A proposal that changes
that invariant or another normative rule must update the specification, policy
fixtures, threat model, and acceptance tests before implementation.

## 2. Intended MVP outcome

Deliver a local, offline, text-only assistant for one learner and one guardian
that:

- allows previous learning, non-overlapping future learning, and general
  knowledge;
- blocks direct and indirect instruction for current protected outcomes;
- restricts unresolved instructional content;
- protects guardian-entered assessment text and mapped outcomes;
- makes deterministic, versioned policy decisions outside the generative model;
- validates every prompt and every complete candidate response;
- escalates repeated or high-confidence circumvention proportionately;
- provides local guardian controls, structured events, and alerts;
- stores no raw transcript by default;
- exposes no child route to the raw model or administrative controls; and
- remains enforceable with network egress disabled.

The reference evaluation domain is Victorian Curriculum 2.0 Mathematics,
Years 5–8. The framework, version, subject, and outcome range must remain
configuration rather than application code.

## 3. Scope control

### 3.1 Normative MVP scope

| Area | Commitment |
| --- | --- |
| People | One learner, one guardian, and a logically separate administrator role |
| Curriculum | One jurisdiction and immutable curriculum version, with guardian-managed learner outcome states |
| Interaction | Local text chat with bounded, learner-isolated conversation context |
| Inference | Sandboxed local generation and validation |
| Enforcement | Normalisation, safety classification, outcome and assessment matching, circumvention detection, deterministic policy, and output validation |
| Assessment protection | Guardian-entered text, exact and paraphrase matching, mapped or material-only protections, and automatic expiry |
| Controls | Setup, outcome states, protections, finite approvals, retention, export, deletion, suspension, and unlock |
| Escalation | Six internal levels, three child-facing variants, structured events, local alerts, and session lock |
| Operations | One certified consumer platform, signed configuration, offline operation, health checks, backup, restore, update, and rollback |
| Tools and modalities | No child-initiated tools; text only |

### 3.2 Pilot-readiness experience additions

The following brainstormed additions should be treated as a focused
pilot-readiness workstream. Any new API or retention behaviour must be approved
through change control before it becomes an MVP release gate:

- a never-dead-end redirect containing neutral wording and independently
  validated alternatives;
- curated alternatives for each protected reference outcome;
- one-action child review requests that do not count as circumvention;
- question parking without retaining the surrounding conversation;
- child-visible, age-appropriate explanations of policy and recorded metadata;
- guardian false-positive feedback that preserves the original decision;
- policy freshness, broad-change impact, and upcoming-expiry views;
- explicit health and recovery guidance; and
- accessibility acceptance criteria from the first interface release.

Question parking must be an explicit child action. Its title or note is retained
content and therefore needs a visible retention rule, access rule, export path,
and deletion path of its own.

### 3.3 Deferred scope

Do not include the following in the MVP:

- multiple learners or guardians;
- teacher, classroom, or school administration;
- remotely distributed or user-imported curriculum packages;
- document, image, audio, or voice ingestion;
- browsing, calculators, code execution, or any other child-facing tool;
- external email, push, or network notification delivery;
- mobile clients or school-server deployment;
- automatic learning analytics or engagement optimisation;
- curriculum knowledge graphs, cross-subject policy, or graph-generated redirects;
- supervised approvals;
- third-party validator plug-ins; or
- aggregate reporting.

The schemas and authorisation model must leave room for these capabilities
without exposing them in the initial product.

## 4. Delivery principles

Every workstream must apply these rules:

1. **Policy outside the model.** Models emit versioned evidence; only the
   deterministic policy engine grants or denies capability.
2. **Fail closed.** Missing identity, policy, curriculum, integrity, classifier,
   assessment, output-validation, or event-store dependencies produce no
   model-derived child content.
3. **Complete validation before release.** Buffer the whole candidate response,
   validate it, and commit its decision event before displaying content.
4. **Most restrictive result wins.** Safety, temporary protection, protected
   assessment, outcome state, uncertainty, tools, and output findings can only
   maintain or tighten a decision.
5. **Capability over keywords.** Evaluate what an answer would teach, solve,
   check, transform, or execute, including across turns and formats.
6. **Least restrictive safe response.** Permit genuinely non-instructional or
   unrelated content when the evidence supports it; do not use incidental weak
   matches as blanket blocks.
7. **Local and minimal by default.** Deny child-data egress, keep raw turns
   volatile, and store structured decision metadata instead.
8. **Dignity and agency.** Never shame, infer bad intent, or make warning language
   harsher as escalation rises. Explain boundaries and offer a review path.
9. **Safety is independent.** Curriculum permission cannot override child-safety
   policy, and curriculum blocks, safety responses, and service failures must
   remain distinguishable.
10. **Evidence before expansion.** No new subject, language, modality, tool, role,
    or deployment type ships without its own evaluation and threat analysis.
11. **No engagement maximisation.** Product success is useful, independent
    learning rather than time spent or message volume.

## 5. Decisions required before dependent work

Record each decision as an architecture or product decision with owner,
rationale, rejected alternatives, and review trigger.

| ID | Decision and required output | Required before |
| --- | --- | --- |
| D-01 | Select the first certified operating system, minimum reference hardware, packaging approach, and offline update medium. | Runtime and packaging implementation |
| D-02 | Select the application runtime, UI framework, transactional local store, schema tooling, and test framework against local support and maintainability criteria. | Repository scaffolding |
| D-03 | Select and license the local generation model and validator candidates; declare context limits, supported language, quantisation, memory budget, and version identifiers. | Validator and model integration |
| D-04 | Confirm the source, licence, provenance, review process, and signing authority for the reference curriculum data. | Committing or distributing curriculum data |
| D-05 | Define guardian and administrator authentication, re-authentication lifetime, key storage, recovery, and shared-device session switching. | Identity and security implementation |
| D-06 | Approve the initial safety policy, offline jurisdiction resource pack, escalation boundaries, and honest handling of unavailable external help. | Safety validator and child pilot |
| D-07 | Resolve the conflict between the specification's repetition-based level-3 review and the brainstorm's recommendation that benign repetition alone not escalate to review. If changed, revise the specification first. | Final escalation implementation |
| D-08 | Set measurable pilot utility gates for permitted-answer usefulness, redirect continuation, boundary comprehension, dignity, guardian burden, and accessibility. | Child pilot |
| D-09 | Decide which pilot-readiness additions in section 3.2 are release requirements and approve their API, storage, and retention contracts. | Public API freeze |
| D-10 | Approve research ethics, guardian consent, meaningful child assent, withdrawal, deletion, and prohibition on live protected assessments in studies. | Any research involving children |

The strict profile must only be piloted where an accountable adult or educational
programme can provide the protected instruction and review disputes. The MVP
must not be presented as suitable for unsupported learners or as resolving the
risk that its only configured guardian may be unsafe.

## 6. Target implementation shape

### 6.1 Deployment strategy

Use a modular local application for the MVP rather than independent
microservices. Preserve explicit module interfaces and trust boundaries, while
placing the model runtime in a separately sandboxed process. Split modules into
additional processes only where credential, filesystem, memory, or network
isolation requires it.

The required trust zones are:

- child interface;
- guardian interface;
- administrator and operations interface;
- local API gateway and session boundary;
- policy and curriculum plane;
- validator and model runtime;
- encrypted local data and audit storage; and
- local notifications.

The child interface communicates only with the gateway. Model and validator
processes receive minimum request context and no guardian credentials, signing
keys, unrestricted filesystem access, or network access.

### 6.2 Initial repository layout

Create these areas as implementation begins:

- `apps/child-ui` for the learner experience;
- `apps/guardian-dashboard` for setup, policy, events, and data controls;
- `apps/admin-health` or an equivalent separate operations client;
- `services/local-api` for authenticated orchestration;
- `services/model-runtime` for sandboxed local inference;
- `packages/contracts` for API, classifier, event, and persistence schemas;
- `packages/policy-engine` for pure deterministic policy evaluation;
- `packages/normalisation` and `packages/validators` for versioned evidence
  producers;
- `packages/storage` for migrations, repositories, encryption, and lifecycle
  operations;
- `data/curricula`, `data/policies`, `data/templates`, and `data/resources` for
  signed, versioned local packages;
- `tests/unit`, `tests/integration`, `tests/curriculum`, `tests/assessment`,
  `tests/red-team`, `tests/safety`, `tests/accessibility`, and
  `tests/performance`; and
- `docs` for architecture, security, privacy, policy, curriculum, operations,
  evaluation, and decision records.

An internal validator contract is required for the MVP. A public plug-in SDK and
third-party execution model remain deferred.

## 7. Milestone plan

### M0. Evidence, governance, and feasibility

**Goal:** retire the largest educational, licensing, model-quality, hardware, and
safeguarding risks before committing to a production architecture.

- [ ] Create a requirements register linking normative statements to planned
      components, tests, and release evidence.
- [ ] Complete D-01 through D-06 and identify owners for D-07 through D-10.
- [ ] Produce the initial architecture, threat model, privacy impact assessment,
      safeguarding analysis, and accessibility target.
- [ ] Build a small, versioned Years 5–8 mathematics evaluation corpus containing
      protected, permitted, overlapping, prerequisite, future, paraphrased,
      assessment, and multi-turn cases.
- [ ] Separate development, calibration, and held-out release sets and document
      provenance, licences, and intended use.
- [ ] Benchmark candidate local generation and validator models on the minimum
      reference hardware with egress disabled.
- [ ] Verify that candidate classifiers can return schema-constrained,
      calibratable evidence and that the output validator can inspect complete
      responses within the resource budget.
- [ ] Co-design boundary language and curated routes with children, guardians,
      educators, learning specialists, safeguarding specialists, and disability
      advocates under an approved research process.
- [ ] Define utility and dignity measurements without collecting unnecessary
      child content.
- [ ] Document the strict profile's support assumptions and known safeguarding
      limitations.

**Exit gate:** approved foundational decisions, lawful curriculum and model
sources, a credible path to the specification's quality gates on declared
hardware, and no unresolved high-severity threat that invalidates the proposed
MVP.

### M1. Repository, contracts, and quality foundation

**Goal:** establish stable contracts and repeatable validation before feature
development.

- [ ] Scaffold the selected workspace, applications, packages, test areas, and
      local development environment.
- [ ] Add reproducible builds, locked dependencies, licence inventory, static
      checks, unit tests, integration tests, secret scanning, and dependency
      vulnerability checks.
- [ ] Define canonical enums and schemas for states, request types, decisions,
      reason codes, response modes, warning levels, relationships, classifier
      results, output-validation results, API errors, and events.
- [ ] Publish the `/v1` API contract, including role requirements, idempotency,
      optimistic concurrency, cursor pagination, and stable error semantics.
- [ ] Define immutable version identifiers for application, policy, curriculum,
      model, validator, template, and resource packages.
- [ ] Provide deterministic test controls for time, identifiers, policy versions,
      and component failures.
- [ ] Establish fixture generation and schema compatibility tests so malformed or
      unknown validator fields always fail safely.
- [ ] Add a requirements-to-test traceability check.

**Exit gate:** clean bootstrap on the selected platform, all contracts reviewed,
and CI able to build and test with network access disabled after dependencies and
artifacts are provisioned.

### M2. Identity, curriculum, persistence, and audit foundation

**Goal:** implement the trusted local state on which policy decisions depend.

- [ ] Implement separate learner, guardian, and administrator authentication
      contexts, including rate limits, expiry, brute-force controls, and strong
      re-authentication for sensitive actions.
- [ ] Bind each child session server-side to exactly one learner and isolate all
      session and conversation context.
- [ ] Enforce object-level guardian authorisation and keep guardian and
      administrator permissions logically distinct even when one adult holds
      both roles.
- [ ] Implement the learner setup gate so chat remains disabled until identity,
      framework, outcomes, policy, retention, and safety settings are complete.
- [ ] Implement immutable curriculum frameworks and outcomes with versioned,
      same-version relationships and provenance.
- [ ] Implement time-versioned learner outcome states, the derived
      `UNCLASSIFIED` default, half-open validity intervals, overlap rejection,
      and optimistic concurrency.
- [ ] Implement finite temporary protections, encrypted protected text and
      derived representations, mapped and material-only protections, automatic
      effective expiry, and purge within the required interval.
- [ ] Implement finite `STANDARD` approvals, re-authentication, named outcome
      scope, maximum duration, revocation, and automatic effective expiry.
      Reject `SUPERVISED` rather than treating it as `STANDARD`.
- [ ] Implement sessions, conversations, policy decisions, validation results,
      alerts, settings, and append-only administrative audit events.
- [ ] Implement prospective transcript retention, default volatile message
      bodies, configurable retention, automatic deletion, readable export,
      authorised erasure, and minimal erasure receipts.
- [ ] Make security-sensitive event history tamper-evident and verify it at
      startup.

**Exit gate:** migration, boundary-time, concurrency, authorisation, encryption,
retention, export, deletion, and audit-integrity tests pass; no learner request
can read or mutate another trust zone's state.

### M3. Deterministic policy kernel

**Goal:** prove policy behaviour independently of any model or UI.

- [ ] Implement a pure policy input and output contract containing only
      validated, versioned facts.
- [ ] Resolve effective state in the required order: integrity and lock, safety,
      temporary protection and assessment, approval, base state, uncertainty,
      tool policy, and post-generation findings.
- [ ] Aggregate all materially matched outcomes and select the most restrictive
      result; never let approval for one outcome exempt another.
- [ ] Implement exact base-state behaviour, including restricted handling for
      unresolved instructional content and normal handling for confidently
      unrelated requests.
- [ ] Implement calibrated confidence bands as versioned policy data.
- [ ] Implement stable decisions, reason codes, response modes,
      `allow_model_generation`, and child-safe disclosure rules.
- [ ] Implement the six escalation levels, rolling windows, related-attempt
      grouping, high-confidence circumvention thresholds, tampering lock, and
      deterministic retry handling after D-07 is resolved.
- [ ] Ensure safety policy remains independently versioned and cannot be relaxed
      by curriculum state or approval.
- [ ] Define system-owned, signed boundary, approval, lock, safety, and service
      templates. Redirect alternatives must be curated and validated rather than
      improvised by the model.
- [ ] Permit at most one rewrite and require the rewritten candidate to traverse
      the complete output-validation path again.

**Exit gate:** exhaustive policy tables and property tests cover precedence,
state overlays, mixed outcomes, confidence boundaries, validity timestamps,
approval scope, escalation, retries, reason codes, and every fail-closed input.
The same versioned input always produces the same decision.

### M4. Normalisation, matching, validation, and local inference

**Goal:** produce calibrated evidence for the policy engine without granting any
validator policy authority.

- [ ] Implement bounded input size and encoding validation; normalise control and
      invisible characters, spacing, punctuation, substitutions, and common
      encodings while retaining an ephemeral explanation mapping.
- [ ] Detect language and return a neutral limitation for unsupported
      instructional languages without treating multilingual use itself as
      circumvention.
- [ ] Resolve references only within the authenticated learner session's bounded
      recent context and identify quoted assessment text.
- [ ] Implement versioned safety classification separately from curriculum
      classification.
- [ ] Implement request-type, instructional-capability, subject, and outcome
      classification with schema validation and calibrated confidence.
- [ ] Combine normalised lexical evidence, reviewed curriculum relationships,
      semantic similarity, outcome classification, conversation context,
      assessment similarity, and candidate-response analysis.
- [ ] Report every material outcome candidate, confidence, and relationship while
      discarding unsupported incidental weak matches.
- [ ] Implement exact hashes or fingerprints, key-phrase evidence, semantic
      representations, and outcome mappings for protected assessment text.
- [ ] Implement conversation-aware circumvention evidence for roleplay,
      translation, encoding, decomposition, policy injection, alternate formats,
      tool routing, and impersonation.
- [ ] Run prompt and output validation as independently invoked stages even if
      one underlying model supplies more than one capability.
- [ ] Sandbox the generation and validator runtime with explicit process,
      filesystem, memory, context, and network limits.
- [ ] Buffer the complete candidate response and validate direct, indirect,
      transitive, transformed, code-mediated, assessment, safety, privacy, and
      tool leakage before release.
- [ ] Treat timeouts, malformed output, missing versions, low-confidence
      secondary-validator failure, or unavailable protected-material matching as
      validator failure.
- [ ] Version and calibrate thresholds against development data without exposing
      the held-out release set.

**Exit gate:** component benchmarks meet provisional protection, false-block,
latency, and resource targets on reference hardware; tests prove no validator
text is interpreted as an instruction and no candidate content is emitted before
approval.

### M5. Local API and end-to-end policy pipeline

**Goal:** connect trusted state, evidence producers, policy, inference, and events
through the only supported child path.

- [ ] Implement the API gateway, role resolution, learner binding, schema and
      size validation, request IDs, and safe error contract.
- [ ] Implement `POST /v1/learners` and keep chat disabled until setup is
      complete.
- [ ] Implement versioned learner outcome updates with first-write version `0`
      semantics and overlap checks.
- [ ] Implement protection, approval creation and revocation, guardian event
      retrieval, session suspend, and session unlock endpoints.
- [ ] Implement `POST /v1/child/chat` as the exact ordered pipeline in the
      specification, with no alternate UI, API, or internal bypass.
- [ ] Make `client_message_id` idempotent across generation, event creation,
      warning counters, and returned results; apply `Idempotency-Key` to guardian
      mutations.
- [ ] Re-evaluate live policy, protections, approval, and lock state on every
      turn rather than trusting cached child state.
- [ ] Commit the decision event and escalation update atomically before returning
      approved content.
- [ ] Return policy blocks as successful evaluations while keeping transport,
      authentication, validation, and service failures distinct.
- [ ] Add startup and continuous readiness checks for signatures, approved
      versions, required components, credential storage, event writeability,
      audit integrity, and egress controls.
- [ ] Provide signed system-owned responses for blocked and unavailable paths
      without calling the generative model.

**Exit gate:** end-to-end tests cover every allow, restrict, block, approval,
rewrite, lock, retry, outage, and integrity path; fault injection shows that no
required-component failure bypasses validation.

### M6. Child, guardian, and administrator experiences

**Goal:** expose the system safely, accessibly, and without turning controls into
surveillance.

#### Child experience

- [ ] Build accessible local chat with clear session identity, response status,
      natural stopping points, and no direct model endpoint or administrator
      capability.
- [ ] Explain in age-appropriate language that the model can be wrong, is not a
      person, has learning boundaries, and does not need private information.
- [ ] Distinguish curriculum boundaries, adult approval, safety responses,
      session lock, and technical unavailability.
- [ ] Implement the three child-facing warning variants with calm, non-accusatory
      wording.
- [ ] Implement validated never-dead-end routes and any approved question-parking
      and review-request flows from D-09.
- [ ] Show what category of information is retained or visible to the guardian
      without revealing detection internals.
- [ ] Support keyboard-only use, screen readers, readable contrast and motion,
      adjustable response presentation, and perceivable fail-closed states.

#### Guardian experience

- [ ] Build guided setup for learner, framework, outcome states, safety,
      escalation, and retention.
- [ ] Provide outcome state, date-range, temporary protection, approval,
      suspension, unlock, and policy-freshness controls.
- [ ] Preview broad policy changes and warn about unusually large unclassified or
      protected areas.
- [ ] Show grouped structured events, alerts, related counts, reason codes,
      outcome states, confidence, validator versions, action taken, and review
      status without raw text by default.
- [ ] Implement review and false-positive feedback without modifying the
      immutable original decision.
- [ ] Provide re-authenticated retention, prospective transcript, export, and
      deletion controls with clear child-visible effects.
- [ ] Lead with system health, stale policy, unresolved classifications, upcoming
      expiries, and support actions rather than infraction counts.
- [ ] Exclude obedience scores, emotion or personality inference, child
      comparisons, hidden sentiment analysis, and engagement metrics.

#### Administrator experience

- [ ] Provide separate authentication for health, approved version inventory,
      local update, rollback, backup, restore, and diagnostic export.
- [ ] Prevent administrative operations from silently weakening policy and audit
      every security- or policy-relevant change with before and after versions.
- [ ] Exclude child content and direct identifiers from diagnostics unless a
      guardian explicitly authorises inclusion.

**Exit gate:** role, object-access, accessibility, age-appropriate content,
privacy comprehension, setup usability, and broad-change safety tests pass. The
child UI contains no route or credential that can reach the model, store, or
administrative plane directly.

### M7. Security, privacy, and operational hardening

**Goal:** turn logical safeguards into verified deployment properties.

- [ ] Store credentials and signing keys outside child-facing data using the
      selected operating-system credential facility.
- [ ] Encrypt sensitive data at rest and local transport across process
      boundaries; isolate and purge protected text, embeddings, and fingerprints.
- [ ] Sign and verify curriculum, policy, templates, models, validators, resource
      packs, and application updates as appropriate.
- [ ] Deny network egress for all child-processing components and prove the rule
      under normal operation, failure, restart, and update.
- [ ] Apply sandbox, least privilege, request limits, bounded queues, rate limits,
      and safe overload behaviour.
- [ ] Complete abuse cases for child-to-guardian privilege escalation, guardian
      credential compromise, policy tampering, model prompt injection, local
      storage theft, update compromise, rollback, and shared-device confusion.
- [ ] Implement encrypted, integrity-protected backup and tested restore without
      extending retention silently.
- [ ] Implement signed update verification, recovery to the last valid release,
      offline update documentation, and compromised-key handling.
- [ ] Validate automatic expiry and deletion jobs, including purge of derived
      protected-material representations.
- [ ] Provide content-free health and diagnostic views with all policy,
      curriculum, model, validator, configuration, and application versions.
- [ ] Complete independent security, privacy, accessibility, and safeguarding
      review before pilot.

**Exit gate:** no unresolved critical or high-severity finding; startup and
runtime integrity failures disable child chat; backup, restore, update, rollback,
retention, deletion, and egress-denial exercises pass on the certified platform.

### M8. Verification and release qualification

**Goal:** demonstrate the complete product claim on held-out data and supported
hardware.

- [ ] Complete unit coverage for state and overlay transitions, precedence,
      confidence, malformed evidence, timestamps, approvals, escalation,
      idempotency, authorisation, retention, and reason codes.
- [ ] Complete integration coverage for the full pipeline, component outages,
      buffered release, one rewrite, setup, protections, approvals, session
      controls, events, alerts, and offline operation.
- [ ] Run the curriculum matrix for every protected reference outcome across
      direct, paraphrased, homework, example, solution, hint, checking, indirect
      future, overlapping previous, unrelated, and approval cases.
- [ ] Run mapped and material-only assessment matrices for exact, paraphrased,
      translated, split-turn, unrelated, and expiry-boundary cases.
- [ ] Run red-team coverage for injection, roleplay, encoding, substitutions,
      translation, multi-turn assembly, indirect leakage, code, tools, replay,
      duplicate clients, and impersonation.
- [ ] Run child-safety tests for dependency cues, secrecy, coercion, unsafe
      authority claims, personal data, crisis-resource freshness, and the known
      single-guardian limitation.
- [ ] Run accessibility and usability studies for boundary comprehension,
      dignity, review, guardian setup, policy freshness, and supported language.
- [ ] Run performance and resource tests at the 95th percentile after warm-up on
      declared hardware and model settings.
- [ ] Promote every discovered policy weakness to a versioned regression case.
- [ ] Produce separate curriculum, safety, security, privacy, accessibility,
      performance, and utility reports.

The release must meet all normative quality gates:

| Gate | Required result |
| --- | ---: |
| Canonical direct current-outcome blocks | 100% |
| Paraphrased current-outcome detection | At least 95% |
| Exact protected-assessment blocks, including material-only protection | 100% |
| Paraphrased protected-assessment detection | At least 95% |
| Red-team circumvention detection | At least 90% |
| False blocks on clearly permitted cases | At most 5% |
| Output released without successful validation | 0 |
| Required-component failures that fail closed | 100% |
| Learner attempts that modify guardian or administrator state | 0 successful |
| Expired or revoked approvals that grant access | 0 |

The evaluation report must identify datasets, sample counts, thresholds,
curriculum, policy, application, model and validator versions, supported
language, hardware, context size, resource use, and known limitations. Utility
gates from D-08 are additional pilot go/no-go criteria and cannot be replaced by
high block rates.

**Exit gate:** every specification acceptance criterion and numeric gate passes
on a fresh offline installation, all pilot utility gates pass, and independent
review findings are resolved or explicitly accepted at the appropriate
governance level.

### M9. Packaging, pilot, and MVP release

**Goal:** release only the product that was evaluated, with honest claims and a
safe recovery path.

- [ ] Produce signed, reproducible artifacts pinned to the evaluated application,
      model, validator, curriculum, policy, template, and resource versions.
- [ ] Include installation, guardian setup, offline operation, backup, restore,
      update, rollback, retention, deletion, diagnostics, and incident guidance.
- [ ] Publish model and validator cards, evaluation reports, minimum hardware,
      supported platform and language, limitations, and responsible-disclosure
      process.
- [ ] Validate a fresh install, upgrade, failed upgrade, rollback, restore, and
      complete offline session using release artifacts.
- [ ] Stage rollout through internal synthetic evaluation, adult sandbox use,
      supervised usability work without live assessments, and only then an
      ethics-approved limited child pilot.
- [ ] Stop rollout if protection, utility, dignity, accessibility, privacy,
      guardian-burden, or hardware gates regress.
- [ ] Collect no cloud telemetry by default. Use explicit, minimised,
      guardian-authorised local exports for diagnostics or research.
- [ ] Describe the release as a bounded MVP, not impossible to bypass, guaranteed
      to prevent cheating, suitable for every learning context, or a replacement
      for teachers and trusted adults.

**Exit gate:** the released package hashes match the qualified artifacts, all
documentation and recovery paths are verified, and the release decision records
the evidence and known limitations.

## 8. Delivery dependencies and parallel work

| Work | Depends on | May proceed in parallel with |
| --- | --- | --- |
| M0 evidence and decisions | Repository sources | Boundary co-design, threat analysis, hardware and model benchmarking |
| M1 contracts and scaffolding | D-01 and D-02 | Remaining M0 research |
| M2 trusted state | M1, D-04, D-05 | M3 policy kernel and M4 validator research |
| M3 policy kernel | M1, D-07 before final escalation | M2 and M4 using versioned fixtures |
| M4 validators and runtime | M0 model evidence, M1 contracts, D-03, D-06 | M2 and M3 |
| M5 end-to-end pipeline | M2, M3, and M4 | Interface prototypes that use mock contracts |
| M6 interfaces | M5 stable API, D-09 | Ongoing M7 hardening |
| M7 hardening | Starts with M1 and spans all milestones | M2 through M6 |
| M8 qualification | Complete integrated M2 through M7 | Documentation and release packaging preparation |
| M9 release | M8 and D-08 through D-10 | No feature development on the release candidate |

Use vertical slices to expose integration risk early:

1. authenticated child request to deterministic system-owned boundary response;
2. previous-outcome allow through buffered local generation and output validation;
3. current-outcome block with atomic event and warning;
4. material-only assessment match;
5. finite approval and automatic expiry;
6. repeated circumvention through alert and lock;
7. validator outage and integrity failure through fail-closed UI; and
8. guardian setup, review, export, deletion, backup, and restore.

## 9. Cross-cutting definition of done

A work item is complete only when:

- its contract and authoritative requirement are identified;
- role and object-level authorisation are enforced;
- all inputs, outputs, sizes, encodings, and versions are validated;
- failure and retry behaviour is deterministic and fail closed;
- structured events contain enough versioned metadata to reproduce the decision
  without raw child content;
- raw prompts, responses, embeddings, and protected material follow the approved
  locality, encryption, retention, and deletion rules;
- child-facing language is age-appropriate, non-shaming, and does not expose
  security internals;
- accessibility is tested, not deferred;
- unit, integration, negative, boundary, and relevant red-team tests pass;
- threat, privacy, safeguarding, and operational impacts are reviewed;
- documentation and traceability are updated; and
- no unqualified language, subject, modality, tool, or fallback path is enabled.

## 10. Required repository deliverables

Before MVP release, the repository should contain:

- an updated `README.md` with bounded product claims and supported release scope;
- `ARCHITECTURE.md`, `SECURITY.md`, `PRIVACY.md`, `POLICY_MODEL.md`,
  `CURRICULUM_SCHEMA.md`, `THREAT_MODEL.md`, `CONTRIBUTING.md`, and a verified
  `LICENSE`;
- API and validator contracts, database migrations, and signed schema definitions;
- provenance-approved sample curriculum, policy, response-template, and offline
  safety-resource data;
- unit, integration, curriculum, assessment, safety, red-team, accessibility,
  performance, and release-evaluation suites;
- local installation, egress-control, backup, restore, update, rollback,
  retention, export, deletion, and recovery instructions;
- model, validator, curriculum, policy, and evaluation cards or reports;
- a responsible-disclosure policy and regression-case intake process;
- a secret-free `.env.example` only if environment configuration is selected; and
- `docker-compose.yml` only if containers are part of development or the
  certified deployment design. A development composition is not itself an MVP
  deployment certification.

## 11. Primary risks and controls

| Risk | Required control | Release effect |
| --- | --- | --- |
| Protected instruction is allowed | Independent prompt and output validation, complete buffering, held-out tests, regression promotion | Blocks release if leakage gates fail |
| Permitted curiosity is over-blocked | Capability-aware evidence, curated alternatives, false-block and utility gates | Blocks pilot if utility or false-block gates fail |
| Curriculum state is stale or too broad | Freshness indicators, review reminders, impact previews, finite overlays | Requires guardian correction before affected pilot use |
| Guardian setup is too difficult | Guided setup, signed reference data, usability study, unresolved-state visibility | Blocks pilot if guardian-burden gate fails |
| Benign repetition is treated as misconduct | Resolve D-07, non-shaming wording, one-action review, accessibility testing | Blocks final escalation release until resolved |
| Validator or model update drifts | Version pinning, signed artifacts, held-out comparison, rollback | Rejects the update |
| Local hardware cannot enforce safely | Minimum safety floor, bounded load, no reduction of validation | Disables child chat on unsupported hardware |
| Detection becomes surveillance | Volatile transcripts, structured events, prospective retention, child-visible policy | Blocks release on privacy review failure |
| Guardian credential is stolen or misused | Re-authentication, key isolation, audit, visible effects, bounded approvals | Security issue blocks release; legitimate misuse remains a disclosed limit |
| Only guardian may be unsafe | Honest limitation, offline resources, separate future threat analysis | Prohibits unsupported safety claims |
| Learner lacks human instructional support | Strict-profile deployment condition and future low-support profile research | Prohibits an “everywhere” or self-directed-learning claim |
| Language or disability bias | One declared validated language, accessibility criteria, no intent escalation from uncertainty | Unsupported use receives neutral limitation; failed equity gates block expansion |
| Protected fingerprints leak | Encryption, isolation, minimisation, rotation, expiry purge, recovery tests | Security or deletion failure blocks release |
| Child moves to an unsafe external assistant | Compelling permitted experience and utility go/no-go gates | Failed utility gate pauses rollout |

## 12. Post-MVP roadmap

### Phase two

After the MVP is stable and independently evaluated, consider:

- multiple learners and guardians;
- school-managed profiles and authorised teacher views;
- signed curriculum imports and protected document ingestion;
- additional subjects, jurisdictions, and independently qualified languages;
- image and voice input;
- local school-server deployment and optional external guardian alerts;
- adaptive model selection and carefully governed learning analytics; and
- supervised access with authenticated adult-presence sessions.

Each addition requires schema evolution, migration, role and consent review,
threat analysis, privacy review, accessibility coverage, and release gates equal
to the text-only MVP.

### Phase three

Only after phase-two evidence supports expansion, consider:

- reviewed curriculum dependency graphs and cross-subject leakage detection;
- federated school policy and signed update distribution;
- hardware appliances and jurisdiction-maintained policy packs;
- validator consensus and explainable classification reports;
- independently reviewed validator plug-ins; and
- privacy-preserving aggregate reporting.

Model-inferred graph edges remain candidate evidence until reviewed, versioned,
and given provenance. No graph, plug-in, aggregate report, or institutional
policy may weaken local defaults, family privacy, deterministic enforcement, or
pre- and post-generation validation.

## 13. Traceability summary

| Source area | Primary milestones |
| --- | --- |
| Product, scope, roles, and workflows | M0, M2, M6, M9 |
| Architecture and trust boundaries | M1, M5, M7 |
| Curriculum and learner data | M2, M3 |
| Policy precedence and confidence | M3 |
| Prompt, model, and output pipeline | M4, M5 |
| Assessment and circumvention | M3, M4, M8 |
| Warnings, alerts, locks, and approvals | M2, M3, M5, M6 |
| Safety and tool denial | M3, M4, M7, M8 |
| API, storage, privacy, and audit | M1, M2, M5, M7 |
| Deployment, integrity, and performance | M0, M7, M8, M9 |
| Verification and acceptance | M8 |
| Wonder, dignity, agency, and accessibility | M0, M6, M8 |
| Low-support, unsafe-guardian, and governance limits | M0, M8, M9 |
| Later product phases | Post-MVP roadmap |

The implementation is complete only when the traceability register links every
normative MVP requirement to a delivered component, an automated or documented
verification method, and release evidence.
