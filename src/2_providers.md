# Providers

A provider is an entity which owns nodes within a space. 
It is responsible for ensuring its nodes abide by the space's rules, risking it's bond if it misbehaves.
Providers also have a permission structure which can associated levels of privilege with cryptographic keys.

### Provider Permission Levels
Provider keys have the following permission levels, from most to least privileged

* **Root level**
  - add/remove admins (effectively allows for admin key rotation)
* **Admin level**
  - add/remove nodes
  - bill tenants
* **Node level**
  - Co-author versions with tenants
  - Mark itself as no longer pending
  - Participate in part networking

Keys with a higher level may set the permission level of any keys strictly below it. 
For example, a key with root permission may give other keys admin level, but admin keys may not give other keys admin or change the permission level of a key at admin level.

### Provider Blockchain Calls
In addition to setting permissions on keys, we have the following calls

* **CreateProvider($\korigin, \spaceid, \provid$)** 
  - Check governance to see whether korigin can create a provider
  - Creates $\provid$ and sets its space to $\spaceid$
  - Sets $\korigin$ as the root key ($\kroot$) of $\provid$
  - Sets $\korigin$ as a key for $\provid$ with root level
  - Bonds some currency from $\kroot$ to the space for $\provid$
* **AddNode($\korigin, \provid, \nodeid, \knode, \locator$)**
  - Checks that $\korigin$ has at least admin level in $\provid$.
  - Creates a node $\nodeid$ with locator $\locator}$
  - Registers $\knode$ to $\provid$ with node permission level [^1]
  - Marks the node as pending while it syncs up parts with the rest of the space

* **ConfirmNode($\korigin, \provid, \nodeid$)]** marks a node as no longer pending
  - Checks that $\korigin$ has node permission or above for $\provid$
  - Sets $\nodeid$ to no longer pending
* **RemoveNode($\korigin, \provid, \nodeid$)** removes a node
  - Checks that $\korigin$ has at least admin permission in $\provid$
  - Removes all $\nodeid$ information from the space and provider
* **BillTenant** TODO

[^1] Should this error if the key already exists within the permissions scheme?
