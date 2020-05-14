name: Chapter-3
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 3      
## Running a Production Vault Server

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

Chapter 3 focuses on running a production Vault server

---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: vault-production-serves
# Running a Production Vault Server
* Running a Vault server in "Prod" mode involves multiple steps:
  * Specify configuration in a config file.
  * Start the server.
  * Initialize the server to get unseal keys and an initial root token.
  * Unseal the Vault server with the unseal keys.

???
* Running a production Vault server requires additional steps
* Provide configuration file when starting vault
* Vault will start in sealed mode
  * Requires unseal key/keys like shamir, cloud KMS, HSM(Ent)
  * Once unsealed it can decrypt the backend storage, load cfg, and be useable

---
name: configuring-vault
# Configuring Vault Servers
* Vault configuration files can be specified in [HCL](https://github.com/hashicorp/hcl) or JSON.
* Common configuration settings include:
  * listener
  * storage
  * seal
  * log_level
  * ui
  * api_addr
  * cluster_addr

???
Here are some common attributes you'll find in your vault servers configuration
* We only need to set a few of these and when we go through the lab we will configure them.

---
name: running-vault
# Starting a Production Vault Server
* You use the `vault server` command to start a Vault Production server.
* But, you do not use the `-dev` option.

???
We use the same vault go binary to start the vault server as we do for the CLI.

We are just going to pass it different options.

```
vault server -config=/vault/config/vault-config.hcl
```

---
name: initializing-vault
# Initializing Vault Clusters
* Recall that a Vault cluster runs multiple Vault servers.
* Each Vault cluster must be initialized once.
* This is done with the `vault operator init` command.
* The number of key shares and the key threshold can be specified with the `-key-shares` and `key-threshold` options.
* The command returns the unseal keys and the initial root token for the cluster.

???
To start vault you need the unseal key and a root token to do the initial configuraiton but initially we dont have any of this.
* First time you start a vault server (1 standalone or n cluster)
* initialize it with `vault operator init` 
* During init you will get your unseal keys and root key

---
name: unsealing-vault
# Unsealing Vault Servers
* Each Vault server must be unsealed each time it is started.
* You cannot use the server until you unseal it.
* This is done with the `vault operator unseal` command, using the unseal keys returned when you initialized the cluster.

???
Everytime a server starts
* its in a **sealed state** and must be **unsealed** before its usable
* This is true for every server in a cluster

---
name: vault-status-command
# Determining the Status of a Vault Server
* Use the `vault status` command to get the status of a Vault server.
* It will tell you if your Vault server is sealed or unsealed.
* It will also tell you the following:
  * the number of key shares and the key threshold
  * whether HA mode (clustering) is enabled
  * whether the server is running as a performance standby.

???
* **vault status** was one of the first commands I learned.  
* Not only does it tell you if the server is sealed.
* But it tells you right away if you can't connnect to it.  
* this usually means you need to set your env variables correctly.
`VAULT_ADDR=http://localhost:8200`

It also tells you useful information on
* Version,  HA Mode,  Key Shares / Threshold

---
name: chapter-3-review-questions
# 📝 Chapter 3 Review

* What is used to configure a "Prod" mode Vault server?
* What Vault command needs to be run once against a new Vault cluster?
* What Vault command has to be run each time a Vault server is started?

???
* Let's review what we learned in this chapter.

---
name: chapter-3-review-answers
# 📝 Chapter 3 Review

* What is used to configure a "Prod" mode Vault server?
  * A configuration file
* What Vault command needs to be run once against a new Vault cluster?
  * `vault operator init`
* What Vault command has to be run each time a Vault server is started?
  * `vault operator unseal`

???
* Here are the answers to the review questions.

---
name: getting-started-with-instruqt
# Doing Labs with Instruqt
* [Instruqt](https://instruqt.com/) is the platform used for HashiCorp workshops.
* Instruqt labs are run in "tracks" that are divided into "challenges".
* If you've never used Instruqt before, start with this [tutorial](https://play.instruqt.com/instruqt/tracks/getting-started-with-instruqt).
* Otherwise, you can skip to the next slide.

???
* We'll be using the Instruqt platform for labs in this workshop.
* Don't worry if you've never used it before: there is an easy tutorial that you can run through in 5-10 minutes.
---
name: lab-vault-basics-challenge-1
# 👩‍💻 Lab Challenge 2.1: The Vault CLI
* In this challenge, you'll run some of the Vault CLI commands.
* You'll do this in the first challenge, "The Vault CLI", of the "Vault Basics" Instruqt track using the URL:
https://instruqt.com/hashicorp/tracks/vault-basics.
* You'll continue to work through this Instruqt track in chapters 2-6.

???
* We'll be running the Instruqt track "Vault Basics" and covering the first 4 Challenges
* Starting with `The Vault CLI` Challenge

---
name: lab-vault-basics-challenge-2
# 👩‍💻 Lab Challenge 2.2: Run a Vault "Dev" Server
* In this challenge, you'll run your first Vault server in "Dev" mode.
* You'll also write your first secret to Vault and use the UI.
* Instructions:
  * Click the "Your First Secret" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* Then you'll do `Your First Secret` Challenge.
  * Start the Vault server in Dev mode
  * Login to the UI
  * Write a Secret in the k/v store 

---
name: lab-vault-basics-challenge-3
# 👩‍💻 Lab Challenge 2.3: Use the Vault HTTP API
* In this challenge, you'll use the Vault HTTP API.
* You'll first check the health of your Vault server.
* You'll then read your `my-first-secret` secret from Vault.
* Instructions:
  * Click the challenge called "The Vault API" in the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* The 3rd Challenge will be "The Vault API"
  * Check the Vault Server Health
  * Then read the Secret you created in the previous challenge

---
name: lab-vault-basics-challenge-4
# 👩‍💻 Lab Challenge 3.1: Run a Vault "Prod" Server
* In this challenge, you'll run your first Vault server in "Prod" mode.
* You'll learn how to initialize and unseal a Vault server.
* Instructions:
  * Click the "Run a Production Server" challenge of the "Vault Basics" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
Finally you will do the 4th Challenge: `"Run a Production Server"`
* You'll examine a Vault server configuration file
* Start the server
* initialize it and unseal it.

**WARNING:  save your unseal key and root token**  for future challenges
