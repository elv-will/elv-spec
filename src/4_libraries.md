# Libraries
Libraries group keys together which may create/modify content within a specific context. 
They act as a way to separate the keys which manage tenant infrastructure (like KMSs) from keys which manage content.
The goal of the library is to have different levels of keys and different rules for different groups of content.
For example, one library could hold staging content which is not publicly accessible outside of library owners.
Content could be created there and then moved to a production library which has greater visibility.

This also provides a way for other parts of the blockchain if a given key has edit access to a library.

## Library Rights

Any user in a library has some associated rights,
The exact format of this structure is TDB (currently they’re bitflags), but should at the very least be able to answer the following questions:

* **IsAdmin($\kuser$)**: Is the user allowed to add other users as non-admins

* **CanEdit($\kuser$)**: Is the user allowed to edit content

## Library Blockchain Calls

* **CreateLibrary($\korigin, \tenantid, \libid, \name$)** 
  - Checks the origin has at least admin permission within $\tenantid$
  - Creates a library with $\libid$ and the given name within $\tenantid$
    * Note that for convenience, this call could also set $\rights$ such that **IsAdmin($\korigin$)** is true
* **SetRights($\korigin, \tenantid, \libid, \ktarget, \rights$)**
  - Check that the $\rights_\korigin$ of $\korigin$ have **IsAdmin**
  - Checks that $\adminrights{\rights}$
  - Sets the rights of $\\keyid{target}$ in $\\libid$ to $\\rights{target}$

* **TenantSetRights($\korigin, \\tenantid, \\libid, \\keyid{target}, \\rights{target}$)**
-   Check that $\korigin$ has $\\perm{admin}$ in $\\tenantid$
-   Sets the rights of $\\keyid{target}$ in $\\libid$ to $\\rights{target}$

This also provides a way for other parts of the blockchain if a given
key has edit access to a library.
