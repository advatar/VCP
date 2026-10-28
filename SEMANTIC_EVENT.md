# VCP Semantic Event v0.1

**Status:** proposed VCP companion draft v0.1 — for discussion; not a W3C specification or adopted CG report

This document defines the VCP Semantic Event profile for describing the meaning of a contribution or state transition without making VCP dependent on software-development terminology or any one creative domain.

It is a companion to the [VCP discussion proposal](W3C.md) and is governed by the same abstract-model principles, including the separation of Claims from facts, Evidence from Claims, Identity from Authority, and semantics from serialization.

SHALL, SHOULD and MAY express candidate requirements within this proposed profile. The examples are illustrative and do not claim conformance to an agreed interchange schema. All `https://example.org/vcp/` identifiers are placeholders, not assigned standard namespaces or network dependencies.

## 1. Purpose

A VCP `Action` describes an occurrence involving Actors and Artifacts; a separate Claim asserts that it happened. A `SemanticEvent` adds a portable description of **what kind of contribution or transformation that occurrence represented**.

The same structural provenance pattern occurs across many domains:

```text
Actor -> Action -> Artifact
```

but the semantic meaning can differ materially:

```text
editor     -> revised   -> manuscript
engineer   -> designed  -> CAD model
AI agent   -> generated -> source code
scientist  -> measured  -> dataset
musician   -> performed -> audio take
reviewer   -> approved  -> specification
```

VCP SHALL therefore support a small stable core event vocabulary together with extensible domain activity vocabularies.

## 2. Semantic boundary

A `SemanticEvent` is a description of the asserted meaning of a state transition. It is not by itself proof that the event occurred.

The following concepts SHALL remain distinct:

```text
SemanticEvent
    describes the meaning of a transition

Claim
    asserts that the SemanticEvent occurred or that an Actor participated

Observation
    records what a system measured or observed

Evidence
    supports or challenges a Claim

Attestation
    records an Actor's signed or otherwise attributable statement concerning a Claim
```

A SemanticEvent SHALL NOT be interpreted as true merely because it is syntactically valid.

## 3. State transitions

A SemanticEvent SHOULD identify a before-state and after-state where the workflow exposes them. A state reference MAY use an Artifact digest, an immutable version identifier or another binding-defined identifier. Its meaning SHALL be explicit; a mutable location alone does not identify the exact content state.

```text
Artifact state A
    content before
         |
         | SemanticEvent
         | actor = Alice
         | eventType = Modify
         | activity = LineEdit
         v
Artifact state B
    content after
```

The SemanticEvent describes what the transition is claimed to mean. Integrity Evidence binds the description to the identified content states. This separation applies to documents, datasets, recordings, designs and software without requiring any particular storage service, version-control system or ledger.

A SemanticEvent SHALL NOT replace an Artifact digest, Claim, Evidence or Attestation.

## 4. Core event vocabulary

`eventType` SHALL use a deliberately small, stable vocabulary. Version 0.1 defines:

```text
Create
Modify
Transform
Combine
Separate
Select
Arrange
Perform
Record
Generate
Measure
Observe
Annotate
Review
Approve
Reject
Correct
Translate
Publish
Delete
Restore
```

A domain profile MAY define a more specific `activity.type`, but SHALL NOT redefine the meaning of a core `eventType`.

For example:

```text
eventType = Measure
activity.type = https://example.org/vcp/activity/research/measurement/spectroscopy
```

A Consumer that does not understand the specific activity can still reason about the broader `Measure` event.

## 5. Activity vocabulary

An Activity refines the semantic meaning of the core event.

An Activity SHOULD identify:

```text
type
label
broaderActivity
profile
```

Activity identifiers SHALL be globally unique. The following illustrative pattern is used in this proposal; a published profile would need to define its vocabulary namespace:

```text
https://example.org/vcp/activity/<domain>/<activity-path>
```

Examples:

