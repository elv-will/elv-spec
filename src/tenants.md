# Tenants

A tenant is an owner and creator of content. They are responsible for providing a service available to providers' nodes which can manage keys and encrypt/decrypt content.


### Tenant Permissions
Tenant keys have the following permission levels, from most to least privileged

* **Root level** 
  - add/remove admins
* **Admin level** 
  - add/remove kmses, 
  - create libraries
  - add/remove any users to/from libraries
* **KMS level** 
  - can co-author content object versions with nodes

### Tenant Blockchain Calls
* **CreateTenant($\korigin, \spaceid, \tenantid$)** creates a tenancy
  - Checks governance to see whether origin can create a tenant
  - Creates $\tenantid$ and sets its space to $\spaceid$
  - Sets $\korigin$ as the creator of $\tenantid$
  - Sets $\korigin$ as a key for $\tenantid$ with root level 
  - Bonds some currency from $\korigin$ to the space under $\tenantid$
* **AddKMS($\korigin, \tenantid, \kmsid, \kkms, \locator$)** creates a kms
  - Checks that $\korigin$ has at least admin permission in $\tenantid$.
  - Creates a KMS $\kmsid$ within $\tenantid$ with locator $\locator$
  - Registers $\kkms$ to $\tenantid$ with KMS permission level 
* **RemoveKMS($\korigin, \tenantid, \kmsid$)** removes a node
  - Checks that $\korigin$ has at least admin permissions in $\tenantid$
  - Removes all $\kmsid$ information from the space and tenancy
  - Removes $\kkms$ from $\tenantid$
* **TODO: Remove Tenant, Top up billing balance**
