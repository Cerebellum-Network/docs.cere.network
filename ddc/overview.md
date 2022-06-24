# 📖 Overview

## 

What are we going to learn after reading the Overview?
* What is the DDC Network?
* How is it structured?
* What are the benefits of using DDC?
* What are the actors in the network?
* What are clusters?
* Why do we have clusters?
* How do clusters connect users and providers?
* How are clusters managed?
* Is there any concensus?
* How does it scale?
* How do I pay for DDC?
* What are buckets?
* Which Tools can I use to get started?
* How is my data protected?

1. What is the DDC Network?
2. Benefits of DDC
3. Network participants
  3.1 Users
  3.2 Providers
  3.3 Clusters
4. Data storage
  Concepts: Buckets
  Large File support
  What type of data?
  How is it secured?
5. Data delivery
  * Replication
  * Accelelarate data delivery
6. Scalability
7. Payment
  Users
  Providers
8. Markets
9. Tools


## What is the DDC Network?

DDC is a Decentralised Data Cloud offering services to store, retrieve, accelerate and manage data to enable real world use cases.

## Benefits of DDC

DDC enables you to built real-world use cases on a Decentralized Data cloud where the User owns the data. It provides multiple services on the protocol level and a toolset to quickly get started building dApps and serverless applications. DDC offers flexible data management, secure, reliable and GDPR compliant data storing, fast data delivery, Built-in File Sharing and serverless App hosting. 

## Network participants

Users are developers who want to store and retrieve data.

Providers want to make money. They provide hardware.

Users do not trust providers to provide correctly and to keep their data safe. Providers do not trust users to pay them.
That is why users and providers meet around a "cluster". A cluster is chosen by users and providers; this is an easier choice than all users and providers connecting directly.
Users and providers can come and go in and out of a cluster and be replaced by others.
A cluster is managed by a set of rules and by a number of cluster validators.

## Services

There are multiple services in DDC.

Blockchain for payments and management.

Storage to persist and search data.
[Details.](storage-nodes.md)

CDN to access data and power apps.
[Details.](cdn-nodes.md)

Name service to give names to your objects.

NFT registry to find assets of NFTs.




{% hint style="warning" %}
TODO

Explain that there are users and providers, services and payments, all forming a market.
{% endhint %}

{% hint style="warning" %}
TODO

Write up the [contract concepts](https://github.com/Cerebellum-Network/ddc-bucket-contract/blob/dev/DOC.md), very simplified.

User ownership is abstracted by buckets.

The contract between users and providers is organized by clusters.
{% endhint %}

{% hint style="warning" %}
TODO

Explain the ownership of data (self-custody apps or managed apps).
{% endhint %}
