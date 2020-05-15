name: chapter-6
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 6      
## Vault Policies

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 6 introduces Vault Policies

---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-policies
# Vault Policies
* Vault Policies restrict the secrets users and applications have access to.
* Vault follows the practice of least privilege, *denying* access by default.
* Vault administrators must explicity grant users and applications access to specific paths with policy statements.
* In addition to specifying paths, policies also specify a set of capabilities for those paths.
* Policies are written in HashiCorp Configuration Language (HCL).

???
Vault Polices is the last chapter before our next lab.  
* Polices are really tying together authentication with the secrets engines
* User/App - Authenticate - Mapped to a policy that allows **capabilities** for defined **paths**
* Capabilities: Read, Write, List, Delete
---
name: vault-policy-example
# A Vault Policy Example
* Here is an example of a Vault policy:
```hcl
# Allow tokens to look up their own properties
path "auth/token/lookup-self" {
    capabilities = ["read"]
}
```
* Note that this policy does not allow tokens to change their own properties.
???
* This policy allows tokens to look up their own properties

---
name: vault-policy-paths-capabilities
# Policy Paths and Capabilities
* The path of a policy maps to a Vault API path.
* The most common capabilities granted are: `create`, `read`, `update`, `delete`, and `list` which correspond to HTTP verbs like POST and GET.
* Two other capabilities do not correspond to HTTP verbs:
  * `sudo` allows access to paths that are root-protected.
  * `deny` denies access to a path and takes precedence over other capabilities.

???
* Explain policy paths and capabilities

---
name: policies-for-lobs
# Configuring Policies for LOBs
* Many organizations organize Vault secrets by line of business (LOB) and department.
* Here's an example policy for line of business A, department 1:

```hcl
path "lob_a/dept_1/*" {
    capabilities = ["read", "list", "create", "delete", "update"]
}
```

* This policy grants all standard capabilities to all secrets mounted under `lob_a/dept_1/` by using the glob character (`*`).

???
* Vault is very path based.  Like a unix file system.  
* So when your designing your vault structure it often reflects your organization's structure
* This can be by line of business, department, team, or even partner or customer.
* Explain Example
* Note: the glob character can only be used at the end of a path.

---
name: vault-policy-commands
# Vault Policy CLI Commands
* Vault policies can be added to a Vault server using Vault's CLI, UI, or API.
* The command to add a policy with the CLI is `vault policy write`.
* Here is a command that creates a policy called "lob-A-dept-1" from the HCL file "lob-A-dept-1-policy.hcl":<br>
`vault policy write lob-A-dept-1 lob-A-dept-1-policy.hcl`
* Here is a command that associates this policy with a Userpass user:<br>
`vault write auth/userpass/users/joe/policies policies=lob-A-dept-1`

???
* Vault Policies can be added using the CLI, UI, or API
* **`vault policy write <policy_name> <file.hcl>`**
* Once the policy is created we need to associate it to a user or application
* In this case the Userpass user Joe.


---
name: lab-vault-basics-challenge-5
# 👩‍💻 Lab Challenge 4.1: KV v2 Secrets Engine
* In this Challenge, you'll enable and use the KV v2 secrets engine.
* Note that the path will be `kv` instead of `secret`.
* Instructions:
  * Click the "Use the KV V2 Secrets Engine" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* You're going to start where we left off at the "Use the KV V2 Secrets Engine" challenge
* You will enable an instance of the KV v2 secrets engine.
* This time the path will be `kv` instead of `secret` as was the case for the challenges with the Dev mode server.


---
name: lab-vault-basics-challenge-6
# 👩‍💻 Lab Challenge 5.1: Userpass Auth Method
* In this Challenge, you'll enable and use the Userpass auth method.
* Instructions:
  * Click the "Use the Userpass Auth Method" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* You will do the "Use the Userpass Auth Method" challenge next
* Here you will enable an instance of the Userpass auth method.
* You can see that Vault is "deny by default" since the Userpass user that they create will not have any access to secrets yet.

---
name: lab-vault-basics-challenge-7
# 👩‍💻 Lab Challenge 6.1: Vault Policies
* In this Challenge, you'll use Vault policies to grant different users access to different secrets.
* Instructions:
  * Click the "Use Vault Policies" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* In the last challenge of Lab 1, "Use Vault Policies", you going to associate policies to the users of the auth method you enabled.  
* These policies will define what secret engine and path the users have access too. 
* In the end you should be able to verify each user can only access their own secrets.

