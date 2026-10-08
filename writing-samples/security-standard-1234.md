# Standard 1234: Secret Storage and Handling

This standard defines the requirements for storing, accessing, and maintaining service secrets. Use it to determine which storage service is appropriate and what safeguards must be in place before a service handles secrets.

> **Published portfolio example.** The control, services, owner roles, and governance records are fictional. The approval status below is part of the example workflow.

| Document field | Value |
| --- | --- |
| Document ID | STD-1234 |
| Control ID | SEC-1234 |
| Document owner | Security Governance Lead (example role) |
| Audience | Service owners, engineers, and security reviewers |
| Portfolio status | Published example |
| Example workflow status | Awaiting approval |
| Effective date in example | Set after fictional approval |
| Review cycle | Quarterly after approval, and when control requirements or supported services change |

## Control SEC-1234

**Control objective:** Protect service secrets from unauthorized disclosure and use throughout their lifecycle by requiring approved storage, restricted access, traceable changes, and accountable ownership.

This standard states the requirements. The [Secure Data Handling guidelines](secure-data-handling.md#data-storage) explain the broader handling principles. The [Secrets Vault onboarding and usage procedure](vault-secrets-management.md#2-quickstart) describes how to apply relevant requirements in the example service.

The mapping to SEC-1234 covers the secret-handling portions of those documents. It does not assign every topic in the broader data handling guidelines to this control.

## Scope

These requirements apply to operational credentials and private key material used by services, including API tokens, database credentials, and private keys. They apply when secrets are created, stored, retrieved, replaced, or retired. User-password verification stores are outside this example's scope.

**Service owner** means the team accountable for the application and its secrets. **Document owner** means the role accountable for the accuracy and approval of a controlled document. The dashboard identifies the [document owners](security-controls-health-dashboard.md#document-owners).

## Storage eligibility

Determine the secret's purpose and restrictions before selecting a storage service.

| Secret category | Required handling in this example |
| --- | --- |
| Internal service secrets eligible for software-based storage | Store in the approved Secrets Vault service, using the service's assigned role and path. |
| Private key material designated as requiring a hardware security module (HSM) | Use the approved HSM service. Do not copy the protected key material into Secrets Vault. |
| External-customer secrets | Do not store in this internal Secrets Vault. Obtain the approved destination and handling requirements from the control owner before onboarding. |
| Purpose or storage eligibility is unclear | Obtain a classification and storage decision from the control owner before uploading the secret. |

These eligibility rules are fictional organizational requirements, not product limitations of HashiCorp Vault.

## Requirements

| ID | Requirement |
| --- | --- |
| SEC-1234.1 | The service owner must identify the secret category and use the destination specified in [Storage eligibility](#storage-eligibility). Secrets must not be stored in source code, shared documents, or unapproved configuration files. |
| SEC-1234.2 | Each onboarded service must have an owning team and contact. Access must be limited to the identities, paths, and operations needed by that service. Use the [role request procedure](vault-secrets-management.md#request-a-role) to establish access. |
| SEC-1234.3 | Secret values and authentication tokens must not appear in logs, support requests, screenshots, or shell history. Use clearly fictitious values in documentation examples. |
| SEC-1234.4 | The storage service must record access and administrative changes in protected audit records without disclosing secret values. The responsible team must be able to identify the acting identity, target, action, time, and outcome. |
| SEC-1234.5 | The service owner must maintain an appropriate credential rotation or expiration schedule and act on suspected exposure. Replacing a stored value must be coordinated with the system that issues or accepts the credential. |
| SEC-1234.6 | Access must be reviewed quarterly and removed when no longer needed. Before permanently destroying stored versions, the service owner must confirm that dependent services no longer require them. Follow the [access removal procedure](vault-secrets-management.md#7-updating-or-removing-access) and [deletion guidance](vault-secrets-management.md#6-updating-or-deleting-secret-paths). |

The Vault example uses versioned key/value storage. Its [versioning and deletion behavior](https://developer.hashicorp.com/vault/docs/secrets/kv/kv-v2) and [path-based access policies](https://developer.hashicorp.com/vault/docs/concepts/policies) are documented by HashiCorp. This standard supplies the fictional organization's requirements around those capabilities.

## Exceptions and changes

An exception requires a documented reason, affected service, risk assessment, compensating safeguards, accountable owner, and expiration date. The control owner must approve the exception before the service relies on it. An exception does not silently change the standard or its supporting instructions.

For a documentation revision, the document owner must review accuracy and assess effects on related documents. A change to a control requirement also requires the control owner's approval. Record the affected document, exact version, change summary, approval evidence, and publication date in the [revision register](security-controls-health-dashboard.md#revision-register). Review related guidance before publication so that requirements and instructions remain consistent.

Record evidence references, not secret values. A documentation approval establishes approval of that revision; it does not establish that every service has implemented the control.

---

## Document governance

This document supports [Control SEC-1234](security-standard-1234.md#control-sec-1234) and is governed by formal change control. Every revision must be approved by the [document owner](security-controls-health-dashboard.md#document-owners) and recorded in the [Security Controls Health Dashboard](security-controls-health-dashboard.md#revision-register) before publication. The dashboard maintains the revision and approval history for this documentation set.

*Control SEC-1234 and its governance records are fictional portfolio examples.*
