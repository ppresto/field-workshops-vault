name: chapter-8
class: title, shelf, no-footer, fullbleed
background-image: url(https://hashicorp.github.io/field-workshops-assets/assets/bkgs/HashiCorp-Title-bkg.jpeg)
count: false

# Chapter 8    
## Encryption as a Service

![:scale 15%](https://hashicorp.github.io/field-workshops-assets/assets/logos/logo_vault.png)

???

* Chapter 8 introduces Vault's Encryption-as-a-Service (EaaS).
* This is our final chapter
* and we will set this up in the last lab

---
layout: true

.footer[
- Copyright © 2019 HashiCorp
- ![:scale 100%](https://hashicorp.github.io/field-workshops-assets/assets/logos/HashiCorp_Icon_Black.svg)
]

---
name: Vault-Transit-Engine

# Vault Transit Engine - Encryption as a Service
.center[![:scale 80%](images/vault-eaas.webp)]

* Vault's Transit Secrets Engine functions as an Encryption-as-a-Service.
* Developers use it to encrypt and decrypt data stored outside of Vault.

???
* What is the Transit secrets engine?
* It provides an encryption API that is secure , accessible , and easy to implement.
* Often times well see
  developers forced to learn and build cryptography into their apps and we find certificates embeded in the app or local to where its running.
  * These custom solutions take time , hard to maintain , and are rarely implemented securely
  * So, if attacker gains access to the system they also have the cert to decrypt the data
* With Vaults EaaS we present you with a familiar API that can be used to encrypt and decrypt data
* Vault policies can control who or what app can encrypt or decrypt the data.
* The key used to encrypt the data are stored in vault and not local to the application.

---
name: transit-engine-benefits
# Transit Engine Benefits

* Vault's Transit Engine provides developers a well-architected EaaS API so that they don't have to become encryption or cryptography experts.
* It provides centralized key management.
* It ensures that only approved ciphers and algorithms are used.
* It supports automated key rotation and re-wrapping.
* If an attacker manages to get access to the encrypted data, they will only see ciphertext that is useless without Vault.

???
There are seveal benefits of using the Transit engine.
* Encrypt/Decrypt data with a single request to Vaults Certified EaaS API
* Centralize all your key mgmt
* leverage a centralized audit log
* take advantage of automated key rotation & re-wrapping

---
name: Vault-Transit-Engine-1
# Vault Transit - Example Application

* In the next lab we'll use a web application that uses the Transit engine to encrypt and decrypt data.
* The app will store its encrypted data in the same MySQL database we used in Chapter 7.
* It will also get MySQL credentials from the Database secrets engine we configured in that chapter's lab.
* We'll first run the web app without Vault: No records are encrypted.
* We'll then run it with Vault enabled and see that new records are encrypted.

???
* In this lab we will be using a simple Python web app to see EaaS in action.
* It will connect the MySQL database we setup in the last lab and use the dynamic secrets engine we configured.

---
name: web-app-screenshot
# The Web App
### Here is a screenshot of the Python web app:

.center[![:scale 70%](images/transit_app.png)]

???
Notice the **Records View** and the **Database View**

---
name: web-app-views
# The Web App's Views
###There are two main sections in the application.
1. **Records View**
  * The Records View displays records in plain text, showing what a logged in user would see after any encrypted data is decrypted.

1. **Database View**
  * The Database View displays the raw records in the database, showing what SQL commands would return:

???
**Records View** : This is the application view showing what an authenticated user will see.  The application will decrypt any encrypted data with vault and display it in clear text here.

**Database View** : This is the Database View showing the raw records as the exist in the DB.  We can use this view to see whats being encrypted.

---
name: records-view
# The Web App's Records View
.center[![:scale 90%](images/records_view.png)]

* As we would expect an authorized user is able to see some of the sensitive data because the app has decrypted any encrypted data.

???
* Show the records view of the web app
* No ciphertext

---
name: Vault-Transit-Engine-6
# The Add User Screen
* In the lab, you will add new users to the database.
.center[![:scale 60%](images/add_user.png)]

???
* In the lab you will use the python app to add new records or users to the DB.


---
name: database-record-without-vault
# Record in Database View Without Vault Enabled
* After adding a record in the lab, you will be instructed to click on the  **Database View** menu.
* You should see the exact same data that you entered.
* This means that Personally Identifiable Data (PII) is being stored in plain text in our database records.
* How can we improve this? Let's enable Vault's Transit engine and see.

???
* Initially Vault will be disabled
* You will see all your PII data stored as clear text in the DB
---
name: encrypted-record
# A Database Record Encrypted by Vault
#### Here is a record that was encrypted by Vault's Transit engine.
.center[![:scale 80%](images/database_view_with_encrypted_record.png)]
* Note that the birth_date and social_security_number are encrypted.
???
* After we enable vaults transit engine we will add new records with our web app.
* This time the Database View will show the birth_date and ss# fields are encrypted
  * Indicated by the cyphertext that starts with vault:v1
* What do you think the v1 means?
  * It indicates the first version of the encryption key : v1

---
name: encryption-key-rotation
# Rotating Transit Engine Encryption Keys
* The encryption keys of Vaults Transit Engine can be rotated.
* The newest version of the key is used to encrypt new data
* Older versions of the key can still decrypt old data but cannot decrypt new data.
* When we rotate the encryption keys, apps that use the Transit engine are unaware of any changes.
* Data can also be re-encrypted using the `rewrap` endpoint.

???
* The encryption keys can easily be rotated without applications being impacted.
* Vault always knows which key to use to decrypt data, and new data will use the latest version
* Vault can use the latest key to rewrap data that was encrypted with an old key allowing you to remove old keys
 
---
name: lab-transit-challenge-1
# 👩‍💻 Challenge 8.1: Enable the Transit Engine
* In this lab challenge, you'll enable the Transit engine.
* You'll do this in the [Vault Encryption as a Service](https://instruqt.com/hashicorp/tracks/vault-encryption-as-a-service) Instruqt track.
* Instructions:
  * Click the "Enable the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
And this brings us to our final Lab:  "Vault Encryption as a Service" 
* We will Enable the Transit Engine
* path "lob_a/workshop/transit".

---
name: lab-database-challenge-2
# 👩‍💻 Challenge 8.2: Create an Encryption Key
* In this lab, you'll create an encryption key for use with the Transit engine you enabled in the previous challenge.
* Instructions:
  * Click the "Create a Key for the Transit Secrets Engine" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
* We'll create an Encryption Key

---
name: lab-database-challenge-3
# 👩‍💻 Challenge 8.3: Use the Web App Without Vault
* In this lab, you'll use the web application without Vault.
* Instructions:
  * Click the "Use the Web App Without Vault" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
We will get familiar with the web application without Vault.
* So new records will not be encrypted.

---
name: lab-database-challenge-4
# 👩‍💻 Challenge 8.4: Use the Web App With Vault
* In this lab, you'll use the web application with Vault.
* You'll also rotate the encryption key.
* Instructions:
  * Click the "Use the Web App With Vault" challenge of the "Vault Encryption as a Service" track.
  * Then click the green "Start" button.
  * Follow the challenge's instructions.
  * Click the green "Check" button when finished.

???
Then we'll use the web application with Vault enabled.
* And now new records will have sensitive fields encrypted by Vault's Transit engine.

---
name: chapter-8-review-questions
# 📝 Chapter 8 Review
* What is the main advantage of using Vault's Transit secrets engine?
* Where does Vault's Transit Engine store encrypted data?
* Was the application still able to decrypt older encrypted records after you rotated the encryption key?
* Is it possible to tell which version of an encryption key was used?

???
* Let's review what we learned in this chapter.

---
name: chapter-8-review-answers
# 📝 Chapter 8 Review
* What is the main advantage of using Vault's Transit secrets engine?
  * Developers can encrypt data without being experts in cryptography.
* Where does Vault's Transit Engine store encrypted data?
  * Wherever developers want, but outside of Vault
* Was the application still able to decrypt older encrypted records after you rotated the encryption key?
  * Yes
* Is it possible to tell which version of an encryption key was used?
  * Yes. The version is indicated by `v1`, `v2`, etc.

???
* Here are the answers to the review questions.

---
name: conclusion
# Thank You for Participating!
.center[![:scale 40%](images/vault_logo.png)]

### For more information please refer to the following links:
* https://www.vaultproject.io/docs/
* https://www.vaultproject.io/api/
* https://learn.hashicorp.com/vault

???
* Thank the students for their participation
* Share some Vault links

---
name: Feedback-Survey
# Workshop Feedback Survey
* Your feedback is important to us!
* The survey is short, we promise:
  * http://bit.ly/hashiworkshopfeedback

???
* Ask them to fill out the online survey
