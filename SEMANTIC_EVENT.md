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

## 3. State transitions and optional Git binding

A SemanticEvent SHOULD identify a before-state and after-state where the workflow exposes them, using content digests or another binding-defined state identifier. A Git binding is one possible representation: Git can identify the exact project state while the event describes the contribution meaning. The following Git example is illustrative and does not require other domains to use Git.

```text
Git commit C100
    artifact state before
         |
         | SemanticEvent
         | actor = Alice
         | eventType = Modify
         | activity = LineEdit
         v
Git commit C101
    artifact state after
```

The SemanticEvent states what the transition is claimed to mean. Git identifies the exact bytes or content-addressed object state before and after.

A SemanticEvent SHALL NOT replace the Git binding, Artifact digest, Claim, Evidence, or Attestation.

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
git-commit
git-tree
artifact-digest
external-state
```

A Git-bound profile SHOULD use Git commit or tree identifiers where possible.

The after-state MAY be the commit containing the SemanticEvent record when the applicable Git profile defines that binding. A profile SHALL specify how circular self-reference is avoided.

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

Similarly, a Git diff can establish the exact project transition but does not by itself establish who conceived or authored the change.

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
  "schema": "https://example.org/vcp/semantic-event/v0.1",
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
      "actorRef": "urn:person:alice",
      "actorType": "human",
      "role": "https://example.org/vcp/role/research/experimenter",
      "participation": "primary"
    },
    {
      "actorRef": "urn:person:carol",
      "actorType": "human",
      "role": "https://example.org/vcp/role/research/instrument-operator",
      "participation": "operator"
    }
  ],
  "artifacts": [
    {
      "artifactRef": "git:path:Data/Runs/Run-07.csv",
      "relationship": "output",
      "path": "Data/Runs/Run-07.csv",
      "mediaType": "text/csv",
      "selector": {"kind": "object", "value": "Sample B"}
    },
    {
      "artifactRef": "git:path:Experiment.json",
      "relationship": "modified",
      "path": "Experiment.json"
    }
  ],
  "before": {"type": "git-commit", "ref": "8f10..."},
  "after": {"type": "git-commit", "ref": "91ab..."},
  "claimRefs": ["claim-measurement-run-7"],
  "evidenceRefs": ["instrument-observation-884"],
  "createdAt": "2026-09-12T15:18:00Z"
}
```

This describes the contribution semantics while the referenced Git state contains or content-addresses the actual dataset and experiment project state.

## 15. Publishing profile example

```json
{
  "schema": "https://example.org/vcp/semantic-event/v0.1",
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
  "actors": [{
    "actorRef": "urn:person:editor-1",
    "actorType": "human",
    "role": "https://example.org/vcp/role/publishing/line-editor",
    "participation": "primary"
  }],
  "artifacts": [{
    "artifactRef": "git:path:manuscript/book.docx",
    "relationship": "modified",
    "path": "manuscript/book.docx",
    "selector": {"kind": "page-range", "value": {"start": 87, "end": 104}}
  }],
  "before": {"type": "git-commit", "ref": "124a..."},
  "after": {"type": "git-commit", "ref": "57bc..."},
  "createdAt": "2026-09-12T14:00:00Z"
}
```

## 16. Human-AI software profile example

```json
{
  "schema": "https://example.org/vcp/semantic-event/v0.1",
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
  "artifacts": [{
    "artifactRef": "git:path:Sources/Auth/",
    "relationship": "modified",
    "path": "Sources/Auth/",
    "selector": {"kind": "directory", "value": "Sources/Auth/"}
  }],
  "before": {"type": "git-commit", "ref": "abcdef..."},
  "after": {"type": "git-commit", "ref": "123456..."},
  "authorityRefs": ["authority-alice-agent-17"],
  "claimRefs": ["claim-agent-auth-implementation"],
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

A Git-bound Semantic Event Verifier SHALL additionally:

- validate referenced before/after Git states where available;
- validate that referenced artifact paths or objects exist at the applicable state;
- preserve the distinction between the exact Git transition and the semantic Claim about what that transition means;
- report unsupported selectors or domain semantics separately from structural failure.

## 18. Proposed JSON Binding

The JSON examples illustrate a possible binding of the Semantic Event model. This repository does not yet define a normative JSON Schema or a conformance suite. The `schema` URLs in the examples are placeholders identifying the proposed shape; they are not retrieval endpoints.

A future binding would need to define field types, identifier and reference rules, state and selector validation, extension handling, canonicalization and versioning, together with positive and adversarial fixtures. It must preserve the assertion boundary and the distinction between semantic description, Claim, Evidence and Attestation.

Domain-specific bindings, including a possible Git binding, should be reviewed separately from the portable semantic model.
