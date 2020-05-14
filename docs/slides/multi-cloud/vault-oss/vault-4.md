name: chapter-4
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 4      
## Vault Secrets Engines

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
Lab Overview
* Used Vault CLI, UI, and API to write and read secrets.  
* And you finished up by starting Vault in Production mode.

Chapter 4 introduces Vault secrets engines
* It focuses on the KV v2 engine.

---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-secrets-engines-1
# Vault Secrets Engines

.center[![:scale 65%](images/vault-secrets-engines.png)]
.center[Vault includes many different secrets engines.]

???
Anytime you access any secret from vault you're using a Secret Engine.  
* Some secrets engines simply store and read static secrets - like k/v
* Others connect to ext services and generate dynamic creds on demand (such as DB, Cloud Creds) 
* Other secrets engines provide encryption as a service, PKI, SSH, KMIP, Format-preserving-encryp, Masking, and much more.

---
name:vault-secrets-engines-2
# Important Vault Secrets Engines
* Key/Value (KV)
* PKI
* SSH
* TOTP
* Databases
* AWS, Azure, and Google
* Active Directory
* Transit

???
Spend some time pointing out what some of these do:
* KV - Used to manage generic, static secrets. KV v2 supports versioning.
* PKI - Used to generate dynamic X.509 certificates
* SSH - Take all the pain and drudgery out of securing your SSH infrastructure. Vault can provide key signing services that make securing SSH a snap.
* TOTP - The TOTP tool allows Vault to either act as a code-generating device for MFA logins or to provide TOTP server capabilities for MFA infrastructure.
* Databases - Generate dynamic, short-lived database credentials.
* Cloud credentials engines - Generate dynamic, short-lived cloud credentials for major clouds.
* Active Directory - Vault can rotate AD passwords.
* Transit - Implement's Vault's encryption-as-a-service. Provides an API that can handle all your encryption and decryption needs, based on policy, so that you don't have to manage a complicated key infrastructure.
* Custom Plugins

---
name: enabling-secrets-engines
# Enabling Secrets Engines

* Most Vault secrets engines need to be explicitly enabled.
* This is done with the `vault secrets enable` command.
* Each secrets engine has a default path.
* Alternate paths can be specified to enable multiple instances:<br> `vault secrets enable -path=aws-east aws`
* Custom paths must be specified in CLI commands and API calls:<br>
`vault write aws-east/config/root`<br>
instead of<br>
`vault write aws/config/root`

???

* When we initially deploy vault we need to enable the secrets engines we want to use
  `vault secrets enable <NAME_OF_ENGINE>`
* Default Path = `<NAME_OF_ENGINE>`

---
name: vault-kv-engine
# Vault's KV Secrets Engine
* Vault's KV secrets engine actually has 2 versions:
  * KV v1 (without versioning)
  * KV v2 (with versioning)
* In the second lab challenge, we used the instance of the KV v2 engine that is automatically enabled for "Dev" mode Vault servers.
* Vault does not enable any instances of the KV secrets engine for "Prod" mode servers.
* So, you'll need to enable it yourself.

???
* We used Vault's Key/Value (KV) engine in the second challenge.
  * Dev mode automatically enabled this engine for us
* But we'll need to mount it ourselves for the "Prod" mode server.

---
name: vault-kv-commands
# KV Secrets Engine Commands
* Use this command to mount an instance of the KV v2 secrets engine on the default path `kv`:<br>
`vault secrets enable -version=2 kv`
* The `vault kv` commands allow you to interact with KV engines.
  * `vault kv list` lists secrets at a specified path.
  * `vault kv put` writes a secret at a specified path.
  * `vault kv get` reads a secret at a specified path.
  * `vault kv delete` deletes a secret at a specified path.
* Other `vault kv` subcommands operate on versions of KV v2 secrets.

???

* To enable or **mount** the kv v2 engine we use the enable command
* `vault secrets enable -version=2 kv`
* The k/v engine has a command shortcut
  * vault kv [ list, put, get, delete ]

---
name: chapter-4-review-questions
# 📝 Chapter 4 Review

* What option is added to the `vault secrets enable` command to enable multiple instances?
* What is the difference between the two versions of the KV secrets engine?
* Can an old version of a KV v2 secret be retrieved?

???
* Let's review what we learned in this chapter.

---
name: chapter-4-review-answers
# 📝 Chapter 4 Review

* What option is added to the `vault secrets enable` command to enable multiple instances?
  * Add the `-path=<path>` option and use `<path>` with the CLI and API.
* What is the difference between the two versions of the KV secrets engine?
  * KV V2 supports versioning of secrets.
* Can an old version of a KV v2 secret be retrieved?
  * Yes. You will this using the Vault UI in the next challenge.

???
* Here are the answers to the review questions.
