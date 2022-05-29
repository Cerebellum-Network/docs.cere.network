# ☁ CDN Nodes

CDN node is stateless node which responsible for routing content to storage nodes, replication, caching(**not implemented yet**). All access to stored data goes through the CDN node.

A CDN node is also a web gateway to DDC. It can act as a web server and provide content to
regural HTTP clients, especially web browsers. Web apps can rely on CDN nodes to serve various assets,
or even be hosted completely on DDC.


{% hint style="warning" %}
TODO

Explain that CDN nodes address the complexity and slowness of decentralized storage, both for server-based and server-less apps.
{% endhint %}

## Metrics

CDN node provides metrics for monitoring:

* general HTTP metrics
* number piece viewed

{% embed url="https://github.com/Cerebellum-Network/ddc-gateway-node" %}
