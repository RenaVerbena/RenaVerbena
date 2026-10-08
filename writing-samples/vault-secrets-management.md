# Secret and Key Management with Secrets Vault

If you are looking for instructions on how to manage your keys and secrets using this fictitious Secrets Vault, you are in the right place.

> [!IMPORTANT]
> Do not store external-customer secrets in Secrets Vault.
> Per [Standard 1234](security-standard-1234.md#storage-eligibility), HSM-required secrets must be stored in the HSM service,
> where key material is protected by hardware security modules.
>
> To determine which secret types belong in Secrets Vault versus HSM, see
> [secret storage eligibility requirements](security-standard-1234.md#storage-eligibility).

Use this documentation to onboard your service. If you get stuck or have questions, contact `#support-channel` on Slack.

---

## Table of Contents
1. [About Secrets Vault](#1-about-secrets-vault)
2. [Quickstart](#2-quickstart)
3. [Onboarding and Configuring Access](#3-onboarding-and-configuring-access)
4. [Writing Secrets to Vault](#4-writing-secrets-to-vault)
5. [Reading Secrets](#5-reading-secrets)
6. [Updating or Deleting Secret Paths](#6-updating-or-deleting-secret-paths)
7. [Updating or Removing Access](#7-updating-or-removing-access)
8. [Additional Resources](#8-additional-resources)

---

## 1. About Secrets Vault

Secrets Vault is our internal secrets management service built on HashiCorp Vault. It provides centralized storage and access control for:

* API keys and tokens
* Database credentials
* TLS certificates
* Encryption keys
* Application configuration secrets
* SSH keys
* Internal PKI / x.509 certificates
* OIDC / JWT tokens

**Key features:**
* Dynamic secret generation
* Automatic secret rotation
* Audit logging of all access
* Fine-grained access policies
* Multiple authentication methods supported
* High availability across regions

---

## 2. Quickstart

Get started with Secrets Vault.

### Prerequisites
* A Vault role for your service (see [Onboarding and Configuring Access](#3-onboarding-and-configuring-access))
* Network access to Vault endpoints
* Vault CLI installed (optional; the examples below use `curl` so they work anywhere)

### Basic Workflow
1. **Authenticate** - Get a Vault token using your service credentials.
2. **Write a secret** - Store your API keys or passwords.
3. **Read the secret** - Retrieve secrets in your application.

### Example: Write and read a secret

```bash
# Authenticate (returns a token)
curl -X POST https://vault.example.com/v1/auth/kubernetes/login \
  -d '{"role": "myapp-role", "jwt": "YOUR_K8S_TOKEN"}'

# Write a secret (payload must be JSON)
curl -X POST https://vault.example.com/v1/secret/data/myapp/config \
  -H "X-Vault-Token: s.1234abcd..." \
  -H "Content-Type: application/json" \
  -d '{"data": {"api_key": "secret123", "db_password": "pass456"}}'

# Read the secret
curl -X GET https://vault.example.com/v1/secret/data/myapp/config \
  -H "X-Vault-Token: s.1234abcd..."
```

---

## 3. Onboarding and Configuring Access

Each service gets its own Vault role and its own path under `secret/data/<service-name>/`. A role can only read and write within its own path.

### Request a role

1. Open a request in `#support-channel` using the **New service onboarding** template.
2. Provide:
   * Service name (lowercase, hyphenated; this becomes your secret path)
   * Kubernetes namespace and service account that will authenticate
   * Owning team and an on-call contact
   * Whether the service needs read-only or read/write access
3. The Vault team creates the role and policy and confirms the path in the request thread.

Onboarding requests are handled within one business day.

### Verify access

Once the role exists, confirm your service can authenticate:

```bash
curl -X POST https://vault.example.com/v1/auth/kubernetes/login \
  -d '{"role": "myapp-role", "jwt": "YOUR_K8S_TOKEN"}'
```

A successful response includes a `client_token`. Use that value in the `X-Vault-Token` header for every other request.

---

## 4. Writing Secrets to Vault

Write secrets as a JSON object under `data`. Each write creates a new version of the secret at that path; previous versions are retained.

```bash
curl -X POST https://vault.example.com/v1/secret/data/myapp/config \
  -H "X-Vault-Token: $VAULT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"data": {"api_key": "secret123", "db_password": "pass456"}}'
```

**Guidelines**

* Store one logical group of secrets per path (for example, `myapp/config` for app settings and `myapp/db` for database credentials).
* Never write secrets into logs, shell history, or CI output. Load values from a file or environment variable instead of typing them inline.
* Do not store external-customer secrets or HSM-required key material here. See the note at the top of this page.

---

## 5. Reading Secrets

```bash
curl -X GET https://vault.example.com/v1/secret/data/myapp/config \
  -H "X-Vault-Token: $VAULT_TOKEN"
```

The secret values are returned under `data.data`. To read a specific version, add `?version=<n>` to the URL.

**Guidelines**

* Read secrets at startup or on demand. Do not copy them into configuration files that get committed or baked into images.
* Every read is audit logged. Expect your team to be asked about unusual access patterns.

---

## 6. Updating or Deleting Secret Paths

**Update a secret.** Writing to an existing path creates a new version. Use the same `POST` request shown in [Writing Secrets to Vault](#4-writing-secrets-to-vault). Vault keeps prior versions, so an accidental overwrite can be recovered.

**Delete the latest version (soft delete).** The data is hidden but can be undeleted.

```bash
curl -X DELETE https://vault.example.com/v1/secret/data/myapp/config \
  -H "X-Vault-Token: $VAULT_TOKEN"
```

**Permanently destroy all versions.** Use this only when a secret has been rotated and the old values must not be recoverable.

```bash
curl -X DELETE https://vault.example.com/v1/secret/metadata/myapp/config \
  -H "X-Vault-Token: $VAULT_TOKEN"
```

> [!WARNING]
> Destroying metadata removes every version of the secret and cannot be undone. Confirm that no running service still depends on the path before you do this.

---

## 7. Updating or Removing Access

Access is managed through the role and policy the Vault team created for your service. To change it, open a request in `#support-channel` using the **Access change** template and specify:

* The service name and role
* What is changing (add or remove a path, change read-only to read/write, add or remove a Kubernetes service account)
* The reason for the change

Remove access when a service is decommissioned or when a team no longer owns it. Unused roles are flagged during quarterly access reviews and removed if the owning team does not respond.

---

## 8. Additional Resources

* [Secret storage eligibility requirements](security-standard-1234.md#storage-eligibility): which secret types belong in Secrets Vault versus the HSM service.
* [Standard 1234: Secret Storage and Handling](security-standard-1234.md): requirements for the example service.
* [HashiCorp Vault KV secrets engine, version 2](https://developer.hashicorp.com/vault/docs/secrets/kv/kv-v2)
* [HashiCorp Vault Kubernetes auth method](https://developer.hashicorp.com/vault/docs/auth/kubernetes)
* Support: `#support-channel` on Slack

---

## Document governance

This document supports [Control SEC-1234](security-standard-1234.md#control-sec-1234) and is governed by formal change control. Every revision must be approved by the [document owner](security-controls-health-dashboard.md#document-owners) and recorded in the [Security Controls Health Dashboard](security-controls-health-dashboard.md#revision-register) before publication. The dashboard maintains the revision and approval history for this documentation set.

*Control SEC-1234 and its governance records are fictional portfolio examples.*
