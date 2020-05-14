name: vault-title-slide
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Vault OSS Workshop
## Modern Security with Vault for any Cloud

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
Welcome to the Vault OSS Workshop

Usually one of the first questions I get at these workshops is

Can I have a copy of the slides?
---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: Link-to-Slide-Deck
# The Slide Deck
<br><br>
### Follow along on your own computer at this link:

https://hashicorp.github.io/field-workshops-vault/slides/multi-cloud/vault-oss/index.html

???
Here is a link to the slides.  

Follow along on your computer or bookmark for reference

---
name: Introductions
# Introductions

* Patrick Presto
* Sr. Solutions Engineer

???
Previously I worked at a fortune 500 as a **Director of the global infrastructure** team.<br>
**Supported 23 private DC** and given a direction to moving workloads to the 3 major public clouds<br>
took a **life and shift** approach using our onprem architecture, os images, monitoring solutions, leveraged existing processes, etc...<br>
It was Painful, we had to redo all of it, and **we learned a lot!** <br>
One of our **Biggest take aways** was how unprepared we were **to address todays security requirements** for running our services in the cloud<br>
We took a lot of liberties with our more traditional security model and were forced to mature quickly<br>
We looked at a couple enterprise tools, We chose 1.  Long story short.  I now work for the company.

---
name: Table-of-Contents
# Table of Contents

1. HashiCorp Vault Overview
1. Interacting with Vault
1. Running a Production Server
1. Vault Secrets Engines
1. Vault Authentication Methods
1. Vault Policies
1. Dynamic Database Secrets
1. Encryption as a Service

???
* We will start out with an overview of Hashicorp Vault
* Vault Basics
* Different ways to Authenticate
* Store and Get Secrets
* Find out what Dynamic Secrets Are?
* In our final lab we will setup Encryption as a Service

---
name: instruqt-tracks
# Lab Environment Used
* This workshop uses [Instruqt](https://instruqt.com) for hands-on labs.
* Instruqt labs are run in "tracks" that are divided into "challenges".
* This workshop uses the following tracks:
    1. https://instruqt.com/hashicorp/tracks/vault-basics
    1. https://instruqt.com/hashicorp/tracks/vault-dynamic-database-credentials
    1. https://instruqt.com/hashicorp/tracks/vault-encryption-as-a-service
