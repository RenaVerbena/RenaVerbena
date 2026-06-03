# Secret and Key Management with Vault

If you are looking for instructions on how to manage your keys and secrets using this fictitious Secrets Vault, you are in the right place.

> [!IMPORTANT]
> Do not store external-customer secrets in the Secrets Vault.
> Per standard #1234, HSM-required secrets must be stored in the HSM service,
> where key material is protected by hardware security modules.
>
> To determine which secret types belong in Secrets Vault versus HSM, see
> [Secret Storage Guidance](link).

The Secrets Vault is our customized version of HashiCorp Vault. We have features and automation that are different from what you'll find on the HashiCorp Vault website.

Please use this documentation to onboard your service. If you get stuck or have questions, contact `#support-channel` on Slack.

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
* SSH Keys
* Internal PKI / x.509 Certificates
* OIDC / JWT Tokens

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
* Service account credentials
* Network access to Vault endpoints
* Vault CLI installed (optional)

### Basic Workflow
1.  **Authenticate** - Get a Vault token using your service credentials.
2.  **Write a secret** - Store your API keys or passwords.
3.  **Read the secret** - Retrieve secrets in your application.

### Example: Write and read a secret

```bash
# Authenticate (returns a token)
curl -X POST [https://vault.example.com/v1/auth/kubernetes/login](https://vault.example.com/v1/auth/kubernetes/login) \
  -d '{"role": "myapp-role", "jwt": "YOUR_K8S_TOKEN"}'

# Write a secret (Payload must be JSON)
curl -X POST [https://vault.example.com/v1/secret/data/myapp/config](https://vault.example.com/v1/secret/data/myapp/config) \
  -H "X-Vault-Token: s.1234abcd..." \
  -H "Content-Type: application/json" \
  -d '{"data": {"api_key": "secret123", "db_password": "pass456"}}'

# Read the secret
curl -X GET [https://vault.example.com/v1/secret/data/myapp/config](https://vault.example.com/v1/secret/data/myapp/config) \
  -H "X-Vault-Token: s.1234abcd..."
```
