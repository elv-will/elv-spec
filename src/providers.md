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

### Provider Blockchain Storage
```rust
(ProviderId) -> {
  space: SpaceId,
  root: AccountId,
}
(ProviderId, NodeId) -> {
  pending: true,
  locator: BoundedString,
}
```

### Provider Blockchain Calls
In addition to setting permissions on keys, we have the following calls

* **`CreateProvider(origin: Origin, space: SpaceId, prv: ProviderId)`** 
  - Checks `space` governance to see whether `origin` can create a provider
  - Creates a provider at `prv` 
    ```rust
    {
      space: space,
      root: origin,
    }
    ```
  - Bonds some currency from `root` to `space` under `prv`
* **`CreateNode(origin: Origin, prv: ProviderId, node: NodeId, locator: BoundedString)`**
  - Checks that `origin` has at least `ADMIN` level in `prv`
  - Creates a node (note that it's marked as pending while it syncs up with other nodes)
    ```rust
    {
      pending: true,
      locator: locator,
    }
    ```
  - Registers `origin` to `prv` with `NODE` permission level [^1]

* **`ConfirmNode(origin: Origin, prv: ProviderId, node: NodeId)`** marks a node as no longer pending
  - Checks that `origin` has `NODE` permission or above in `prv`
  - Sets `pending=false` at  `(prv, node)`
* **`RemoveNode(origin: Origin, prv: ProviderId, node: NodeId)`** removes a node
  - Checks that `origin` has at least admin permission in `prv`
  - Deletes the node at `(prv, node)`
* **BillTenant** TODO

[^1] Should this error if the key already exists within the permissions scheme?
