# ☁ CDN Service

The CDN service is offered by CDN nodes. A CDN node is a gateway from the Web to DDC. It can act as a web server and provide content to
regural HTTP clients, especially web browsers. Web apps can rely on CDN nodes to serve various
assets, or even be hosted completely on DDC.

CDN nodes address the complexity and slowness of decentralized storage. This is useful both
for server-based and for server-less apps. All access to stored data should go through a CDN node.

A CDN node is a stateless service which is capable of routing content to and from storage nodes,
as well as data replication, and caching.

**See the [☁ CDN API](/ddc/protocols/cdn-api.md) for details.**

## Implementation of CDN node

{% embed url="https://github.com/Cerebellum-Network/ddc-gateway-node" %}