```text
https://example.org/vcp/activity/publishing/edit/line
https://example.org/vcp/activity/publishing/author/draft
https://example.org/vcp/activity/research/measurement/spectroscopy
https://example.org/vcp/activity/research/analysis
https://example.org/vcp/activity/software/implement
https://example.org/vcp/activity/software/refactor
```

A domain MAY define a hierarchy with `broaderActivity`, for example:

```text
LineEdit
    broaderActivity -> Edit
    broaderActivity -> EditorialContribution
```

## 6. Actor participation

A SemanticEvent SHALL identify one or more participating Actors.

Each participation identifies:

```text
actorRef
actorType
role
participation
authorityRef (where applicable)
```

`actorType` MAY be:

```text
human
agent
model
device
software
organization
```

`participation` MAY be:

```text
primary
contributor
assistant
operator
reviewer
approver
witness
observer
```

Roles SHALL use extensible identifiers such as:

```text
https://example.org/vcp/role/publishing/line-editor
https://example.org/vcp/role/research/experimenter
https://example.org/vcp/role/research/instrument-operator
https://example.org/vcp/role/software/implementer
https://example.org/vcp/role/software/principal
```

Different roles SHALL NOT be collapsed merely because the Actors participated in the same state transition. For example, an experimenter and an instrument operator are distinct participants even when one measurement event involves both.

## 7. Artifact participation

A SemanticEvent SHALL identify one or more Artifacts and their relationship to the event.

The base relationships are:

```text
input
output
modified
referenced
selected
rejected
deleted
```

An Artifact participation MAY additionally include:

```text
path
mediaType
selector
```

Selectors permit a contribution to address a subpart of an Artifact without inventing a new Artifact identity solely for convenience.

Version 0.1 selector kinds include:

```text
file
directory
text-range
time-range
track
clip
region
page-range
frame-range
object
symbol
other
```

Examples include pages in a manuscript, a source-code symbol, a region of a design, a time range in an audio track, or a frame range in video.

## 8. State references

`before` and `after` state references MAY identify:

```text
artifact-digest
versioned-state
external-state
```

A binding SHALL define how each reference identifies its exact state and how that binding is verified. A consumer SHALL distinguish an identifier from proof of the referenced state's integrity or availability.

A profile SHALL specify how circular self-reference is avoided if an event record is stored with the state it describes. The event model does not prescribe where either the event or the content is stored.

## 9. Event relationships

Semantic Events MAY relate to earlier events.

Version 0.1 defines:

```text
derived-from
supersedes
corrects
qualifies
disputes
combines
splits
continues
implements
reviews
```

A correction SHALL be append-preserving. It SHALL NOT require mutation or deletion of the original SemanticEvent, Claim, Evidence, or Attestation.

## 10. Claims and semantic events

A SemanticEvent is not a Claim.

A Claim can assert propositions such as:

```text
SemanticEvent E occurred.
Actor A was the primary contributor in E.
Artifact X was produced by E.
Actor B witnessed E.
```

The SemanticEvent SHOULD be referenced by Claims rather than duplicating its entire structure inside every Claim.

The same SemanticEvent MAY have confirming, qualifying, correcting, or disputing Claims and Attestations.

## 11. Observations

A machine observation SHALL be limited to facts the observing system can support.

For example, an authoring, capture or instrument application may credibly observe:

```text
an object appeared at time T;
region X changed;
input device D was active;
project state moved from digest A to digest B.
```

It SHALL NOT infer a human identity or creative authorship merely from those observations unless independent Identity Evidence supports that proposition.

Similarly, comparing two verified content states can establish what changed without establishing who conceived or authored the change.

## 12. Subjective and legal conclusions

A SemanticEvent SHOULD encode observable, attributable, or attestable activities rather than subjective creative conclusions.

For example:

```text
Bob drafted section 4.2 of the specification
```

may be represented as a contribution event and associated Claim.

