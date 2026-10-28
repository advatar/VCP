# VCP — Verifiable Contribution Provenance

VCP is a proposal for describing and independently checking contributions by humans, software, AI models and autonomous agents to digital artifacts.

> Who or what contributed to this exact content state, in which role, under whose authority, and what evidence supports each assertion?

The model keeps **identity**, **authority**, **provenance**, **claims** and **evidence** distinct. A signature can protect a record without proving that the described event occurred. A verified identity does not grant authority. A publication can preserve the roles of its author, editor, publisher, AI model and delegated agent instead of collapsing them into one creator.

**Canonical public proposal:** [W3C.md in this repository](W3C.md).

## Read the proposal

- **[W3C discussion proposal](W3C.md)** — meeting brief, suggested opening, full contribution model, privacy and security requirements, interoperability questions and proposed next steps.
- **[Semantic Event companion](SEMANTIC_EVENT.md)** — a candidate vocabulary for describing contribution activities across publishing, research, software and other domains.
- **[Proposal status](STATUS.md)** — current scope and publication checks.

## W3C discussion context

This proposal was prepared for the [Content and Originator Authenticity Community Group kickoff at TPAC on 28 October 2026](https://www.w3.org/events/meetings/f25640e1-5025-4d05-92de-e1bd4b41b656/).

It offers a mixed human–AI publishing use case for the [COA CG's work](https://www.w3.org/community/coa-cg/): connecting content provenance, originator attributes and web integration while preserving privacy. The proposal asks how C2PA, CAWG, Originator Profile, Verifiable Credentials, W3C PROV and web mechanisms can be used together, and which gaps remain.

The starting example is an article with human-written text, an AI-generated illustration, editorial revision and publisher approval. The proposed experiment would let a reader inspect evidence for the exact content state, including a pseudonymous author, private prompts, missing credentials and subsequent corrections.

## Status and scope

This is an author's discussion proposal, not a W3C specification or an adopted Community Group report. Normative keywords in the drafts express proposed VCP requirements for review.

This repository contains the complete public proposal and its semantic companion. All definitions and illustrative examples needed to read them are included here. External references point to public standards, meeting context or this repository; no private repository, platform service or schema download is needed.

The proposal is independent of any commercial implementation, identity provider, hosting platform or distributed ledger. It does not yet publish an agreed wire format, normative schema or conformance suite, and it makes no claim of completed interoperability.

## Feedback

Please [open an issue](https://github.com/advatar/VCP/issues) with use cases, questions, mapping suggestions or implementation feedback. Useful topics include exact-version binding, provenance discovery, originator attributes, delegated authority, privacy, user-agent presentation and reuse of existing standards.
