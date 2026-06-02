# Secret and Key Management with Vault

If you are looking for instructions on how to manage your keys and secrets using this fictitious Secrets
Vault, you are in the right place.
> [!IMPORTANT]
> If you have external customer secrets, please do NOT store them in the Secrets Vault
> (per standard #1234). Use the HSM service to store external-customer secrets in hardware
> security modules.
> 
The Secrets Vault is our customized version of HashiCorp Vault. We have features and
automation that are different from what you'll find on the HashiCorp Vault website.
Please use this documentation to onboard your service. If you get stuck or have
questions, contact #internal-support-channel on Slack.

## Quickstart
Get started with Secrets Vault

### Prerequisites:
- Service account credentials
- Network access to Vault endpoints
- Vault CLI installed (optional)

### Basic workflow:
1. Authenticate - Get a Vault token using your service credentials
2. Write a secret - Store your API keys or passwords
3. Read the secret - Retrieve secrets in your application

#### Example: Write and read a secret

```
# Authenticate (returns a token)
curl -X POST https://vault.example.com/v1/auth/kubernetes/login \
-d '{"role": "myapp-role", "jwt": "YOUR_K8S_TOKEN"}'

# Write a secret
curl -X POST https://vault.example.com/v1/secret/data/myapp/config \
-H "X-Vault-Token: s.1234abcd..." \
-d '{"data": {"api_key": "secret123", "db_password": "pass456"}}'

# Read the secret
curl -X GET https://vault.example.com/v1/secret/data/myapp/config \
-H "X-Vault-Token: s.1234abcd..."
```
