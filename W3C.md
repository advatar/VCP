# Discussion Proposal — Verifiable Contribution Provenance for Web Content

**VCP working draft v0.2 — participant input for the W3C Content and Originator Authenticity Community Group**

**Meeting:** [Kickoff of the Content and Originator Authenticity CG](https://www.w3.org/events/meetings/f25640e1-5025-4d05-92de-e1bd4b41b656/), TPAC, 28 October 2026.

**Status:** Author's discussion proposal. This document has not been adopted by the Community Group and is not a W3C specification or Recommendation. SHALL, SHOULD and MAY express candidate VCP requirements for review, not requirements agreed by the group.

## Meeting brief

### Suggested opening

We are developing Verifiable Contribution Provenance (VCP) to describe how people, AI models and agents contribute to digital content, with evidence and explicit delegation. For a web resource, we want to distinguish its publisher, its contributors, their authority and the exact content state covered by each attestation. Could the group use a mixed human–AI publishing example to examine how existing identity and content-provenance mechanisms connect, and where web integration still needs work?

### Proposed contribution to the group's work

The [COA CG mission](https://www.w3.org/community/coa-cg/) brings identity attestations, content provenance and web integration into one discussion. Its initial outputs are explanatory reports, with the possibility of later specification incubation. This proposal offers a contribution-workflow use case and an evidence model as input to that work.

The meeting's agenda invites stakeholder perspectives and priorities. The immediate request is to:

1. Include a human–AI publishing workflow in the group's use-case collection.
2. Compare how C2PA, CAWG, Originator Profile, Verifiable Credentials and W3C PROV address the workflow, identifying what can already be reused and what remains unresolved.
3. Examine resource-to-evidence association, exact-version binding and safe user-agent presentation as candidate web integration topics.
4. Invite publisher, browser, identity, provenance and privacy implementers to review a small interoperability experiment.

### The question VCP brings

> Who or what contributed to this exact content state, in which role, under whose authority, and what evidence supports each assertion?

A publisher identity, a valid signature and a contribution claim answer different questions. A site can publish material it did not create; a verified person can be one of many contributors; an agent can act under a person's delegation without becoming that person. The proposed model preserves these distinctions.

### First web use case

A publisher releases an article containing a human-written analysis and an AI-generated illustration. An editor revises the text and approves publication. A reader follows the article URL and wants to inspect the provenance of the version being displayed.

A candidate experiment would associate that resource with an evidence bundle bound to its exact content state. The bundle would describe the author, model, editor and publisher as separate Actors; distinguish generation, editing and publication; and reference optional third-party identity attestations and explicit delegation. A user agent would show which checks succeeded, which evidence was unavailable and which assertions remain disputed. Corrections and later versions would retain their own history.

The same use case must work with a pseudonymous author, withheld prompts and no identity credential. Authenticity signals must not imply that the article is accurate, legally owned or endorsed by the browser.

### Reading guide

- Sections 1–18 set out the proposed contribution model and its privacy and integrity principles.
- Section 19 identifies interoperability questions for existing work.
- Section 25 proposes COA CG work items and a bounded experiment.
- Section 31 collects questions about URLs, HTTP, HTML and user agents.
- Section 30 records specification work still needed before any interoperability claim.

The proposal is **Verifiable Contribution Provenance (VCP)**. The [Semantic Event companion draft](SEMANTIC_EVENT.md) describes contribution activities across domains. Identifiers under `https://example.org/vcp/` are illustrative placeholders, not assigned standard namespaces or network dependencies. This proposal is independent of any product, hosting platform or identity service.

---

## 1. Purpose

Digital artifacts are increasingly created through interactions between humans, software tools, artificial intelligence systems and autonomous agents.

Human–AI workflows require several kinds of provenance and identity evidence to be connected. Existing standards provide important building blocks; this proposal asks how their coverage and mappings apply to contribution workflows on the web. A digital artifact may now result from a chain involving human instructions, AI generation, autonomous delegation, model invocations, selection, editing, transformation and composition.

This document proposes a technology-neutral model for representing **verifiable contribution provenance across such workflows**, offered for comparison with existing work in the COA CG.

The standard is intended to answer three distinct questions:

1. **Identity — Who or what participated?**
2. **Authority — On whose authority did they act?**
3. **Provenance — What happened?**

The standard should enable claims concerning these questions to be accompanied by evidence sufficient for independent verification.

It should not prescribe a particular commercial platform, identity provider, distributed ledger, artificial intelligence provider, transport protocol or serialization format. Cryptographic mechanisms remain agile at the abstract-model layer. A VCP security profile SHALL prescribe an interoperable algorithm set and SHALL NOT permit a quantum-vulnerable signature as its sole authenticity mechanism. These are candidate VCP security requirements; their evaluation is separate from the initial web association and semantic mapping study.

---

## 2. Scope

The proposed VCP specification defines an abstract information model and candidate conformance requirements for representing the provenance of digital artifacts created, modified, transformed, selected or otherwise affected by humans, software systems, AI models and autonomous agents.

It covers:

- identification of actors and artifacts;
- actions performed upon or resulting in artifacts;
- human and machine contributions;
- relationships between source and derived artifacts;
- claims concerning provenance;
- evidence supporting those claims;
- attestations by actors and third parties;
- identity evidence and levels of identity assurance;
- delegation and authority;
- AI model and agent participation;
- integrity and cryptographic verification;
- correction, supersession, revocation and dispute;
- privacy and selective disclosure;
- extensibility and interoperability;
- conformance requirements.

The standard does **not** prescribe:

- a particular serialization such as XML or JSON;
- a particular transport protocol;
- a particular identity system;
- a particular public-key infrastructure;
- blockchain or distributed-ledger technology;
- a particular AI provider or model architecture;
- a particular rights-management regime;
- a particular application or commercial implementation.

### 2.1 Relationship to COA CG scope

The proposed contribution to the CG is the association and interpretation of contribution evidence for web content. The abstract model is offered as a candidate vocabulary and source of requirements; it does not assume that the CG should adopt a new core standard.

The web integration discussion should distinguish the originator of content from an HTTP origin, a hosting service, a publisher and a credential issuer. Control of a URL or an origin does not, by itself, establish who created the content.

Content truthfulness, legal ownership and entitlement, compulsory identity verification, a global reputation score and a new universal identity service are outside this proposal's scope. Pseudonymous publication and publication without provenance metadata remain valid use cases.

---

## 3. Design Principles

### 3.1 Semantics before serialization

The normative foundation of a VCP specification SHALL be an abstract information model.

XML, JSON, CBOR, JSON-LD or other representations MAY be defined as bindings of that model but SHALL NOT define the semantics of the model itself.

### 3.2 Claims are not facts

A provenance assertion SHALL be represented as a **Claim**.

The existence of a Claim does not establish its truth.

A Claim MAY reference Evidence, Attestations and other Claims supporting or contradicting it.

This permits provenance systems to represent uncertainty, disagreement and incomplete evidence without rewriting historical records.

### 3.3 Identity, authority and provenance are distinct

The standard SHALL distinguish:

**Identity** — who or what an Actor is;

**Authority** — the basis on which an Actor is permitted to act;

**Provenance** — what occurred.

Establishing an Actor's identity SHALL NOT implicitly establish that Actor's authority to perform an Action.

### 3.4 Humans and machines are first-class actors

The model SHALL support at least:

- Human Actors;
- Organizational Actors;
- Software Actors;
- AI Model Actors;
- Agent Actors;
- Device Actors.

The nature of an Actor SHALL be explicitly representable.

### 3.5 Evidence should be independently verifiable

Where technically possible, provenance Claims SHOULD be accompanied by Evidence allowing a relying party to independently assess them.

### 3.6 Open implementation

The standard SHOULD be implementable without dependence upon a proprietary service, proprietary identifier namespace or commercial platform.

### 3.7 Privacy by design

Public provenance SHALL NOT require public disclosure of legal identity or possession of a verified identity credential. A relying party MAY evaluate optional identity attributes separately from content integrity and provenance.

The standard SHALL permit pseudonymous participation, selective disclosure and separation of identity evidence from publicly exchanged provenance.

---

## 4. Conceptual Model

The initial core model consists of nine principal concepts:

```text
Actor
  │
  │ performs
  ▼
Action
  │
  ├──── consumes/references ───► Artifact
  │
  └──── produces/modifies ─────► Artifact
                                      │
                                      │ described by
                                      ▼
                                    Claim
                                      │
                    ┌─────────────────┼────────────────┐
                    ▼                 ▼                ▼
                 Evidence        Attestation       Authority
```

The core entities are:

1. **Actor**
2. **Artifact**
3. **Action**
4. **Claim**
5. **Evidence**
6. **Attestation**
7. **Authority**
8. **Identity Evidence**
9. **Relationship**

Domain-neutral semantic refinement of an Action or artifact state transition is defined by the companion **SemanticEvent** profile in [SEMANTIC_EVENT.md](SEMANTIC_EVENT.md). A SemanticEvent does not replace Action or Claim semantics and SHALL NOT be interpreted as proof that the described event occurred.

---

## 5. Actor

An Actor is an entity capable of participating in an Action.

An Actor SHALL have a stable identifier within the relevant provenance context.

An Actor MAY represent:

```text
Human
Organization
Agent
AI Model
Software
Device
Service
```

An Actor identifier SHALL NOT necessarily reveal the legal identity of the Actor.

For example:

```text
Actor
    identifier: urn:example:actor:7f82...
    type: Human
```

may subsequently be associated with independently verifiable Identity Evidence.

---

## 6. Human Identity

The standard SHALL NOT define a new universal human identity system.

Instead, an Actor MAY have one or more **Identity Claims**, each supported by Identity Evidence.

Identity mechanisms may include:

- self-asserted identity;
- persistent cryptographic identity;
- passkeys or equivalent device-bound credentials;
- organizational credentials;
- professional credentials;
- national electronic identity;
- selectively disclosed identity credentials;
- other authoritative identity systems.

Identity assurance SHALL be represented independently from the provenance Claim.

For example:

```text
Actor
    ↓
Identity Claim
    "Actor is Alice Example"
    ↓
Identity Evidence
    issuer
    credential type
    assurance
    validity
    cryptographic proof
```

The standard SHOULD permit a relying party to establish that two Actions were performed by the same verified Actor without requiring disclosure of that Actor's legal identity.

---

## 7. Artifact

An Artifact is an identifiable digital object or state to which provenance information can refer.

Examples include:

- source code;
- documents;
- datasets;
- images;
- video;
- audio recordings;
- model outputs;
- model checkpoints;
- prompts;
- compositions;
- designs;
- intermediate representations;
- derived works.

Artifacts MAY be identified using cryptographic digests, persistent identifiers, content identifiers or other mechanisms.

Artifact identity SHALL be distinct from storage location.

---

## 8. Action

An Action represents an occurrence involving one or more Actors and zero or more Artifacts.

Examples include:

```text
Create
Generate
Record
Transform
Edit
Mix
Combine
Select
Reject
Review
Approve
Publish
Train
Fine-tune
Invoke
Delegate
```

Actions MAY consume existing Artifacts and MAY produce new Artifacts.

For example:

```text
Human Actor
      │
      │ instructs
      ▼
Agent Actor
      │
      │ invokes
      ▼
AI Model
      │
      │ generates
      ▼
Artifact A
      │
Human Actor edits
      ▼
Artifact B
```

The provenance graph preserves each relationship rather than collapsing the process into the assertion that the Human Actor "created Artifact B".

Where domain-specific semantic meaning is required, an Action MAY be refined by a SemanticEvent. The SemanticEvent core vocabulary and domain activity vocabularies are defined in [SEMANTIC_EVENT.md](SEMANTIC_EVENT.md); implementations SHALL keep the SemanticEvent, Claims concerning that event, observations of the event, and Evidence supporting those Claims semantically distinct.

---

## 9. AI Generation and Assistance

AI participation SHALL be explicitly representable.

The model SHOULD distinguish between:

- human creation without material AI participation;
- AI-assisted human creation;
- human-directed AI generation;
- autonomous agent generation;
- AI transformation of existing material;
- human modification of AI-generated material;
- combinations of these processes.

Where available, an AI-related Action MAY reference:

```text
model provider
model identifier
model version
execution environment
input artifacts
instructions
prompts
parameters
tool calls
output artifacts
execution evidence
```

The standard SHALL NOT require disclosure of information that is unavailable, confidential or inappropriate to disclose.

Absence of such information SHALL be distinguishable from an assertion that the information does not exist.

---

## 10. Claim

A Claim is an assertion concerning an Actor, Artifact, Action or relationship.

A Claim may conceptually contain:

```text
Claim
    identifier
    subject
    predicate
    object
    issuer
    time
    evidence[]
    attestations[]
    authority[]
    status
```

Example:

```text
Subject:
    Actor Alice

Predicate:
    performed

Object:
    Action 9381

Evidence:
    device signature
    application event
    file creation record
    identity credential
```

Multiple Claims MAY concern the same event.

Claims MAY support, qualify or contradict other Claims.

A Claim MAY reference a SemanticEvent to assert that the described event occurred, that an Actor participated in it, or that an Artifact was produced, modified, selected, reviewed or otherwise affected by it. A SemanticEvent SHALL NOT itself be treated as such an assertion.

---

## 11. Evidence

Evidence is information capable of supporting or challenging a Claim.

Examples include:

- cryptographic hashes;
- digital signatures;
- trusted timestamps;
- device attestations;
- application event records;
- execution receipts;
- identity credentials;
- model execution records;
- external records;
- human attestations;
- provenance records generated by another conforming system.

Evidence SHALL be distinguishable from the Claim it supports.

This distinction is fundamental.

A system SHALL NOT be required to accept a Claim merely because it is syntactically conformant with this standard.

---

## 12. Attestation

An Attestation represents an Actor explicitly endorsing, confirming or witnessing a Claim.

For example:

```text
Claim:
    Alice wrote the methods section of the report.

Attestations:
    Alice confirms
    Co-author confirms
    Institution confirms
```

A relying party MAY assign different trust to different Attestations.

---

## 13. Authority and Delegation

AI agents make authority a first-class provenance concern.

The model SHALL support explicit delegation relationships.

For example:

```text
Human
  │
  │ authorizes
  ▼
Agent
  │
  │ invokes
  ▼
Model
  │
  │ produces
  ▼
Artifact
```

This SHALL NOT be represented as though the Human directly performed the Model's Action.

Instead, the provenance chain preserves both execution and authority.

An Authority record MAY express:

```text
principal
delegate
scope
constraints
validity
evidence
revocation status
```

The standard SHALL permit authorization mechanisms to be supplied by external systems.

---

## 14. Derivation

Relationships between Artifacts SHALL be explicitly representable.

For example:

```text
Artifact A
   │
   ├── transformed into → Artifact B
   │
   └── incorporated into → Artifact C
```

Possible derivation relationships include:

```text
derived-from
generated-from
edited-from
transformed-from
composed-from
trained-from
extracted-from
incorporates
references
```

The vocabulary SHALL provide an extensibility mechanism for domain-specific relationships.

---

## 15. Correction, Supersession and Dispute

Provenance history SHOULD be append-preserving.

A previously issued Claim SHOULD NOT need to be destroyed merely because it later proves incorrect.

The model SHALL support:

```text
Claim A
   │
   ├── superseded by → Claim B
   │
   ├── revoked by ───→ Claim C
   │
   └── disputed by ──→ Claim D
```

This permits independent systems to retain an auditable history of changing assertions.

---

## 16. Privacy and Selective Disclosure

The standard SHALL support provenance without requiring unnecessary disclosure of personal information.

For example, it SHOULD be possible to establish:

> Artifact A and Artifact B were created by the same verified human Actor.

without necessarily revealing:

> The Actor's legal name is Alice Example.

Identity credentials SHOULD normally remain separable from provenance records.

Implementations SHOULD follow data-minimization principles and SHOULD avoid embedding government identifiers or other unnecessary personal information into persistent public provenance.

---

## 17. Integrity

The standard SHALL support mechanisms allowing a relying party to determine whether provenance information has been altered.

Possible mechanisms include:

- cryptographic digests;
- digital signatures;
- Merkle structures;
- trusted timestamps;
- hardware-backed attestations;
- transparency logs.

No single cryptographic technology SHALL be required by the abstract information model unless necessary to satisfy defined conformance requirements.

Cryptographic mechanisms SHOULD be algorithm-agile.

A VCP conformance profile that uses public-key signatures SHALL require a post-quantum signature. During the transition proposed by this draft, it SHALL combine that signature with a traditional signature using a profile-defined AND-combiner; it SHALL NOT accept a traditional signature alone as conformant authenticity evidence. This preserves the draft VCP security proposal without making its adoption a prerequisite for participating in the CG or for the initial web integration study.

---

## 18. Serialization and Bindings

The core standard SHALL NOT depend upon a single serialization format.

Bindings MAY subsequently define representations using:

```text
JSON
CBOR
JSON-LD
Protocol Buffers
other future representations
```

All conforming bindings SHALL preserve the normative semantics of the abstract model.

A JSON representation, for example, might express an Action as:

```json
{
  "id": "urn:example:action:1234",
  "type": "Generate",
  "actor": "urn:example:actor:5678",
  "inputs": ["urn:example:artifact:1111"],
  "outputs": ["urn:example:artifact:2222"]
}
```

This representation is illustrative. JSON itself is not the information model.

---

## 19. Interoperability with Existing Work

The starting point is a mapping exercise with existing communities. New terminology or formats should be justified by an identified gap and independently reviewed use cases.

The COA CG identifies C2PA, CAWG, Originator Profile and Verifiable Credentials among the communities and technologies to bring together. The following are **proposed mapping questions**, not claims of completed integration or endorsements by those communities.

| Existing work | Question for the proposed study |
| --- | --- |
| C2PA and CAWG | How can existing content and identity assertions be referenced without reissuing or changing their meaning? Which contribution roles and transformations can already be expressed? |
| Originator Profile | How can publisher or originator attributes be associated with the relevant Actor while keeping the publisher distinct from individual contributors? |
| [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model-2.0/) | How can issuer claims about Actor attributes be bound to the relevant provenance context, with optional disclosure and separate credential-status checks? |
| [W3C PROV-O](https://www.w3.org/TR/prov-o/) | How do Actor, Artifact, Action and derivation map to existing agent, entity, activity and provenance relations? Which additional assertion, evidence or authority semantics need a profile? |
| URLs, HTTP, HTML and existing web integrity mechanisms | How should a user agent find evidence and determine which representation or version it covers? What can be reused before considering a new association mechanism? |
| Software supply-chain and creative-domain formats | How can evidence from authoring, repository, build and publication systems remain independently interpretable across the lifecycle? |

The proposed mappings must preserve issuer, subject, content binding, evidence scope, availability and verification results. Wrapping an existing assertion in VCP must not upgrade its assurance or turn an unevaluated claim into a verified fact.

VCP Semantic Event domain profiles MAY describe activities in software, research, publishing, design, film and music while preserving the core assertion boundary. A domain profile or vocabulary does not require every user agent to understand every domain term.

---

## 20. Illustrative Multi-Contributor Example

Consider a technical report produced through the following process:

```text
Alice
  │
  │ writes analysis
  ▼
Analysis Draft
  │
  │
  ├──────────────┐
  │              │
  │         Bob writes prompt
  │              │
  │              ▼
  │          AI Model
  │              │
  │         generates figures
  │              │
  └──────┐       ▼
         ▼    Figure Set
          \      /
           \    /
            ▼  ▼
            Carol
             │
             │ edits and assembles
             ▼
          Final Report
```

The provenance record can establish separately that:

- Alice wrote the analysis draft;
- Bob authored or supplied an instruction;
- an identified AI model generated the figure set;
- the figure set was derived through a particular model invocation;
- Carol edited and assembled the resulting artifacts;
- the final report incorporates both human-authored and AI-generated material.

Each assertion MAY have independent Evidence.

No single assertion such as "Bob created the report" is required to collapse these relationships.

The workflow MAY additionally represent these contribution semantics using the SemanticEvent profile, for example `eventType=Create` with `activity.type=https://example.org/vcp/activity/publishing/author/draft`, while content-addressed artifact states identify the actual documents, figures and project state. The model leaves storage and version management to independently defined bindings.

---

## 21. Illustrative Autonomous-Agent Example

A human asks an AI agent to create a software component:

```text
Human
  │
  │ delegates task
  ▼
Agent A
  │
  ├── invokes Model B
  │       │
  │       └── generates Code A
  │
  ├── invokes Tool C
  │       │
  │       └── tests Code A
  │
  └── modifies
          │
          ▼
        Code B
          │
          ▼
Human reviews and approves
```

The resulting provenance can distinguish:

- human authority;
- agent decisions;
- model generation;
- tool execution;
- derived artifacts;
- human review;
- final approval.

This distinction is increasingly necessary as autonomous systems perform material actions on behalf of humans and organizations.

---

## 22. Conformance

Conformance SHALL be defined in terms of behaviour and semantics rather than use of a particular software implementation.

The initial conformance classes are:

**Producer**

A system capable of producing conforming provenance information.

**Consumer**

A system capable of interpreting conforming provenance information.

**Verifier**

A system capable of evaluating integrity and Evidence associated with Claims.

**Identity Verifier**

A system capable of evaluating supported forms of Identity Evidence.

**Authority Verifier**

A system capable of evaluating delegation or authority Evidence.

An implementation MAY conform to one or more classes.

A conformance claim SHALL identify:

- the version of this standard;
- each claimed conformance class;
- each binding and domain profile used;
- the version of the applicable conformance suite;
- the result of each mandatory test;
- any optional capabilities that were not evaluated.

Syntactic validity alone SHALL NOT establish conformance.

A conforming **Producer** SHALL:

- assign stable identifiers to every emitted core entity;
- keep Claims, Evidence, Attestations, Identity Evidence and Authority records semantically distinct;
- emit referentially closed bundles, except where a binding explicitly represents an external Evidence location;
- distinguish unavailable, withheld, not-recorded and not-applicable information where absence would otherwise be ambiguous;
- preserve correction history rather than mutating or deleting previously issued Claims;
- provide an integrity mechanism defined by the selected binding or profile.

A conforming **Consumer** SHALL:

- interpret entity types and relationships according to the abstract model;
- preserve unknown extension terms when it re-emits information it does not understand;
- distinguish syntactic validity from acceptance or truth;
- avoid treating identity assurance as authority or treating delegation as proof that an Action occurred.

A conforming **Verifier** SHALL independently evaluate, where present:

- binding validity;
- identifier uniqueness and reference integrity;
- integrity digests, signatures or equivalent protections;
- Claim-to-Evidence and Claim-to-Attestation references;
- derivation and correction relationship constraints;
- explicit AI participation and disclosure-state semantics;
- the status of Evidence that cannot be evaluated.

A conforming **Identity Verifier** SHALL report identity assurance separately from provenance and Authority. It SHALL identify the assurance framework, assurance level, issuer, validity status and evaluated proof mechanism, or report why these could not be established.

A conforming **Authority Verifier** SHALL evaluate the principal, delegate, scope, constraints, validity interval, delegation chain and revocation status. It SHALL reject scope or validity escalation and SHALL NOT treat a valid Authority record as Evidence that the authorized Action occurred.

---

## 23. Reference Architecture

The proposed separation can be summarized as:

```text
                 IDENTITY
                Who are you?
                     │
                     ▼
                 AUTHORITY
              What may you do?
                     │
                     ▼
                PROVENANCE
               What happened?
                     │
                     ▼
                  EVIDENCE
            Why should I believe it?
```

These layers SHOULD remain independently extensible.

---

## 24. Relationship to Implementations

The standard SHALL remain independent of individual commercial implementations.

An implementation could, for example, satisfy the standard by automatically capturing activity from authoring applications, repositories, operating systems, AI agents, AI models and external services:

```text
Applications ──┐
Repositories ──┤
AI Models ─────┤
Agents ────────┼──► Provenance Capture ──► Evidence Graph
Devices ───────┤
Identity ──────┤
Authority ─────┘
```

Other organizations SHALL be able to build independent conforming implementations.

Interoperability between independent implementations should be a primary measure of success for the standard.

---

## 25. Proposed COA CG Work Items and Next Steps

The proposed first step is to contribute use cases and requirements to the CG's reports, then assess whether a specification gap remains. This follows the group's [published mission and deliverables](https://www.w3.org/community/coa-cg/); it does not presume group agreement to standardize VCP.

### 25.1 Landscape study

Compare the article workflow in the meeting brief against existing content, identity and provenance mechanisms. Document reusable elements, differences in terminology and any missing mappings. In particular, distinguish creator, publisher, hosting origin, credential issuer, delegate and observer.

### 25.2 Use cases and requirements

Develop examples for mixed human–AI publishing, pseudonymous authorship, editorial correction and republication of transformed content. Record what the reader needs to establish, what information may remain private, what trust inputs are required and which questions cannot be answered from the available evidence.

### 25.3 Web integration study

Evaluate evidence discovery and retrieval, exact-version binding, redirects and content negotiation, cross-origin access, metadata loss, availability, safe presentation and privacy. Section 31 lists candidate questions for browser and publisher review. No new HTTP field, HTML relation, browser API or trust indicator is specified by this document.

### 25.4 Bounded interoperability experiment

Use one article, one illustration and one revised publication state. Have a producer export evidence and a separately implemented consumer inspect it using an existing association mechanism selected for the experiment.

The experiment should demonstrate:

- distinct author, model, editor and publisher Actors;
- separate contribution, identity, delegation and content-integrity results;
- exact content-state binding, including refusal when content is substituted;
- pseudonymous publication and absent identity evidence;
- private prompts remaining undisclosed;
- explicit handling of revoked, disputed, missing or unsupported evidence;
- a reader-facing explanation that does not equate authenticity with truthfulness;
- no automatic disclosure of the reader's browsing activity to identity or evidence providers.

Success means independent interpretation of the bounded workflow and a documented account of the remaining gaps. It does not mean universal trust in a publisher, a model or a content claim.

### 25.5 Subsequent incubation

If the reports and experiment identify a concrete gap, participants could propose a focused CG specification, with suitable contribution terms and review. Any later Working Group proposal or standards-track work would require its own scope, consensus and process. Core semantic mappings, web associations and domain vocabularies should remain separately reviewable.

**Requested outcome from this meeting:** identify interested reviewers and agree whether the mixed human–AI article example belongs in the initial use-case and web integration work.

---

## 26. Intended Outcome

The objective is to establish an open foundation by which any participant in a digital ecosystem can make and evaluate statements of the form:

> **Who or what did what, to which digital artifact, under whose authority, and what evidence supports that assertion?**

The same fundamental model should apply whether the artifact is:

- software;
- a document;
- a dataset;
- a scientific result;
- a design;
- a photograph;
- a film;
- a song;
- or an artifact produced by future forms of artificial intelligence.

The standard should therefore not attempt merely to describe AI-generated content.

It should provide a general-purpose **trust and provenance layer for digital creation in an age where humans and machines create together.**

---

## 27. Core Semantic Invariants

The following invariants are candidate normative requirements within the proposed VCP model.

### 27.1 Identifiers and references

Every Actor, Artifact, Action, Claim, Evidence, Attestation, Authority, Identity Claim, Identity Evidence and Relationship SHALL have an identifier that is stable within its declared provenance context.

Bindings SHALL distinguish entity identifiers from storage locations. Moving an Artifact or Evidence object between storage locations SHALL NOT, by itself, change its identity.

A bounded provenance bundle SHALL be referentially closed. A reference to information outside the bundle SHALL be represented as an external Evidence reference with an explicit availability state, not as a dangling entity reference.

Identifiers SHALL be unique across all core entity types within one provenance context. Reusing an identifier for objects of different types is invalid.

### 27.2 Assertion boundary

The presence of an Actor, Artifact, Action or Relationship description in a bundle does not establish that the described entity existed or that the described occurrence took place.

A Claim is the only core entity that asserts a proposition. Evidence may support or challenge a Claim, and an Attestation may endorse, confirm, witness or challenge a Claim, but neither becomes the Claim itself.

A signature protects integrity and may identify a signer. It SHALL NOT, without an explicit Claim and applicable Evidence, be interpreted as proof that the signer performed an Action, created an Artifact, possessed a legal identity or had Authority.

A SemanticEvent is a semantic description and follows the same assertion boundary. Its presence does not establish that the described event occurred; Claims assert propositions concerning it.

### 27.3 Reference direction and back-references

When a binding supplies bidirectional references, both directions SHALL agree. For example:

- if a Claim references Evidence, that Evidence SHALL identify the Claim as supported or challenged;
- if a Claim references an Attestation, that Attestation SHALL identify the Claim;
- if a Claim references a Relationship, that Relationship SHALL identify the Claim that asserts it.

This requirement prevents unrelated but individually valid objects from being substituted into a provenance bundle.

### 27.4 Time

An unqualified timestamp is an assertion by its issuer. Trusted or independently established time SHALL be represented as Evidence and SHALL identify the mechanism by which it was established.

An Action end time SHALL NOT precede its start or occurrence time. An Action relying on Authority SHALL fall within the Authority validity interval.

### 27.5 Information availability

Bindings SHALL provide distinct states for information that is:

```text
disclosed
committed but not disclosed
withheld
unavailable to the producer
not recorded
not applicable
```

Omitting a field SHALL NOT be interpreted as an assertion that the corresponding information does not exist.

Where committed information is not disclosed, the commitment mechanism SHALL be identified.

### 27.6 Vocabulary extension

Core vocabulary terms MAY be extended using globally unique identifiers. An extension term SHALL NOT redefine the semantics of an existing core term.

A Consumer that does not understand an extension term SHALL preserve it when re-emitting the record and SHALL report the corresponding semantic evaluation as unsupported rather than silently mapping it to a known term.

SemanticEvent domain activities and roles SHALL follow this rule. A Consumer MAY reason over the base SemanticEvent `eventType` while preserving an unknown more-specific activity vocabulary term.

---

## 28. Authority Semantics

An Authority record SHALL identify, at minimum:

```text
principal Actor
delegate Actor
permitted Action types or equivalent scope
validity start
status
supporting Evidence
```

An Authority record MAY additionally identify Artifact constraints, other constraints, a validity end, permission to delegate, a parent Authority and a revocation Claim.

When Authority is delegated through more than one Actor:

- each child Authority SHALL reference its parent Authority;
- the child principal SHALL be the parent delegate;
- the parent SHALL permit further delegation;
- the child scope SHALL be equal to or narrower than the parent scope;
- the child validity interval SHALL be contained within the parent validity interval;
- the delegation graph SHALL be acyclic and an active child SHALL NOT rely on an inactive parent.

A binding SHALL distinguish unrestricted Artifact scope from an explicitly empty scope. A child Authority SHALL preserve or narrow any explicit parent Artifact restrictions. Inherited constraints SHALL NOT be silently removed or changed. Preserving a constraint is distinct from evaluating it; domain-specific policy conditions require an Authority Verifier that understands those conditions.

Authority status SHALL distinguish at least asserted, active, revoked and expired.

A revoked Authority SHALL reference the Claim establishing or asserting revocation. Revocation SHALL NOT erase the historical Authority record or Actions previously associated with it.

---

## 29. Relationship Semantics

Artifact derivation relationships include:

```text
derived-from
generated-from
edited-from
transformed-from
composed-from
trained-from
extracted-from
incorporates
references
```

Both endpoints of an Artifact derivation relationship SHALL be Artifacts.

Claim relationships include:

```text
supports
qualifies
contradicts
supersedes
revokes
disputes
```

Both endpoints of a Claim relationship SHALL be Claims. Correction relationships SHALL be append-preserving and SHALL NOT require removal or mutation of the target Claim.

Every Relationship included in a bounded provenance bundle SHALL reference at least one Claim asserting that relationship.

---

## 30. Specification Work and Current Status

This public repository contains the complete discussion proposal and its [Semantic Event companion](SEMANTIC_EVENT.md). All proposal definitions and examples needed to read these documents are included here; external standards links provide public background and candidate interoperability targets.

This repository does not yet publish an agreed interchange schema, canonicalization specification, trust registry or conformance suite. The JSON snippets illustrate the proposed model; they are not a finalized wire format.

No schema file in another repository is required or incorporated by reference. Future bindings would need to publish their own complete definitions and supporting material.

Before claiming interoperable conformance, a binding would need to define:

- exact entity structures and vocabulary-extension rules;
- identifier, reference and availability-state constraints;
- canonical bytes, digest coverage and signature verification rules;
- trust inputs, status checking and algorithm migration;
- privacy and disclosure behavior;
- positive and adversarial fixtures with deterministic expected results;
- independently implemented producers and consumers tested against those fixtures.

Useful adversarial cases include Actor substitution, content substitution, dangling references, scope escalation, Claim/Evidence conflation, ambiguous absence of AI information, duplicate identifiers, invalid time ranges, integrity tampering and signature downgrade.

These are proposed work items. Publication of this proposal does not establish independent interoperability or adoption by the COA CG. Product-specific implementations, capture systems and deployment details are outside this repository's scope.

---

## 31. Web Integration Questions for COA CG Review

These are proposed research and design questions, not definitions of new web-platform behavior.

### 31.1 Evidence association and discovery

- Which existing HTTP, HTML or embedded-metadata mechanisms can associate a web resource with provenance evidence?
- Does an association describe the document, one embedded image, a downloadable asset or a set of resources?
- What happens when different association mechanisms disagree, or an intermediary strips metadata?
- Can evidence be inspected without running publisher-controlled code or contacting an unrelated identity provider?

### 31.2 Exact representations and lifecycle

- How does evidence bind to the exact bytes or explicitly defined content state being verified, rather than only to a mutable URL?
- How should verification handle redirects, content negotiation, compression, localization, dynamic assembly, caching and republication?
- How can a reader distinguish an unchanged copy from a transformed derivative, and an unavailable original from an invalid claim?
- Which integrity checks require access to the original material, and what remains verifiable after that material is removed?

### 31.3 Originator attributes and authority

- Which Actor is the subject of a third-party attestation, and what binds that subject to a particular contribution?
- How should issuer trust, credential validity, holder binding, key rotation and revocation be evaluated separately?
- How can a user agent distinguish an organization publishing content from a person writing it or an agent acting under delegation?
- What is the minimum information needed to evaluate a claim without disclosing a legal name or a global identifier?

### 31.4 User-agent presentation and accessibility

- How should interfaces distinguish integrity checked, signer trusted, identity attribute checked, authority checked and claim unsupported?
- How should missing, unsupported, expired, disputed and invalid evidence be explained without collapsing them into one status?
- How can scope be made clear when only one image, one contribution or one publication state has been checked?
- How should keyboard access, assistive technologies, localization and understandable wording be included in any presentation experiment?

### 31.5 Privacy and security review

- Can evidence retrieval expose browsing history, enable cross-site correlation or turn credential-status checks into tracking?
- When should retrieval be local, cached, mediated or explicitly initiated by the reader?
- How should consumers limit untrusted evidence sizes, recursive dependencies, network requests and hostile metadata?
- How are consent, selective disclosure and private evidence handled without making real-name publication a condition of participation?

These questions should be reviewed with browser implementers, publishers, the existing provenance and identity communities, and web security, privacy and accessibility reviewers before selecting an integration design.
