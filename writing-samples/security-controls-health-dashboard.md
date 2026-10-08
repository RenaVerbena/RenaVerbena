# Security Controls Health Dashboard

This example dashboard connects a security control to its documentation, accountable owners, and revision records. Use it to find who can approve a change and whether the affected documents are ready for publication.

> **Published portfolio example.** The control, owner roles, and revision records are fictional. Pending approvals and release dates describe the example workflow; these portfolio pages are published.

For actual changes to this portfolio, see the [GitHub revision history](https://github.com/RenaVerbena/RenaVerbena/commits/main/).

## Control overview

| Field | Current record |
| --- | --- |
| Portfolio status | Published example |
| Control | [SEC-1234: Protected storage and handling of service secrets](security-standard-1234.md#control-sec-1234) |
| Standard | [Standard 1234: Secret Storage and Handling](security-standard-1234.md) |
| Control owner | Security Governance Lead (example role) |
| Documentation coverage | One standard, one set of handling guidelines, and one service procedure |
| Revisions in the example workflow | 3 |
| Approvals in the example workflow | 0 |
| Example workflow status | Awaiting approval |
| Control implementation and effectiveness | Not assessed by this documentation example |

## Document owners

Owners below are illustrative roles. In an operational register, each role would resolve to a named accountable person or maintained team contact.

| Document | Document owner | Relationship to the control | Next review |
| --- | --- | --- | --- |
| [Standard 1234](security-standard-1234.md) | Security Governance Lead | Defines control requirements and storage eligibility. | Set at approval; quarterly thereafter. |
| [Secure Data Handling](secure-data-handling.md) | Security Documentation Lead | Explains handling principles; its secret-handling sections support SEC-1234. | Set at approval; quarterly thereafter. |
| [Secret and Key Management with Secrets Vault](vault-secrets-management.md) | Secrets Platform Lead | Provides onboarding, access, storage, and deletion instructions for the example service. | Set at approval; quarterly thereafter. |

## Revision register

The records below model revisions awaiting approval in the fictional workflow. Maintain a separate entry for each revision and retain earlier entries when new revisions are added so that the change and approval history remains visible.

| Record | Document | Proposed change | Example workflow status |
| --- | --- | --- | --- |
| REV-001 | [Standard 1234](security-standard-1234.md) | Introduce the fictional standard and shared governance note. | Awaiting approval |
| REV-002 | [Secure Data Handling](secure-data-handling.md) | Add the shared governance note. Existing guidance remains unchanged. | Awaiting approval |
| REV-003 | [Secrets Vault](vault-secrets-management.md) | Connect existing standard and storage references to the new standard; add the shared governance note. Existing procedures remain unchanged. | Awaiting approval |

### Example approval and publication records

The version field identifies the exact reviewed document, such as an immutable Git commit and file path. The approval evidence must identify that version. A document changed after approval must be reviewed again before publication.

| Record | Reviewed version | Approval evidence | Release date in example |
| --- | --- | --- | --- |
| REV-001 | Not yet reviewed | Pending in example | Not scheduled |
| REV-002 | Not yet reviewed | Pending in example | Not scheduled |
| REV-003 | Not yet reviewed | Pending in example | Not scheduled |

## Recording a revision

1. Add a record identifying the affected document and proposed change.
2. Ask the document owner to review the exact revision and its effects on related guidance. Obtain control-owner approval if control requirements change.
3. Record the reviewed version, approver, approval date, and evidence reference before publication. Confirm that links resolve and related guidance remains consistent.
4. Publish the approved revision, then record its publication date and next review date. Preserve the previous record.

Draft or rejected revisions remain identifiable by their status. If approval is withdrawn or a published revision is superseded, record that decision and the replacement reference. Do not mark a document current merely because it was edited recently.

This first version is maintained manually. An operational implementation would protect approval records from unauthorized changes and link to the underlying version history and approval evidence.

## Reading the health indicators

- **Ownership:** Every controlled document has an accountable owner.
- **Approval:** The published revision matches the revision that was approved.
- **Freshness:** A review is due when its scheduled date arrives or a relevant control or service change occurs.
- **Consistency:** The standard, guidance, and procedure express compatible requirements and use working references.

These indicators describe documentation health. Testing whether a service actually enforces SEC-1234 requires separate implementation evidence.
