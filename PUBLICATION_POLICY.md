# Publication Policy

- Status: **approved**
- Version: **1.0**
- Last reviewed: **2026-09-06**
- Scope: all public content in this repository

## 1. Principle

This repository contains independently created personal engineering practices only.

It must not disclose, reconstruct, or closely paraphrase non-public employer, client, or partner information.

## 2. Allowed sources

Allowed source classes include:

- public standards and laws;
- official vendor documentation;
- public open-source documentation and code, subject to license terms;
- publicly available engineering practices;
- independently created synthetic examples;
- the owner's own general principles that do not reveal confidential work product;
- previously published public material owned by the repository owner.

## 3. Prohibited content

Do not publish:

- employer/client source code or derivative snippets based on non-public code;
- internal coding standards, QA policies, templates, checklists, architecture documents, meeting notes, proposals, or runbooks;
- customer identities tied to non-public incidents, vulnerabilities, architectures, or business logic;
- internal IP addresses, hostnames, URLs, ports, account names, credentials, tokens, keys, or secrets;
- real internal schema/table/column names or production data;
- proprietary algorithms, scoring logic, business rules, pricing logic, or operational procedures learned from confidential work;
- screenshots of internal/admin/customer systems;
- text that is technically rewritten but still allows a reader to reconstruct a confidential source.

## 4. Work-experience boundary

A lesson learned at work may be used only after converting it into an independently justified general rule.

Safe transformation requires all of the following:

1. remove employer/client identity and environment-specific details;
2. do not reuse non-public wording, diagrams, code, or templates;
3. justify the rule from public standards, public documentation, or independently reproducible evidence where possible;
4. use synthetic examples;
5. ensure the final rule is useful without knowledge of the original project.

If a rule cannot be explained without confidential context, do not publish it.

## 5. Employer-policy uncertainty

As of 2026-09-06, no explicit employer policy authorizing or prohibiting publication of a personal engineering handbook has been verified in the sources available to this workflow.

Therefore the repository uses the conservative rule:

> Do not publish employer-derived material. Publish only independently created, public-source-grounded, synthetic, general engineering practices.

If an applicable employer policy is later obtained, it overrides this repository when stricter and this document must be reviewed.

## 6. Pre-publication checklist

Before merging substantive content, confirm:

- [ ] no company/client source material is copied or closely paraphrased;
- [ ] examples are synthetic or already legitimately public;
- [ ] no internal identifiers, infrastructure, data, or credentials appear;
- [ ] licenses/attribution requirements for public sources are respected;
- [ ] claims distinguish tested fact, official guidance, inference, and personal preference;
- [ ] the document has a status and review date;
- [ ] sensitive or ambiguous material is removed rather than rationalized into publication.
