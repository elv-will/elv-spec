# Tenants

A tenant is an owner and creator of content. They are responsible for providing a service available to providers' nodes which can manage keys and encrypt/decrypt content.


### Tenant Permissions
Tenant keys have the following permission levels, from most to least privileged

* **Root level** 
  - add/remove admins
  - participate in space governance
* **Admin level** 
  - add/remove kmses
  - add funds for billing
* **KMS level** 
  - create/remove content keys
* **Content level** 
  - co-author content object versions with nodes

### Tenant Blockchain Storage
```rust
(TenantId) -> {
  space: SpaceId,
  root: AccountId,
}
(TenantId, KMSId) -> {
  locator: BoundedString,
}
```

### Tenant Blockchain Calls
* **`CreateTenant(origin: Origin, space: SpaceId, tenant: TenantId)`**
  - Checks `space` governance to see whether `origin` can create a tenant
  - Creates `tenant` 
  ```rust
  {
    space: space,
    root: origin,
  }
  ```
  - Registers `origin` with `tenant` with `ADMIN` level 
  - Bonds some currency from `origin` to the `space` under `tenant`
* **`AddKMS(origin: Origin, tenant: TenantId, kms: KMSId, locator: BoundedString)`** 
  - Checks that `origin` has `ADMIN` permission for `tenant`
  - Creates a KMS at `(tenant, kms)`
  ```rust
    {
      locator: locator,
    }
  ```
* **`RemoveKMS(origin: Origin, tenant: TenantId, kms: KMSId)`** removes a node
  - Checks that `origin` has at least `ADMIN` permissions in `tenant`
  - Removes `(tenant, kms)`
* **TODO: Remove Tenant, Top up billing balance**