Statements such as:

```text
Bob contributed 70% of the work.
Alice is legally the author.
This edit created the emotional essence of the work.
```

are not base SemanticEvent semantics. They are Claims, agreements, rights allocations, legal conclusions, or subjective assessments and SHOULD be represented separately.

## 13. Domain profiles

A domain profile MAY define:

- domain identifier;
- activity vocabulary;
- role vocabulary;
- permitted or required selector kinds;
- additional validation constraints;
- mappings to domain-specific standards;
- policy requirements for Claims, Evidence, Authority, or Attestations.

The base SemanticEvent remains interoperable when domain extensions are not understood.

Candidate profile identifiers used in the examples are:

```text
https://example.org/vcp/profile/research-data/v0.1
https://example.org/vcp/profile/publishing/v0.1
https://example.org/vcp/profile/human-ai-software/v0.1
```

These examples cover different domains so reviewers can assess whether the abstraction is sufficiently general.

## 14. Research data profile example

```json
{
  "id": "event-measurement-run-7",
  "entityType": "SemanticEvent",
  "eventType": "Measure",
  "domain": "https://example.org/vcp/domain/research",
  "activity": {
    "type": "https://example.org/vcp/activity/research/measurement/spectroscopy",
    "label": "Spectroscopy measurement run",
    "broaderActivity": "https://example.org/vcp/activity/research/measurement",
    "profile": "https://example.org/vcp/profile/research-data/v0.1"
  },
  "actors": [
    {
      "actorRef": "https://example.org/actor/alice",
      "actorType": "human",
      "role": "https://example.org/vcp/role/research/experimenter",
      "participation": "primary"
    },
    {
      "actorRef": "https://example.org/actor/carol",
      "actorType": "human",
      "role": "https://example.org/vcp/role/research/instrument-operator",
      "participation": "operator"
    }
  ],
  "artifacts": [
    {
      "artifactRef": "https://example.org/artifact/measurement-run-7",
      "relationship": "output",
      "mediaType": "text/csv",
      "selector": {
        "kind": "object",
        "value": "Sample B"
      }
    },
    {
      "artifactRef": "https://example.org/artifact/experiment-description",
      "relationship": "modified"
    }
  ],
  "before": {
    "type": "versioned-state",
    "ref": "https://example.org/state/experiment/v1"
  },
  "after": {
    "type": "versioned-state",
    "ref": "https://example.org/state/experiment/v2"
  },
  "claimRefs": [
    "claim-measurement-run-7"
  ],
  "evidenceRefs": [
    "instrument-observation-884"
  ],
  "createdAt": "2026-09-12T15:18:00Z"
}
```

This describes the contribution semantics while the state references identify the dataset and experiment state. A selected binding would define the integrity evidence for those states.

## 15. Publishing profile example

```json
{
  "id": "event-chapter4-line-edit",
  "entityType": "SemanticEvent",
  "eventType": "Modify",
  "domain": "https://example.org/vcp/domain/publishing",
  "activity": {
    "type": "https://example.org/vcp/activity/publishing/edit/line",
    "label": "Line edit",
    "broaderActivity": "https://example.org/vcp/activity/publishing/edit",
    "profile": "https://example.org/vcp/profile/publishing/v0.1"
  },
  "actors": [
    {
      "actorRef": "https://example.org/actor/editor-1",
      "actorType": "human",
      "role": "https://example.org/vcp/role/publishing/line-editor",
      "participation": "primary"
    }
  ],
  "artifacts": [
    {
      "artifactRef": "https://example.org/artifact/manuscript",
      "relationship": "modified",
      "selector": {
        "kind": "page-range",
        "value": {
          "start": 87,
          "end": 104
        }
      }
    }
  ],
  "before": {
    "type": "versioned-state",
    "ref": "https://example.org/state/manuscript/v1"
  },
  "after": {
    "type": "versioned-state",
    "ref": "https://example.org/state/manuscript/v2"
  },
  "createdAt": "2026-09-12T14:00:00Z"
}
```

