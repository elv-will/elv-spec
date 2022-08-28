
# Definitions

Here are some definitions of entities within the system

| Name                   | Description                                                                                                                                   |
| ---                    | ---                                                                                                                                           |
| Node                   | A server which stores and serves parts.                                                                                                       |
| Provider               | An individual or organization which owns, secures, and operates nodes.                                                                        |
| Tenant                 | An individual or organization which owns content.                                                                                             |
| Content                | A versioned set of data which is owned by a tenant.                                                                                           |
| Space                  | A group of providers and tenants, where providers agree to run nodes that serve content owned by a tenant according to a common set of rules. |
| Part                   | A part is a sequence of bytes stored in the space, referenced by its hash.                                                                    |
| Content Object Version | A collection of parts created by a tenant, referenced by its hash.                                                                            |
| Content Object         | A collection of versions.                                                                                                                     |
| KMS                    | A tenant-owned server which holds keys for encrypting/decrypting content which the tenant stores in the space.                                |


The following entities are identified as follows:

| Entity                 | Identifier        | Substrate Type          |
| :---:                  | ---               | ---                     |
| Provider               | `ProviderId`      | 10-byte array           |
| Node                   | `NodeId`          | 10-byte array[^1]       |
| Space                  | `SpaceId`         | 10-byte array           |
| Content Object         | `ContentObjectId` | 10-byte array           |
| Content Object Version | `CObjVersionId`   | 32-byte array           |
| KMS                    | `KMSId`           | 10-byte array           |

[^1]: Could node id just be an unsigned 32-bit integer and do some round robin or mod-magic for assigning partitions?
