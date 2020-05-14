name: chapter-1
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 1  
## HashiCorp Vault Overview

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???
What is Vault?

---
name: hashiCorp-vault-overview
# HashiCorp Vault Overview
![:scale 10%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

  * HashiCorp Vault is an API-driven, cloud agnostic secrets management system.
  * It allows you to safely store and manage sensitive data in hybrid cloud environments.
  * You can also use Vault to generate dynamic short-lived credentials, or encrypt application data on the fly.

???
Vault is
* A centralized secrets management solution you can run in any private or public cloud
* Create / Store Secrets -  and Manage the lifecycle of a secret (rotate, version, revoke, renew)
* Generate dynamic short-lived credentials
* Encrypt application data on the fly with a single command or API call
* 
* https://www.vaultproject.io/docs/
* https://www.vaultproject.io/api/
* https://learn.hashicorp.com/vault/

---
name: the-old-way
layout: false
# The Traditional Security Model
.center[![:scale 70%](images/bodiam_castle.jpg)]
.center[Also known as the "Castle and Moat" method.]

???
Here's a Castle surrounded by a Moat
* We trust everyone within the castle walls because they protect us
* Visitors have to go over the draw bridge and through 1 main gate to get in
* Safe visitors are identified by their name or address

---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: traditional-security-models
# The Traditional Security Model
* Traditional security models were built upon the idea of perimeter based security.
* There would be a firewall, and inside that firewall it was assumed one was safe.
* Resources such as databases were mostly static.  As such rules were based upon IP address, credentials were baked into source code or kept in a static file on disk.

???
* This is an example of a Perimter based security model 
  * clearly defined network boundries
  * A firewall to secure all inbound/outbound traffic
  * If inside it was assumed one was safe
  * Identity is based on the static host or IP address
  * Credentials often found in source control, and static files

---
name: problems-with-traditional-security-models
# Problems with the Traditional Security Model
* IP Address based rules
* Hardcoded credentials with problems such as:
  * Shared service accounts for apps and users
  * Difficult to rotate, decommission, and determine who has access
  * Revoking compromised credentials could break

???
* Identifying resources by IP Address doesn't work well in the cloud
  * Cloud resource have dynamicaly generated IP's and often times they're short lived
  * Using this Perimeter based security model has short falls
    * Target - HVAC
    * Atlantic City Casino - fishcam, firmware, int network
* Assuming your safe behind the perimeter / castle walls
  * One often Hard Codes Credentials which leads to many problems
    * Rotating or Revoking credentials is a breaking change (involves planning)
    * Shared Services Accounts leak over time and its near impossible to determine who has access
---
name: the-new-way
layout: false
# Modern Secrets Management
.center[![:scale 65%](images/nomadic_houses.jpg)]
.center[No well defined perimeter; security enforced by identity.]

???
* These are Mongolian Yurts or "Ger" as they are called locally. There is no castle with fortified walls to protect them.  They move from place to place, bringing their houses with them.
* And if you don't think the Nomadic way can be an effective security posture, think about this for a moment. The Mongol military tactics enabled  Genghis Khan to conquer nearly all of continental Asia, the Middle East and parts of eastern Europe. Mongol warriors would typically bring 3-4 horses with them, to move up to 100 miles a day, which was unheard of in the 13th century. They were faster, more adaptable, and more resilient than all their enemies.
* Services in the cloud move from place to place or host to host.  Maybe even across networks.  They aren't staying in one place, running on 1 IP that we can build fortified walls around.  so we need to adapt and find more resilient method to identify our services



---
name: identity-based-security-1
#Identity Based Security
.center[![:scale 75%](images/identity-triangle.png)]
.center[[Identity Based Security and Low Trust Networks](https://www.hashicorp.com/identity-based-security-and-low-trust-networks)
]

???
Vault is like a broker in the middle of a transaction 
1. Goal:  Identify who you are [ like Hotel Check-in Desk ]
   * It has Many Auth Methods
   * Ideally you want to leverage your existing Identity Providers
2. Vault can manage many types of secrets and excels at generating short-lived, dynmamic secrets.
3. Vault's ACL policies are associated with tokens that users and applications use to access secrets after authenticating.


---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: identity-based-security-2
# Identity Based Security

Vault was designed to address the security needs of modern applications.  It differs from the traditional approach by using:

* Identity based rules allowing security to stretch across network perimeters
* Dynamic, short lived credentials that are rotated frequently
* Individual accounts to maintain provenance (tie action back to entity)
* Credentials and Entities that can easily be invalidated

???
* Now that we are using identity based rules instead of IP's we can stretch across network perimeters.  We aren't limited by location.

* Now we are using a Token that identifies every human or service (auditable).
  * It has a TTL (expire, revoke)
* This token gives us a layer of abstraction allows us to:
  * rotate backend secrets
  * enables dynamic short lived credentials
  * securely support modern applications

---
name: secrets-engines
layout: false
# Vault Secrets Engines
.center[![:scale 60%](images/vault-engines.png)]
.center[[Vault Secrets Engines](https://www.vaultproject.io/docs/secrets/)]

???
* Vault provides many out-of-the-box secrets engines.
* Additional custom secrets engines can be added by customers.


---
name: vault-reference-architecture-1
# Vault Architecture Internals
.center[![:scale 75%](images/vault_arch.png)]
.center[[HashiCorp Vault Internals Architecture](https://www.vaultproject.io/docs/internals/architecture/)
]

???
* There is a clear seperation of components that are inside or outside the security barrier.
* HTTPS API   ,  Storage Backend (persist encrypted secrets)
* Barrier contains Audit devices, auth methods, secret engines, & policies

When init started vault is sealed.  Needs unseal key/s.  Once unsealed...
  *  Can decrypt storage backend data
  *  Can load audit dev, auth, engines, and polices

---
name: vault-reference-architecture-2
# Vault Architecture - High Availability
.center[![:scale 60%](images/vault-ref-arch-lb.png)]
.center[[Vault High Availability](https://www.vaultproject.io/docs/concepts/ha/)
]

???
Most teams running vault in a production environment have a design similar to this.
* 3 node vault cluster (1 active, 2 standby).  Yes HA, No Scalability
* HA Backend like consul
* New in 1.4 Integrated storage
  
* Click on the link to learn more about Vault's high availability in a single cluster.

---
name: vault-reference-architecture-3
# Vault Architecture - Multi-Region
.center[![:scale 70%](images/vault-ref-arch-replication.png)]
.center[[Vault Enterprise Replication](https://www.vaultproject.io/docs/enterprise/replication/)
]

???
As Services depend more and more on vault you may need to address things like DR and Multi Region Clusters which are both Enterprise features

* DR is pretty self explanitory - Take for example the Vault cluster here in California
* As you grow to multiple data centers
  * Active / Active - Performance Replication
* Click the link to learn more about Vault's replication.

---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: chapter-1-review-question
# 📝 Chapter 1 Review

* What is HashiCorp Vault?

???
* Let's review what we learned in this chapter.
---
name: chapter-1-review-answer
# 📝 Chapter 1 Review
* What is HashiCorp Vault?
  * Vault is a Secrets Management System.
  * It is API-driven and cloud agnostic.
  * It can be used in untrusted networks.
  * It can authenticate users and applications against many systems.
  * It supports dynamic generation of short-lived secrets.
  * It runs in highly available clusters that can be replicated across regions.

???
* Here are the answers to the review questions.
