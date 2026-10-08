# Designing content people (and AI) can find, understand, and trust

An engineer needs to store a service credential. They find the documentation, understand the instructions, and still have a question: *Is this the right place to store this kind of secret?*

That uncertainty is a content design problem. Useful content helps people decide what applies, understand what to do, and recognize when they need a different path. It also gives AI applications better source material for answering those same questions.

The connected examples below use a fictional Secrets Vault service and security control. The design principles also apply to enterprise workflows, product help, and developer documentation.

**On this page**

- [Start with the reader's decision](#start-with-the-decision-the-reader-needs-to-make)
- [Keep context close to the action](#keep-context-close-to-the-action)
- [Reuse content without losing meaning](#reuse-content-without-losing-its-meaning)
- [Terminology, taxonomy, ontology, and knowledge graphs](#give-content-a-shared-vocabulary-and-structure)
- [Make metadata useful](#make-metadata-useful-to-the-workflow)
- [Connect ownership and approval](#keep-ownership-and-approval-connected-to-the-content)
- [Evaluate what people and AI receive](#check-what-people-and-ai-actually-receive)

## Start with the decision the reader needs to make

Before organizing pages, identify the reader's questions and give each content type a clear responsibility.

| Reader's question | Where the answer belongs |
| --- | --- |
| What handling principles apply? | [Secure Data Handling](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/secure-data-handling.md#data-storage) explains the broader context. |
| Is this storage service appropriate? | [Standard 1234](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-standard-1234.md#storage-eligibility) defines eligibility and requirements. |
| How do I get access and use it? | The [Vault procedure](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/vault-secrets-management.md#3-onboarding-and-configuring-access) provides the steps. |
| Who reviews a change? | The [governance dashboard](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-controls-health-dashboard.md#document-owners) identifies accountable owners. |

Readers may arrive directly at a procedure through search, a product interface, or an AI answer. Put essential eligibility information and prerequisites there, too. They should not have to reconstruct the documentation hierarchy before getting started.

## Keep context close to the action

The Vault example places storage restrictions before onboarding. Apply the same principle to destructive actions: place a warning before the command or step that causes an irreversible change. Information must be available while the reader can still use it to make a decision.

Use progressive disclosure to keep the main task manageable: present essential information first, then offer supporting explanations. Keep critical restrictions visible.

[Descriptive link text](https://www.w3.org/WAI/WCAG22/Understanding/link-purpose-in-context.html) helps readers predict where they will go, including when navigating with assistive technology. It also gives AI applications useful context about where information belongs. When link text and its destination are preserved in retrieved content, a label such as “secret storage eligibility requirements” can help an AI assistant direct someone asking about storage rules to the relevant section.

Consider this refinement of a reference in the Vault sample:

**Before:** “See the note at the top of this page.”

**After:** “Do not store external-customer secrets or HSM-required key material here. Check the [secret storage eligibility requirements](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-standard-1234.md#storage-eligibility) before uploading.”

The revised wording identifies both the information and where to find it. It remains meaningful even when the passage appears outside its original page, such as in a search result or an AI answer. In product help, use the same names for actions and objects that people encounter in the interface.

## Reuse content without losing its meaning

Maintain requirements in an authoritative source and link to them from supporting guidance. Repeat a short restriction when readers need it immediately; reference the full rule for scope and exceptions.

The standard, data-handling guide, and Vault procedure share a governance note explaining ownership, approval, and revision tracking. Currently, those notes are repeated Markdown text. Automated reuse would mean maintaining the note once and including it wherever needed. A shared component still needs an owner, and a change should trigger review of the pages that use it.

Choose reuse boundaries around complete meanings: a prerequisite, warning, procedure, or answer. Keep its applicable system, conditions, and essential warnings together. A fragment such as “Do not store these here” cannot travel reliably without its surrounding context.

## Give content a shared vocabulary and structure

These concepts solve different problems. Use the ones that support your readers and maintenance needs.

| Element | Meaning and example |
| --- | --- |
| **Semantically enriched modular content** | Self-contained, reusable content components with metadata describing their meaning, purpose, and relationships. A role-request procedure might identify its audience, applicable service, prerequisites, and governing control. |
| **Metadata** | Structured information about content, such as its identifier, type, audience, owner, status, and review date. A field such as `content_type: procedure` describes what the content is; it does not establish that the procedure is accurate. |
| **Terminology** | A managed vocabulary of preferred terms, definitions, synonyms, abbreviations, and deprecated terms. Define “service owner” and “document owner” separately so their responsibilities remain clear. |
| **Taxonomy** | Defined categories and subcategories for organizing and labeling content. A product documentation hierarchy might group quickstarts under Get started, task instructions under User guides, and deeper explanations under Concepts and architecture. |
| **Ontology** | An explicit model of domain concepts, their characteristics, and relationships. It defines types such as Service and Procedure, attributes such as version and review date, and connections such as **applies to**, **supports**, and **requires**. |
| **Knowledge graph** | Specific entities and their relationships, often organized using an ontology. It could record that the Vault onboarding procedure **applies to** Secrets Vault and **supports** SEC-1234. |

### Taxonomy gives readers a predictable place to look

A taxonomy defines categories and how they relate. A familiar way to present it is a folder-like hierarchy, with broad sections containing more specific topics. For example, a product documentation collection could use this navigation structure:

**Product documentation**

- **Get started**
  - Product overview
  - Quickstart
- **User guides**
  - Configure a service
  - Manage access
- **Integrations**
  - Connect an application
  - Configure automation
- **Concepts and architecture**
  - How the service works
  - Security and access model
- **Reference**
  - API endpoints
  - Configuration options
- **Troubleshooting**
  - Common errors and fixes
  - FAQs

Quickstarts give readers a short path to a first successful outcome. User guides support ongoing tasks. Concepts and architecture provide the deeper explanations behind those tasks, while reference pages supply precise details to look up.

Define what belongs in each category so contributors classify new content consistently. Keep FAQs focused on recurring questions; move substantial procedures and explanations into the relevant guides.

These categories can appear as folders or navigation sections without dictating where source files are stored. A page can also carry classifications such as audience and product version, making it discoverable through several routes without duplicating its content.

### Ontology describes what things are and how they connect

An ontology defines the kinds of things in a domain, their characteristics, and relationships with specific meanings. It lets you express connections that extend across the navigation hierarchy.

For content design, start with three building blocks:

| Building block | What it describes | Example |
| --- | --- | --- |
| **Classes or types** | The kinds of things represented in the model. | `Service`, `Procedure`, `Task`, `Control`, and `User Role`. |
| **Attributes** | Named characteristics whose values describe individual things. | A procedure's identifier, version, status, and review date. |
| **Relationships** | Named connections between things, with a defined meaning and direction. | A procedure **applies to** a service, **supports** a control, or **requires** another task. |

A class is a type, not a particular object: `Procedure` is a class; the Vault onboarding procedure is an instance of that class. In OWL terminology, both attributes and relationships are kinds of properties.

For the connected samples, a model could define the following relationships:

| Source | Relationship | Target |
| --- | --- | --- |
| Vault onboarding procedure | **applies to** | Secrets Vault |
| Vault onboarding procedure | **supports** | Control SEC-1234 |
| Vault onboarding procedure | **is intended for** | Service owner role |
| Write a secret task | **requires** | Verify access task |

The ontology defines the types and relationship meanings; a knowledge graph can record these particular connections. Together, they could support questions such as “Which procedures need review when SEC-1234 changes?” or “Which task must come before writing a secret?”

Descriptive hyperlinks communicate connections to readers. To make the relationship types explicitly machine-readable, represent them in structured data that the application can interpret. The linked samples illustrate the connections; they do not implement a knowledge graph.

### Use relationships to support a specific experience

A content delivery system could use product version, role, and prerequisite relationships to select an applicable, approved procedure and its supporting information. Define that selection behavior and validate the result. An ontology alone does not assemble content or turn Markdown files into a database.

For search and AI, relationships can supply context across documents, such as which control governs a task and which service the task applies to. They can support [retrieval-augmented generation (RAG)](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview), which supplies retrieved source material to a language model. Semantic search can also work without an ontology: [embedding-based search](https://learn.microsoft.com/en-us/azure/search/vector-search-overview) can retrieve conceptually similar content.

For practical explanations, see [Stardog's guide to classes, relationships, and attributes](https://docs.stardog.com/getting-started-series/getting-started-4#understanding-the-schema) and [Microsoft Fabric's ontology core concepts](https://learn.microsoft.com/en-us/fabric/iq/ontology/overview#core-concepts). The Microsoft documentation describes a product feature in preview. For the formal standards behind vocabulary representation and ontology modeling, see W3C's [SKOS Reference](https://www.w3.org/TR/skos-reference/) and [OWL 2 Overview](https://www.w3.org/TR/owl2-overview/).

## Make metadata useful to the workflow

For a documentation collection, define required fields, allowed values, and what consumes them. This illustrative YAML record describes the Vault procedure; it is a proposed extension, not metadata currently attached to the sample.

```yaml
document_id: vault-secrets-management
content_type: procedure
audience:
  - service-owners
  - engineers
applies_to: secrets-vault
supports_control: SEC-1234
owner_role: secrets-platform-lead
example_workflow_status: awaiting-approval
```

Use consistent identifiers and resolve owner roles to maintained contacts. Require each procedure to identify its audience and applicable service. Validate that referenced controls exist.

A publishing or retrieval system must be configured to read these fields and act on them. For example, an operational system could exclude unapproved revisions from production answers. Putting a status label in a file does not enforce that rule.

## Keep ownership and approval connected to the content

Governance becomes useful when it answers practical questions: Who approves this change? Which other pages might be affected? What evidence shows that this version was reviewed?

The [revision register](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-controls-health-dashboard.md#revision-register) models those connections. Its pending approvals are fictional workflow states.

If storage eligibility changes, review the standard, the procedure's restrictions, and related guidance together. Record approval against the exact reviewed version, then track publication and the next review trigger.

A recent edit date alone does not establish accuracy. Documentation approval also does not prove that a service enforces the documented control.

## Check what people and AI actually receive

A page may work well when read from beginning to end but lose meaning when an AI retrieval system selects only part of it. Content preparation must preserve enough context to interpret a passage and trace it to its source.

A retrieved write command can be technically correct but inappropriate for the reader's secret type. Keep enough context with each retrievable passage to identify the service, conditions, and source. Configure retrieval to preserve applicable permissions and publication status. Evaluate both the retrieved material and the generated answer.

Use concrete checks:

- **Findability:** Can a reader locate storage eligibility without reading the entire standard?
- **Task completion:** Can someone identify prerequisites and follow the role-request procedure?
- **Answer accuracy:** Does an answer about external-customer secrets retain the restriction and cite the relevant requirement?
- **Incomplete evidence:** If the retrieved passage omits eligibility, does the system seek the missing source or acknowledge that it cannot determine suitability?
- **Change propagation:** After a rule changes, do affected pages and retrieved answers reflect the approved revision?

For people, trust grows from clear guidance and visible accountability. For AI applications, those same signals must become explicit source-selection and validation rules. Neither polished prose nor extensive metadata can replace checking the answer against the task.