## 16. Human-AI software profile example

```json
{
  "id": "event-auth-implementation",
  "entityType": "SemanticEvent",
  "eventType": "Create",
  "domain": "https://example.org/vcp/domain/software",
  "activity": {
    "type": "https://example.org/vcp/activity/software/implement",
    "label": "Authentication implementation",
    "profile": "https://example.org/vcp/profile/human-ai-software/v0.1"
  },
  "actors": [
    {
      "actorRef": "https://example.org/vcp/agent/example-session-1",
      "actorType": "agent",
      "role": "https://example.org/vcp/role/software/implementer",
      "participation": "primary",
      "authorityRef": "authority-alice-agent-17"
    },
    {
      "actorRef": "https://example.org/vcp/person/alice",
      "actorType": "human",
      "role": "https://example.org/vcp/role/software/principal",
      "participation": "approver"
    },
    {
      "actorRef": "https://example.org/vcp/model/example-model",
      "actorType": "model",
      "role": "https://example.org/vcp/role/ai/model",
      "participation": "assistant"
    }
  ],
  "artifacts": [
    {
      "artifactRef": "https://example.org/artifact/authentication-component",
      "relationship": "modified",
      "selector": {
        "kind": "directory",
        "value": "Sources/Auth/"
      }
    }
  ],
  "before": {
    "type": "versioned-state",
    "ref": "https://example.org/state/software-component/v1"
  },
  "after": {
    "type": "versioned-state",
    "ref": "https://example.org/state/software-component/v2"
  },
  "authorityRefs": [
    "authority-alice-agent-17"
  ],
  "claimRefs": [
    "claim-agent-auth-implementation"
  ],
  "createdAt": "2026-09-12T15:00:00Z"
}
```

This does not retroactively represent the human principal as having performed the Agent's or Model's Action.

## 17. Conformance requirements

A conforming Semantic Event Producer SHALL:

- emit a valid core `eventType`;
- use globally unique identifiers for domain activities and roles;
- identify at least one Actor and one Artifact participation;
- keep SemanticEvent, Claim, Observation, Evidence and Attestation semantics distinct;
- avoid representing subjective rights allocations or legal conclusions as base event semantics;
- preserve extension terms it does not own;
- use append-preserving correction rather than mutating previously committed history.

A conforming Consumer SHALL:

- understand the base `eventType` independently from the domain activity;
- preserve unknown domain activities and roles when re-emitting records;
- report unsupported domain semantics rather than silently remapping them;
- avoid treating a SemanticEvent as proof that an event occurred.

A Semantic Event Verifier evaluating state transitions SHALL additionally:

- evaluate the binding and integrity Evidence for referenced before/after states where available;
- validate that referenced Artifacts and selected subparts belong to the applicable state;
- preserve the distinction between a verified content transition and the semantic Claim about what that transition means;
- report unsupported selectors or domain semantics separately from structural failure.

## 18. Proposed JSON Binding

The JSON examples illustrate a possible representation of the Semantic Event model. They are event fragments, not complete provenance bundles or independently verifiable evidence packages. Referenced Actors, Artifacts, Claims and Evidence would need their own descriptions in a complete bundle.

This repository does not yet define a normative JSON Schema or conformance suite. The examples therefore omit schema-location fields rather than referring to unavailable files or retrieval endpoints. Identifiers under `https://example.org/` are illustrative values; understanding the examples requires no network access.

A future binding would need to define field types, identifier and reference rules, state and selector validation, extension handling, canonicalization and versioning, together with positive and adversarial fixtures. It must preserve the assertion boundary and the distinction between semantic description, Claim, Evidence and Attestation.

Any domain or storage binding should be reviewed separately from the portable semantic model. No implementation or infrastructure is required by this companion profile.
