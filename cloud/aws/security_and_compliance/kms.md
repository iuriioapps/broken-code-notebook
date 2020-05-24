---
description: Key Management Service
---

# KMS

A managed service that enables you to easily encrypt your data. KMS provides a highly available key storage, management and auditing solution for you to encrypt data within your own applications and control the encryption of stored data across AWS services.

### Features

* KMS is integrated with CloudTrail, which provides you the ability to audit who used which keys, on which resources and when.
* Customer Master Keys \(CMKs\) are used to control access to data encryption keys that encrypt and decrypt your data.
* You can choose KMS to automatically rotate master keys created with KMS once per year without the need to re-encrypt your data that has already been encrypted with your master key.
* To help ensure that your keys and your data is highly available, KMS stores multiple copies of encrypted versions of your keys in systems that are designed for 11x9% durability.

### Concepts

* **Customer Master Keys \(CMKs\)** - you can use CMKs to encrypt and decrypt up to 4Kb of data. Typically, you use CMKs to generate, encrypt, and decrypt the data keys that you use outside of KMS to encrypt your data. Master keys are 256 bits in length.
* There are 3 types of CMKs:
  * **Customer-managed CMKs** are CMKs that you create, own and manage. You have full control over these CMKs, including establishing and maintaining their key policies, IAM policies and grants, enabling and disabling them, rotating their cryptographic material, adding tags, adding aliases that refer to the CMK, and scheduling the CMK deletion.
  * **AWS-managed CMKs** are CMKs in your account that are created, managed and used on your behalf by AWS services that integrate with KMS. You can view AWS-managed CMKs in your account, view their key policies, and audit their use in CloudTrail logs.However, you can not manage these CMKs, or change their permissions. And, you can not use AWS-managed CMKs in cryptographic operations directly, the service that creates them, uses them on your behalf.
  * **AWS-owned CMKs** are not in your account. They are part of collection of CMKs that AWS owns and manages for use in multiple AWS accounts. AWS services can use AWS-owned CMKs to protect your data. You can not view, manage or use AWS-owned CMKs or audit their use,

| Type | Can view | Can manage |
| :--- | :--- | :--- |
| Customer-managed CMK | Yes | Yes |
| AWS-managed CMK | Yes | No |
| AWS-owned CMK | No | No |

* **Data keys** - encryption keys that you can use to encrypt data, including large amounts of data and other other data encryption keys.
  * You can use CMKs to generate, encrypt and decrypt data keys. However, KMS does not store, manage or track your data keys, or perform cryptographic operations with data keys.
  * Data keys can be generated at 128 bit or 256 bit length and encrypted under a master key you define. 
* **Envelope encryption** - the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key. The top-level plaintext encryption key is known as master key.
* **Encryption context** - all KMS cryptographic operations accept an encryption context, an optional set of key-value pairs that can contain additional contextual information about the data.
* **Key policies** - when you create a CMK, permissions that determine who can use and manage that CMK are contained in a document called the key policy.
* **Grants** - a grant is an alternative to policy. You can use grants to give long-term access that allow AWS principals to use your CMKs.
* **Grant tokens** - when you create a grant, the permissions specified in the grant might not take effect immediately due to eventual consistency. If you need to mitigate the potential delay, use grant tokens instead.
* When you enable **automatic rotation** for a customer-managed CMK, KMS generates new cryptographic material so it can be used to decrypt data that is encrypted.
* An **alias** is an optional display name for a CMK. Each CMK can have multiple aliases, but each alias points to only one CMK. The alias name must be unique in AWS account and region.
* KMS supports key stores backed by AWS **CloudHSM** clusters. A key store is a secure location for storing cryptographic keys.
* You can connect directly to KMS through a private **VPC endpoint** in your VPC instead of connecting over the internet. When you use a VPC endpoint, communication between your VPC and KMS is conducted entirely within AWS network.

### Importing keys

* A CMK contains a key material used to encrypt and decrypt data. When you create a CMK, by default, AWS generates the key material for that CMK. But you can create a CMK without a key material and then import your own key material into that CMK.
* When you import key material, you can specify expiration date. When the key material expires, KMS deletes the key material and CMK becomes unusable. You can also delete key material on demand.

### Deleting keys

* Deleting the CMK also deletes the key material and all metadata associated with CMK and it's irreversible. You can no longer decrypt the data that was encrypted under that CMK, which means the data becomes unrecoverable.
* You can temporarily disable keys so they can not be used by anyone.

### Pricing

* Each CMK that you create in KMS, regardless whether you use it with KMS-generated key material or key material imported by you, costs you until you delete them.
* For a key material generated by KMS, if you opt-in to have CMK automatically rotated each year, each newly rotated version will raise the cost of the KMS per month.

### Limits

| Description | Limit |
| :--- | :--- |
| CMKs | 10000 |
| Aliases | 10000 |
| Grants per CMK | 10000 |

