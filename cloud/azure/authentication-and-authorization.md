# Authentication & Authorization

### Identity & Access  Management (IAM)

#### IAM Components

"Who" - **Azure Active Directory (AD)** - Manages Azure identities. Azure AD is a cloud-based identity service

* One per tenant
* Provides identity - "who you are?"
* Identity = "**security principal**" (technical term)
* Manage end users (people) or applications
* Email format (end user) - name@domain.com

"Can do what" - **Azure Role-Based Access Control (RBAC)** - Provides fine-grained access management to Azure resources. Controls access using **roles**:

* Assign roles to a security principal
* **Roles** are collections of specific **permissions**
* There are **general** role and **specific** role types:
  * Owner - general role type, full access to all resources in scope
  * Virtual Machine Contributor - only access to manage VMs

"On which resources" - **Scope** - Controls the scope of access in the resource hierarchy. A scope defines a set of resources allowed to access:

* Roles granted to various layers of the resource hierarchy
* Lower levels inherit roles from the higher levels
  * Centralized management







