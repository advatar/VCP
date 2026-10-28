# VCP Interoperability Survey

**Review date:** 28 October 2026. **Status:** technical survey and proposed mapping study supporting [Section 19 of the VCP proposal](W3C.md#19-interoperability-with-existing-work).

Existing standards cover much of the information VCP wants to describe. Content manifests, contributor roles, identity credentials, delegation relations, provenance graphs and web discovery mechanisms already exist. The strongest initial role for VCP is to specify how these can be interpreted together without losing their original scope or increasing their apparent assurance.

The proposed work is therefore an interoperability profile and a set of independently tested mappings. A new vocabulary, assertion type or transport mechanism should follow a demonstrated failure of existing mechanisms to satisfy a concrete use case. This survey distinguishes published capabilities from proposed VCP mappings and candidate gaps; the mappings are design proposals, not completed integrations.

All VCP definitions needed to read the survey are in the [main proposal](W3C.md) and [Semantic Event companion](SEMANTIC_EVENT.md). External references identify public specifications and their maintaining communities. No particular implementation, private repository, identity provider or hosting service is required.

## Principal findings

1. **[C2PA](https://spec.c2pa.org/specifications/specifications/2.5/specs/C2PA_Specification.html) and [CAWG](https://cawg.io/identity/1.3/) already address more than file signing.** Their work includes content history, transformations, contributor identity and roles. VCP should preserve native assertions and their validation before projecting selected meanings.
2. **[Originator Profile](https://docs.originator-profile.org/en/opb/) already connects organizational identity and content to the web.** Its organization, attribute, content and website concepts should remain distinguishable when mapped to VCP Actors and Artifacts.
3. **[Verifiable Credentials](https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/) provide an issuer claim model, not a universal identity assurance level.** Subject binding, presentation, status, disclosure and the verifier's trust policy remain separate questions.
4. **[PROV](https://www.w3.org/TR/2013/REC-prov-dm-20130430/) is a substantial semantic foundation.** It already describes agents, entities, activities, roles, derivation and delegation. VCP should first test a PROV profile that adds explicit assertion and evidence treatment.
5. **[Provenance discovery](https://www.w3.org/TR/2013/NOTE-prov-aq-20130430/) is not an empty standards space.** Existing HTTP linking, PROV-AQ, content-format mechanisms and Originator Profile associations are candidates for reuse. Finding evidence and verifying what it covers are different operations.
6. **[Software provenance](https://slsa.dev/spec/v1.2/source-requirements) already reaches into source workflows.** SLSA 1.2 includes a Source track. The remaining question is how to preserve source-system evidence and connect it to contributions across domains.
7. **Domain vocabularies should remain owned by their communities.** Research roles, photo metadata and recording-session credits provide useful semantics without establishing identity, authorization, legal authorship or event occurrence by themselves.

## Editions and document status

These are the editions examined, not a claim that every deployed product implements them. Versioned links are used where available. Living documentation and draft blueprints require a revision check before an implementation profile is frozen.

| Work | Examined baseline | Consequence for a VCP profile |
| --- | --- | --- |
| C2PA | [Technical Specification 2.5](https://spec.c2pa.org/specifications/specifications/2.5/specs/C2PA_Specification.html), whose version history identifies October 2026 | Pin assertion versions and validation rules; do not assume older consumers support every feature. |
| CAWG | [Identity Assertion 1.3](https://cawg.io/identity/1.3/), DIF Ratified Specification dated 17 August 2026; [Metadata Assertion 1.1](https://cawg.io/metadata/1.1/) | Distinguish the ratified methods from additional experimental credential or assertion work. |
| Originator Profile | [Originator Profile Blueprint](https://docs.originator-profile.org/en/opb/) and its linked models, marked draft | Record the exact model and securing mechanism used; a draft dependency needs explicit version handling. |
| W3C credentials | [VC Data Model 2.0](https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/) and [Bitstring Status List 1.0](https://www.w3.org/TR/2025/REC-vc-bitstring-status-list-20250515/), Recommendations dated 15 May 2025 | Credential semantics, securing mechanisms and status evaluation need distinct profile decisions. |
| Selective disclosure | [Data Integrity BBS Cryptosuites 1.0](https://www.w3.org/TR/2026/CRD-vc-di-bbs-20260910/), Candidate Recommendation Draft dated 10 September 2026 | Evaluate the chosen proof mechanism and its maturity separately from the VC data model. |
| W3C provenance | [PROV-DM](https://www.w3.org/TR/2013/REC-prov-dm-20130430/), [PROV-O](https://www.w3.org/TR/2013/REC-prov-o-20130430/) and [PROV Constraints](https://www.w3.org/TR/2013/REC-prov-constraints-20130430/), Recommendations; [PROV-AQ](https://www.w3.org/TR/2013/NOTE-prov-aq-20130430/), a Working Group Note | Separate semantic foundations and consistency rules from the status and implementation support of discovery guidance. |
| HTTP mechanisms | [RFC 8288](https://www.rfc-editor.org/rfc/rfc8288), [RFC 9530](https://www.rfc-editor.org/rfc/rfc9530) and [RFC 9421](https://datatracker.ietf.org/doc/html/rfc9421) | Link discovery, representation integrity and HTTP message authentication are distinct capabilities. |
| Software attestations | [SLSA 1.2](https://slsa.dev/spec/v1.2/), [in-toto Statement v1](https://github.com/in-toto/attestation/blob/06eafe3635bf8a425ad52cc82c6c90861e94a471/spec/v1/statement.md), [DSSE protocol 1.0.2](https://github.com/secure-systems-lab/dsse/blob/1d14ad6f941e31670a9ee42d64afd8ce1572bc37/protocol.md), and public Sigstore documentation | Preserve the predicate, signed payload and verification policy; the frameworks are not interchangeable assurance labels. |
| Software inventories | [SPDX 3.0.1](https://spdx.github.io/spdx-spec/v3.0.1/) and [CycloneDX 1.7 in ECMA-424 second edition](https://ecma-international.org/publications-and-standards/standards/ecma-424/) | Check which object, lifecycle and claim a field describes before mapping it to contribution provenance. |
| Domain metadata | [IPTC Photo Metadata 2025.1](https://iptc.org/std/photometadata/specification/IPTC-PhotoMetadata-2025.1.html), [CRediT](https://credit.niso.org/) and [DDEX RIN 2.1](https://kb.ddex.net/implementing-each-standard/recording-information-notification-%28rin%29/) | Reuse domain meanings and identifiers; metadata presence is a separate matter from evidence verification. |

## What a mapping must preserve

### Separate retention from interpretation

Three operations should have different names and results:

- **Retention** preserves the original assertion or credential, its encoding, relevant dependencies and its source identifier or digest.
- **Projection** expresses a selected interpretation in the VCP model. It identifies the mapping rules, their version and the party responsible for the interpretation.
- **New attestation** adds a statement by another party. It is a new provenance event, not a replacement signature by the original issuer.

Retaining a source unchanged can preserve information that a projection cannot express. A projection may be useful without being reversible. A mapping should report that loss rather than describe all three operations as equivalent conversion.

The default imported proposition is that a particular source issuer made a particular statement under the source rules. Independently accepting the described event is a separate decision. If an importer issues its own claim that the event occurred, that is a new Claim attributed to that importer, with its own supporting Evidence.

Where a source protects a canonical representation rather than the original encoding, its own securing rules determine the representation and dependencies needed for verification. VCP canonicalization is not a substitute for those rules.

### Preserve the verification question

The following is a proposed common contract for mapping work, not a new wire format.

| Dimension | Information to retain | Error the mapping must avoid |
| --- | --- | --- |
| Issuer and signer | Source issuer, signing credential, key or certificate, and any distinction between them | Presenting the importing system as the original witness or issuer. |
| Subject and participant | Credential subject, named actor, presenter, contributor and publisher, with the applicable bindings | Treating all identities mentioned in a record as the same person or organization. |
| Content binding | Target object, representation or selected region; digest algorithm and exact digest input rules | Equating a URL, normalized text digest, manifest hard binding and full-file digest. |
| Statement scope | The source proposition, role, lifecycle stage and referenced assertions | Turning publication approval into authorship or identity verification into action authorization. |
| Evidence and availability | Original evidence location or retained object, disclosures, omissions and retrieval state | Treating withheld or unavailable evidence as either nonexistent or successfully checked. |
| Time and status | Event time, issuance time, validity periods, evaluation time and status evidence | Treating a timestamp string as trusted time or present validity as proof of historical validity. |
| Verification | Native validation result, trust policy, supported features and any separately evaluated VCP result | Replacing several checks with one unexplained green status. |

An adapter should retain enough information for another implementation to repeat the claimed checks. If it preserves only a verifier's receipt, the receipt is evidence of that verifier's statement; independent verification of the underlying source still requires the relevant source material and trust inputs.

### Do not collapse identity or confidence

Matching names, email addresses, domains or account labels are candidate links, not universal equivalence rules. Different credentials can describe one Actor, and one credential holder can act for a different subject. Conversely, control of two keys does not establish two distinct humans.

An importing system's confidence score should not overwrite source-specific outcomes. For example, a trusted signature over an unsupported assertion is not the same result as a fully interpreted, independently supported contribution claim.

## C2PA and CAWG

### Capabilities to reuse

C2PA 2.5 provides signed manifests, content bindings, actions, ingredients and validation results. It includes HTML embedding rules and AI disclosure alongside source-type information. A manifest's claim is a signed structure referring to assertions; it is not identical to the VCP concept of one asserted proposition. The actions format also distinguishes software used for an action from the claim generator. [C2PA Technical Specification 2.5](https://spec.c2pa.org/specifications/specifications/2.5/specs/C2PA_Specification.html).

CAWG Identity Assertion 1.3 binds a named actor to selected hashed assertions and optional roles, including creator, contributor, editor, producer and publisher. Its model distinguishes the credential holder from the named actor, permits multiple identity assertions, and requires the referenced set to include the applicable hard binding. This is directly relevant to contribution attribution rather than merely an identity label. [CAWG Identity Assertion 1.3](https://cawg.io/identity/1.3/).

CAWG also defines a metadata assertion that can carry metadata from multiple standards without the field restrictions of the C2PA metadata assertion. That provides an existing place to examine domain metadata mappings before proposing another container. [CAWG Metadata Assertion 1.1](https://cawg.io/metadata/1.1/).

### Proposed VCP interpretation

| Source concept | Proposed VCP treatment | Boundary to preserve |
| --- | --- | --- |
| Manifest and claim | Retained Evidence container with its native source identifiers | A signature over the container does not identify every contributor as its signer. |
| Individual assertion | A source statement referenced by a VCP Claim or retained for later interpretation | The projection identifies its own issuer and preserves the original statement's issuer and scope. |
| Action description | Candidate Action and SemanticEvent description | A recorded action is not automatically proven human performance. |
| Ingredient relationship | Candidate Artifact relationship | Check direction, relationship kind and asset state; association is not necessarily derivation. |
| Software agent | Software or model Actor when the source justifies that classification | Do not substitute the authoring application or recorder for the person using it. |
| CAWG named actor and roles | Actor references, contribution roles and scoped attestation or participation evidence | Preserve the holder distinction and the exact assertions selected by the holder. |
| Native validation output | Evidence of the source checks, with policy and evaluation context | Retain unsupported or unevaluated dimensions instead of promoting the whole bundle to verified. |

CAWG's authorization or participation meaning should be retained where applicable. It should not be expanded into an unrestricted grant to an agent, permission for an unrelated future action or a complete delegation chain. Likewise, a generic identity credential should not be described as satisfying a CAWG signature method unless that method's requirements have actually been met.

### Content identity and AI disclosure

The mapping should record the domain of each digest. A format-specific hard binding and a digest over the complete stored file can cover different byte sequences. A discovery match through a fingerprint or other soft binding should remain a discovery result until the required integrity check has succeeded. [C2PA content-binding rules](https://spec.c2pa.org/specifications/specifications/2.5/specs/C2PA_Specification.html).

AI model and oversight disclosures deserve direct comparison with the [Semantic Event profile](SEMANTIC_EVENT.md). Existing source-type or oversight values can be retained as their original assertions. Translating them into events such as Generate, Review or Approve requires an explicit rule and sufficient event-level evidence; a general oversight classification should not manufacture an individual review event or reviewer.

### Candidate gaps and experiment

The useful experiment is to import a manifest with two named contributors, one model-related action and one edited ingredient. A second implementation should reproduce native validation, retain the signed source, and inspect the projected roles and relationships. Mutations should substitute an ingredient, remove a referenced assertion, change a role, or leave an extension uninterpreted.

A candidate gap is a portable account of how source assertions are interpreted across systems with different event and authority models. A new C2PA assertion is justified only if the workflow cannot be expressed by existing assertions, metadata and external references. Neither lack of an implementation in VCP nor a different preferred field name establishes such a gap.

## Originator Profile

### Separate organization and content concepts

Originator Profile's documentation describes draft blueprints for organization credentials, attributes, content attestations and web distribution. Its stated purpose concerns verifiable origin and integrity, not a judgment that content is true. [Originator Profile Blueprint](https://docs.originator-profile.org/en/opb/).

The Core Profile identifies an organization and its public-key set. A Profile Annotation describes attributes of the Core Profile subject under a separate issuer. Those are useful distinctions between an Actor, its verification material and externally asserted attributes. The Profile Annotation also distinguishes the validity period of the information from the validity of its securing JWT. [Core Profile](https://docs.originator-profile.org/en/opb/cp/), [Profile Annotation](https://docs.originator-profile.org/en/opb/pa/).

A Content Attestation identifies an issuer, an asserted content subject, allowed URLs and integrity targets. Its target can be less than the whole page. The integrity descriptor work distinguishes HTML, text, visible text and external-resource targets. [Content Attestation](https://docs.originator-profile.org/en/opb/ca/), [Content Integrity Descriptor](https://docs.originator-profile.org/en/opb/content-integrity-descriptor/).

### Proposed VCP interpretation

| Originator Profile concept | Proposed VCP treatment | Question to resolve |
| --- | --- | --- |
| Organization identified by a Core Profile | Organizational Actor with separately referenced verification material | Which trust policy binds the organization to those keys? |
| Profile Annotation | Identity or organizational-attribute Claim with issuer Evidence | Which attribute was checked, by whom and for what interval? |
| Web Media Profile | Publisher-related Actor attributes | What does the actual issuer attest, and which display information is covered? |
| Content Attestation | Evidence and scoped statements about an Artifact or selected content | Is the covered state full bytes, a DOM fragment, text, or another defined target? |
| Allowed URL and website association | Publication-context constraint | What happens on syndication, a new domain, a redirect or an offline copy? |
| Distributed credential sets | Retained source package or referenced Evidence | Which dependencies and trust inputs were available during verification? |

The publisher organization should remain distinct from the article author, editor, model and hosting service. A verified publisher credential can support a claim about the publisher without establishing that it wrote the article. A contributor's name inside content metadata likewise needs its own attribution and binding treatment.

The [Web Media Profile](https://docs.originator-profile.org/en/opb/web-media-profile/) supplies organization-oriented publishing information. Its fields should be mapped according to their issuer and model, rather than interpreted as a uniform third-party verification of every displayed attribute.

### Content scope matters more than a common digest label

A successful visible-text check cannot be silently reported as verification of executable code, page styling, hidden text or every image. The adapter should identify the precise target and processing method. If a page changes after verification, the earlier result applies to the evaluated state; the consumer needs an explicit rule for invalidating or recomputing that result.

Similarly, a URL constraint and content-integrity check answer different questions. A republication could preserve content while failing the original publication-location condition. The result should preserve both outcomes instead of choosing whichever makes the new page appear verified.

### Web association and candidate gap

The draft HTML association document already describes embedded and referenced sets using script elements and defined media types. It is therefore inaccurate to present the association of organizational credentials and content attestations with HTML as wholly missing work. [Originator Profile HTML association](https://docs.originator-profile.org/en/opb/link-to-html/).

The candidate mapping work is to join organization and content evidence to a finer contribution history, while preserving the original publication and target rules. The proposed test should retain publisher verification when an author's identity is undisclosed, and should show a scope mismatch when a credential covers visible text but a disputed change affects an uncovered image.

## W3C Verifiable Credentials

### What the data model supplies

VC Data Model 2.0 distinguishes issuer, credential subject, holder and verifier. The holder need not be the subject. It provides structures for issuer claims, validity periods, status references and supporting material, while the securing mechanism and application validation determine additional requirements. A credential's inclusion of a URL does not by itself protect the linked content. [VC Data Model 2.0](https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/).

The direct mapping is a credential containing claims about an Actor, not a credential becoming the Actor. A presenter may demonstrate control of a key without proving that it performed an earlier creative action. An identity attribute, organizational affiliation and authority to approve publication can require different issuers and different policies.

### Proposed credential evaluation sequence

A VCP identity or authority profile should specify how to:

1. Identify the credential type, version and securing mechanism without treating an arbitrary type label as proof of support.
2. Verify the protected source using the mechanism's exact rules and required verification material.
3. Determine which issuer is trusted for the particular attribute or mandate, rather than accepting every correctly signed issuer statement.
4. Bind the credential subject to the relevant Actor, keeping any presenter and delegate separate.
5. Check credential validity and required status evidence at a declared evaluation time.
6. Evaluate any presentation freshness, audience, challenge, holder-binding or transaction-binding conditions supplied by the selected presentation mechanism.
7. Report the evaluated attribute or authority scope, the evidence used and the remaining unsupported dimensions.

This is a proposed application sequence, not a claim that the VC data model defines one universal presentation protocol. In particular, a nonce or audience condition is meaningful only when the selected mechanism binds and checks it.

### Status and historical evaluation

Bitstring Status List 1.0 provides a mechanism for publishing status such as revocation or suspension. Its design reduces reliance on contacting an issuer for each individual credential. A consumer still needs a policy for freshness and retrieval, and should preserve the evaluated status purpose. [Bitstring Status List 1.0](https://www.w3.org/TR/2025/REC-vc-bitstring-status-list-20250515/).

Credential revocation, expiry, a revoked signing key, a withdrawn authority grant and a disputed contribution claim are distinct conditions. Revoking an affiliation credential should not erase the historical fact that it was issued. Whether it supports an event at an earlier time depends on the event-time evidence, status history and relying-party policy; a current successful check alone does not settle that historical question.

### Selective disclosure and correlation

Selective disclosure is a capability of a selected securing and presentation mechanism, not a property obtained merely by calling an object a VC. The BBS cryptosuite draft supports selective disclosure and unlinkable derived proofs. Its Candidate Recommendation Draft status should be retained when selecting implementation dependencies. [Data Integrity BBS Cryptosuites 1.0](https://www.w3.org/TR/2026/CRD-vc-di-bbs-20260910/).

The proposed VCP binding should also examine what remains correlatable outside the proof: subject identifiers, stable key references, status-list positions, evidence URLs and repeated publication context. Removing a legal name while leaving a global identifier can preserve a substantial link between activities. The profile should specify which linkage is intentional and which disclosure the user can avoid.

### Signature security is not inherited across layers

The main VCP draft proposes hybrid post-quantum authenticity requirements. Carrying an existing credential inside a newly protected VCP package does not replace the original issuer's signature or change its cryptographic assumptions. The source proof and the enclosing package must have separate security results.

A concrete profile decision is still needed about how externally secured evidence can be represented and relied upon under those requirements. A new signature over retained evidence can establish that the wrapper signer committed to those bytes; it cannot retrospectively establish a stronger issuer authentication or a different original attestation. This decision should remain explicit rather than weakening the main draft through an adapter convention.

## W3C PROV and authority expression

### A semantic foundation already exists

PROV-DM describes entities, activities, agents, derivation and bundles, including provenance of provenance. PROV-O expresses that model in an extensible ontology. These are strong candidates for the underlying graph, rather than work VCP should describe as missing. [PROV-DM](https://www.w3.org/TR/2013/REC-prov-dm-20130430/), [PROV-O](https://www.w3.org/TR/2013/REC-prov-o-20130430/).

### Proposed semantic crosswalk

| VCP concept | Existing PROV construct to examine | Mapping qualification |
| --- | --- | --- |
| Artifact or artifact state | `prov:Entity` | The profile still needs exact-state identity and digest rules. |
| Action | `prov:Activity` | Keep the description of an activity distinct from the assertion that it occurred. |
| Human, organization or software Actor | `prov:Agent` and applicable subclasses | Additional Actor kinds should specialize the model without inferring legal identity. |
| Consumed input | `prov:used` | Preserve which activity used which entity and any qualified role. |
| Produced output | `prov:wasGeneratedBy` | Preserve generation direction and the relevant state. |
| Derivation | `prov:wasDerivedFrom` | Retain more-specific domain meanings where available. |
| Participation and contribution role | Qualified association and `prov:hadRole` | Association does not express a universal amount or legal status of contribution. |
| Revision | `prov:wasRevisionOf` | Revision of content is not automatically correction of a provenance claim. |
| Description set | `prov:Bundle` | Retain attribution and provenance of the description itself. |
| Delegation | `prov:actedOnBehalfOf` and qualified delegation | Map the specific activity and agents; additional authorization checks need an explicit profile. |

This is a proposed crosswalk. It should not be implemented as blanket equivalence declarations between the VCP and PROV terms: some VCP concepts have stricter requirements or a different assertion boundary.

### Claims and evidence need a graph interpretation rule

PROV's ability to describe a history does not decide which statements a relying party should believe. A VCP profile could represent a Claim as an Entity, the making of that Claim as an Activity, and the issuer as an Agent. Evidence objects and verification activities can then have their own provenance. The underlying described activity remains distinguishable from the activity of asserting it.

An implementation must also decide where asserted relationships enter its query or reasoning graph. Importing an untrusted source triple directly into an accepted fact graph can defeat the separation even when the original bytes are retained elsewhere. Keeping source statements in attributed bundles and explicitly controlling which interpretations are accepted is a candidate pattern to test.

PROV Constraints addresses consistency of provenance histories and their associated inference rules. Passing those constraints is useful structural evidence; it is not verification of a signature, issuer trust or truthfulness of the described history. [PROV Constraints](https://www.w3.org/TR/2013/REC-prov-constraints-20130430/).

### Delegation is not a new VCP invention

PROV-O's delegation concept already includes assignment of authority and responsibility to an agent for a specific activity. VCP should preserve that meaning. [PROV-O delegation](https://www.w3.org/TR/2013/REC-prov-o-20130430/). The additional candidate work concerns enforceable scope, validity, redelegation restrictions, revocation evidence and the verification policy needed for a relying party's authorization decision.

ODRL 2.2 is also relevant: it models permissions, prohibitions, duties and constraints over assets and actions. A VCP Authority profile should compare these policy concepts before defining a separate expression language. [ODRL Information Model 2.2](https://www.w3.org/TR/2018/REC-odrl-model-20180215/).

A policy expression, a signed grant and a recorded execution are three distinguishable objects. A useful joint experiment would ask whether a PROV delegation relation plus a referenced policy and grant can express a bounded human-to-agent workflow. Only the missing semantics should motivate additional vocabulary.

## URLs HTTP HTML and web integrity

### Separate discovery from binding and trust

There are at least five questions in web verification:

1. Where is a candidate evidence object advertised?
2. Which resource, representation, version or subpart does it describe?
3. How is that scope cryptographically bound to the evaluated content?
4. Which issuer or signer is trusted for the particular statement?
5. How should the resulting limited evidence be presented to the reader?

One mechanism does not have to answer all five. A generic evidence-discovery layer can retain specialized validation for the evidence it finds.

### Discovery mechanisms already available

RFC 8288 defines web links, link relation types and the HTTP Link field. PROV-AQ describes HTTP and HTML approaches to associating provenance with a resource, including an extension relation and an optional target anchor. PROV-AQ is a Working Group Note, not a Recommendation or a promise of browser support. [RFC 8288](https://www.rfc-editor.org/rfc/rfc8288), [PROV-AQ](https://www.w3.org/TR/2013/NOTE-prov-aq-20130430/).

For example, a proposed experiment can advertise a provenance document without inventing a new relation:

```http
Link: <https://example.org/evidence/article-v2>; rel="http://www.w3.org/ns/prov#has_provenance"; anchor="https://example.org/articles/42/versions/2"
```

The example adapts the PROV-AQ pattern. It identifies a candidate provenance record and its target identifier. It does not authenticate the record, bind the displayed bytes, establish author identity or define a new VCP media type. An experiment must check how the target is identified inside the retrieved record.

C2PA's HTML rules and Originator Profile's association mechanism are additional specialized candidates. An initial study should compare those approaches with generic linking, including multiple or conflicting evidence sources, instead of asserting that all HTML integration requires new work.

### Different integrity mechanisms protect different objects

| Mechanism | Existing protection | What a VCP consumer still needs |
| --- | --- | --- |
| HTTP digest fields | `Content-Digest` covers message content; `Repr-Digest` covers representation data under its specified rules. | An authenticated expected digest, the matching digest scope and any statement about the creator. |
| HTTP Message Signatures | Authentication of selected HTTP message components, subject to the chosen key and policy. | Deliberate component coverage, replay policy and evidence that binds the claimed Artifact state. |
| Subresource Integrity | Digest checking in supported subresource loading paths. | Contributor identity, authority and a history of transformations. |
| Content-format integrity descriptor | Integrity under that format's target selection and processing rules. | A clear description of which content is outside the covered target. |

The first three rows refer to [RFC 9530](https://www.rfc-editor.org/rfc/rfc9530), [RFC 9421](https://datatracker.ietf.org/doc/html/rfc9421) and the [Subresource Integrity Recommendation](https://www.w3.org/TR/2016/REC-SRI-20160623/), respectively. A digest alone is not an authenticated claim about who created the object. An HTTP signature must deliberately protect the components needed by the application; it should not be assumed to cover all related content automatically.

### Representation identity needs explicit rules

The experiment should handle these cases separately:

| Case | Proposed interpretation rule |
| --- | --- |
| The same URL returns a later article revision | Re-evaluate against that revision; do not carry forward a result attached only to the earlier state. |
| Compression or a partial response changes the message bytes | Apply the selected digest mechanism's exact content or representation rules. |
| A page is localized or personalized | Identify the evaluated variant; shared location does not imply shared content. |
| A script changes the DOM after verification | State whether the result concerns response bytes, an earlier DOM state or the new content. |
| A resource is syndicated to another site | Preserve content and issuer checks separately from any original location constraints. |
| A selector finds the same text in a different context | Validate the target and its state; a selector match alone is insufficient attribution. |

VCP should not choose one universal canonicalization procedure for all these objects by accident. Raw files, C2PA-bound assets, DOM targets, RDF datasets and text excerpts may need different, explicitly identified processing rules.

### Retrieval and user-agent behavior

A proposed consumer policy should bound redirects, response sizes, recursive references and processing time. It should avoid sending ambient credentials to arbitrary evidence origins and should define what network access is allowed during validation. Evidence retrieval must not quietly become a mechanism for tracking every page a person reads.

User-agent results should identify the checked scope and question: for example, publisher attribute checked, image integrity checked, review claim unsupported, or authority status unavailable. Native browser trust indicators should not be implied by a successful library-level check. A CG experiment should test understandable wording, accessibility and the difference between absent metadata and detected tampering.

## Software supply chains and source workflows

### SLSA already includes source provenance

SLSA 1.2's Source requirements cover revision history, the process for producing source and evidence from the source control system. The specification deliberately leaves detailed source-provenance formats to those systems while defining a more common verification-summary approach. This is a potential interoperability seam, not an absence of source-provenance work. [SLSA Source requirements](https://slsa.dev/spec/v1.2/source-requirements).

The Build provenance predicate separately describes outputs, a build definition, resolved dependencies, the builder and run details. Source and build evidence should be joined at the identified source revision rather than collapsed into one claim about a human author. [SLSA Build provenance](https://slsa.dev/spec/v1.2/build-provenance).

### Proposed crosswalk

| Existing evidence | Proposed VCP interpretation | Unsupported inference to avoid |
| --- | --- | --- |
| Source revision identifier and parents | Artifact state and lineage | A revision label alone proves the identity or creative contribution of its author field. |
| Recorded review or approval | Source-system statement about a review Action and its target | A review of one revision approves every later revision. |
| Source verification summary | Verifier attestation about evaluated source properties | The summary supplies every underlying event or independently verifies human conception. |
| Build predicate inputs and outputs | Artifact references linked through a build Action | A successful build attests to every development decision that preceded it. |
| Builder identifier | Actor or execution-environment reference under the source model | The builder is the human creator or the model that generated the source. |
| Test or scan result | Scoped Evidence about the tested state and tool invocation | A passing test proves provenance correctness or authorization. |

A useful contribution profile would preserve the event granularity that a source system actually exposes and mark information it cannot establish. It should compare existing review, test and source attestation practices before registering a new contribution predicate.

### in-toto DSSE and Sigstore serve different layers

An in-toto Statement v1 binds a predicate type and predicate to subjects identified by digests. The artifact subject is not the identity credential subject: the same word names different concepts in the two ecosystems. [in-toto Statement v1](https://github.com/in-toto/attestation/blob/06eafe3635bf8a425ad52cc82c6c90861e94a471/spec/v1/statement.md).

DSSE signs a pre-authentication encoding of the payload type and serialized payload. A VCP importer should therefore retain the signed bytes and type, not reserialize selected JSON and claim that the original signature verifies over the projection. [DSSE protocol](https://github.com/secure-systems-lab/dsse/blob/1d14ad6f941e31670a9ee42d64afd8ce1572bc37/protocol.md).

Sigstore verification introduces its own signer-identity and issuer policies. Its bundle format can retain certificates, signatures and transparency material; log entries are not universally required by the bundle specification. The consumer must inspect the evidence actually present and the verification policy actually used. [Sigstore signature verification](https://docs.sigstore.dev/cosign/verifying/verify/), [Sigstore bundle format](https://docs.sigstore.dev/about/bundle/).

A verified workload or account identity should remain that kind of identity evidence. It should not be renamed verified human author. Likewise, log inclusion is a claim about a log event and its verification context, not a general finding that every assertion in an attestation is true.

### Inventories and lifecycle descriptions also overlap

SPDX 3.0.1 includes a Build profile with input, output, invocation and tool relations. Its CreationInfo concerns the creation of the SPDX Element; mapping that information requires attention to the object being described rather than automatic attribution of the underlying work. [SPDX Build profile](https://spdx.github.io/spdx-spec/v3.0.1/model/Build/Build/), [SPDX CreationInfo](https://spdx.github.io/spdx-spec/v3.0.1/model/Core/Classes/CreationInfo/).

CycloneDX 1.7, published as ECMA-424 second edition, includes lifecycle and formulation information as well as claims, attestations and supporting evidence. It should not be treated as merely an inventory of component names. [ECMA-424](https://ecma-international.org/publications-and-standards/standards/ecma-424/).

A VCP software profile should identify the precise lifecycle object and field semantics it reuses. A relationship in an inventory, an observation produced by a scanner and a signed execution attestation may all concern the same artifact while carrying different evidence and assurance.

## Creative and research domain formats

### Reuse contribution vocabularies without importing conclusions

A generic event model does not require a generic replacement for every professional role. The proposed domain-profile rule is to retain the original role or activity identifier, add a broader VCP event classification only when justified, and preserve a source term even when a consumer cannot interpret its finer meaning.

| Work | Existing semantics to reuse | Proposed VCP boundary |
| --- | --- | --- |
| IPTC Photo Metadata | Photo creation, source classification, creator and AI-related metadata | Treat fields as attributed metadata until their provenance and binding are evaluated. |
| CRediT | Contributor roles in research outputs | A contribution role does not establish authorship criteria, ownership or a percentage of the work. |
| DDEX RIN | Recording-session contributors, roles, creations and related production information | Distinguish performance, composition, engineering, production and rights-related statements. |
| Web Annotation | Bodies, targets and selectors for resources or subparts | Locating a segment is not proof of its integrity or its contributor. |

### Photography and AI metadata

IPTC Photo Metadata 2025.1 includes AI prompt information, prompt-writer name, system and system-version fields. Its prompt-writer definition explicitly distinguishes that person from the image creator. A VCP mapping should preserve this distinction instead of treating everyone connected to an AI invocation as having performed the same creative action. [IPTC Photo Metadata 2025.1](https://iptc.org/std/photometadata/specification/IPTC-PhotoMetadata-2025.1.html).

The proposed mapping would represent the prompt, generation process and resulting image as distinguishable objects and activities where the available records support them. A prompt-writer label should not manufacture a signed human attestation, and an undisclosed prompt should not be represented as absent. Existing IPTC terms should remain identifiable if carried by a signed metadata assertion.

### Research contributor roles

CRediT describes 14 contributor roles and allows multiple roles per contributor and multiple contributors per role. Its stated purpose does not define who qualifies as an author. Optional contribution qualifiers are not a universal numeric allocation. [CRediT](https://credit.niso.org/), [CRediT contributor-role guidance](https://credit.niso.org/contributor-roles/).

A research profile could preserve roles such as conceptualization, investigation, software and writing while distinguishing the allocation of a role from evidence of an activity. A person's opportunity to confirm a role can be represented as a separate attestation. The mapping should not turn a taxonomy assignment into a legal finding or a machine-derived estimate of effort.

### Music production

DDEX RIN 2.1 communicates recording-session metadata across the music supply chain, including contributors, roles, recordings and related session objects. This is existing support for detailed contributions, not a blank field awaiting a new universal vocabulary. DDEX also specifies implementation terms that a domain binding must account for. [DDEX RIN guidance](https://kb.ddex.net/implementing-each-standard/recording-information-notification-%28rin%29/), [DDEX RIN overview](https://ddex.net/standards/recording-information-notification/).

A proposed music mapping would keep a composition, a performance, a recorded take and a mixed recording as separate Artifact or activity contexts. It would retain the party assigning a credit and the source message, rather than assuming the credited person signed it. Commercial attribution and rights allocations should remain distinct from evidence that a particular session action occurred. A public evidence projection may also need less information than a business-to-business production message.

### Selectors and subparts

The Web Annotation Data Model already offers resource targets and selectors, including mechanisms for text and other segments. A VCP profile should compare those mechanisms before defining new text-range, page, region or temporal selectors. [Web Annotation Data Model](https://www.w3.org/TR/annotation-model/).

The proposed experiment should bind the selector to an identified content state. It should distinguish failure to locate the target, ambiguity between multiple matches, a changed target and successful location without integrity evidence. Reusing a selector vocabulary does not remove those verification questions.

## A worked lifecycle across the ecosystems

Consider a research article containing human-written analysis, an illustration generated with a model, and a small software package used to produce a figure. An editor approves a particular article state and a publisher releases it. Later the illustration is cropped and the article is syndicated.

The following is a proposed composition of existing mechanisms, not an interoperable package already defined by those standards.

| Stage | Evidence or semantics that could be reused | VCP interpretation to test |
| --- | --- | --- |
| Research contribution | CRediT role assignment and an optional contributor confirmation | Retain the assigning party, role, target and independent confirmation. |
| Model-assisted image production | IPTC AI metadata and C2PA source, action or model disclosures | Keep prompt authorship, generation and image creation distinguishable. |
| Image attribution | CAWG identity assertion covering selected assertions | Preserve named actor, holder, role and content binding. |
| Software revision and build | Source-system attestations, SLSA build provenance and in-toto subjects | Join the exact source and output states without inventing missing development events. |
| Editorial approval | A scoped signed statement represented using the proposed Claim and Evidence treatment | Bind the approval to the article state and the editor's actual authority. |
| Publisher identification | Originator Profile organization and attribute material | Evaluate publisher attributes without attributing all writing to that organization. |
| Web publication | Existing format association or a PROV-AQ link | Discover evidence and then evaluate its target, signature and trust separately. |
| Crop and syndication | Retained derivation history and the applicable new content binding | Preserve original results while evaluating the new artifact and publication context. |

A reader could obtain different results for each object: the image binding succeeds, the publisher attribute is trusted, the source build matches, the editor's mandate status is unavailable, and the author's legal identity is undisclosed. That is a more informative result than declaring the entire article authentic.

The original credentials and manifests should remain available or explicitly unavailable under the selected retention policy. A translation receipt may record which mapping produced the combined view, but it should not replace the original issuers or claim that its author witnessed every earlier event.

## Candidate gaps and decisions

The following are conclusions of this mapping study, not assertions that the maintaining communities lack these capabilities in all forms.

| Candidate work item | Existing starting point | Classification and next decision |
| --- | --- | --- |
| Common agent, artifact and event graph | PROV-DM and PROV-O | Reuse first. Test a profile before proposing another foundational ontology. |
| Contributor roles and AI participation | CAWG, C2PA, IPTC and domain vocabularies | Mapping work. Determine which values are equivalent, broader, narrower or not comparable. |
| Identity and content association | VC models, CAWG and Originator Profile | Mapping and policy work. Preserve the distinct issuer, subject, holder and target bindings. |
| Evidence discovery on the web | RFC 8288, PROV-AQ and format-specific associations | Integration experiment. A new relation or API needs an unmet requirement. |
| Exact state and selected-content interpretation | Format bindings, HTTP integrity and annotation selectors | Profile work. Make representation scope explicit and test differing processing rules. |
| Bounded delegated authority | PROV delegation, ODRL and independently secured grants | Candidate gap in a concrete verification profile, not in the existence of delegation or policy concepts. |
| Cross-format verification results | Source-native validators and application policies | Candidate common reporting model. It must retain source detail and avoid a uniform assurance score. |
| Privacy and source availability | Credential disclosure mechanisms and evidence-retention policies | Cross-system profile work, including status, correlation and retrieval behavior. |
| Historical validity and dispute handling | Source status mechanisms, retained evidence and attributed claim histories | Candidate lifecycle profile. Preserve each status meaning and its time basis. |
| Security requirements for external evidence | Source securing mechanisms and the proposed VCP security profile | Explicit unresolved policy decision. Wrapping cannot upgrade the original issuer's signature. |

Three decisions should precede a normative VCP interchange format:

- Determine whether a PROV-based interpretation profile plus preserved source objects meets the initial use case.
- Define how limited or conflicting native verification results can be represented without inventing a common trust level.
- Resolve what the VCP security requirements demand of externally secured evidence and distinguish that from the integrity of a new VCP package.

A new universal identity service, ledger, authoring application or replacement for professional metadata is not required by this study. Any additional mechanism should have a specific test that fails with the reusable mechanisms and succeeds with the proposed addition.

## Interoperability experiments

### Common fixture set

The first fixture set should describe one article, one image, one source revision and build output, and a later edited publication. It should include two human contributors, a publisher organization, a model and an agent acting under a bounded mandate. A pseudonymous contributor and an undisclosed prompt should be first-class cases.

A fixture package would retain the original source objects and define the exact inputs for each expected result. The independently implemented consumer should not call the producer's verification functions under a different interface. Expected outcomes should distinguish source-native validation, mapping correctness, trust-policy acceptance and VCP-specific requirements.

### Positive and adversarial cases

| Test | Expected property |
| --- | --- |
| Retain and retrieve an original signed source | The source can be verified under its own mechanism; the projection is identified separately. |
| Substitute an image or ingredient | The applicable binding fails; a valid outer wrapper cannot rescue it. |
| Change a role or the selected assertion set | The original signature or relevant binding does not authenticate the modified meaning. |
| Use a credential held for another subject | Holder and subject remain distinct; unsupported Actor equivalence is refused. |
| Present publisher credentials for an author | Publisher verification does not become author identity verification. |
| Replay a fresh-presentation result in another context | A profile requiring challenge or audience binding rejects the substituted context. |
| Remove or stale required status evidence | The dependent result is unavailable or fails the selected policy; it is not silently accepted. |
| Replace full-content scope with visible-text scope | The consumer retains the narrower result and does not claim the uncovered regions were checked. |
| Resolve the same URL to another revision | The previous result does not apply without the required new state check. |
| Exceed a delegated Artifact or action scope | Authority evaluation refuses the escalation even when identity checks pass. |
| Encounter an unknown source extension | The original is preserved; unsupported interpretation is reported according to native and profile rules. |
| Hide private prompts and legal identity | The supported integrity and pseudonymous contribution checks still work. |
| Remove network access | Retained evidence remains inspectable; missing live dependencies are reported precisely. |
| Round-trip a lossy projection | The test reports lost semantics; it does not claim recreation of the original signed object. |
| Wrap evidence under a stronger algorithm profile | Original issuer assurance is unchanged; the wrapper's signature is evaluated independently. |
| Add a correction or dispute | History is preserved and the affected statement is distinguished from unaffected content checks. |

### Deliverables and review questions

The proposed first deliverables are a field-level mapping document, a source-preservation and verification-result contract, a versioned fixture package and two independent consumer reports. Each mapping should state whether it is lossless for the selected purpose, what information it cannot express and how unknown source features behave.

Review should involve the relevant content, identity, provenance, web-platform and domain communities. The questions are concrete: which existing semantics were changed; which scope bindings were omitted; which results were overinterpreted; what must remain private; and whether the same fixture can be understood without the originating implementation.

Only after those results should the group decide whether to incubate a new assertion profile, result vocabulary or association mechanism. A documented mapping that lets independent systems answer the original contribution question may be the useful standardization outcome by itself.
