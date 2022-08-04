# 📖 Overview

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

- Blockchain for payments and management.

- Storage to persist and search data.
[Details.](storage-nodes)

- CDN to access data and power apps.
[Details.](cdn-nodes)

- Name service to give names to your objects.

- NFT registry to find assets of NFTs.


The operations of service nodes are coordinated and rewarded through DDC smart contracts running on the Cere blockchain.

User connect to services by creating buckets in clusters. A bucket represents the ownership and access rights to data or services.


## Scalability

DDC services aim for goals similar to blockchains, that is trustlessness, persistence, decentralization. The key difference is that most data and services are partitioned with a small number of nodes assigned to each partition (unlike blockchains which replicate everything on every node). 

Clusters can grow by adding nodes. Financial incentives attract providers and encourage them to provide more hardware capacity.

The ecosystem can grow with new use-cases, new services, new levels of safety, new payment models, by creating new clusters.


## Security

The most critical rules that govern the relations between users, providers, and validators, are executed on the Cere blockchain.

All data is replicated over a number of independent providers. Failing or malicious providers are detected and replaced.

The state and history of all nodes is also represented on-chain using cryptography to validate the amount and quality of service provided.

All data is identified by cryptographic identifiers and signatures that let both apps and validators verify the integrity and provenance of the data.

A system of data encryption and data sharing is provided to protect private data.


## Payments and resources

Resource management, pricing, and payments work per cluster. 
Providers and users agree on the terms when they join a cluster.

Users can reserve resources (e.g., storage, bandwidth). Reserved resources are paid in regular intervals in CERE tokens. Payments are received by clusters and distributed to providers according to their relative contributions of resources and quality of service. Some upfront payment is taken in custody to guarantee revenues of providers. However, revenues are also locked away from providers until proper service has been provided and validated.


## Markets

Markets are formed by multiple providers and multiple clusters offering similar services. Each cluster can have a different pricing.

Users choose which clusters they want to subscribe to based on, for example, their price, their service quality or their reputation.


## Tools

DDC offers a variety of tools to build applications. The DDC SDKs contain many functionalities including subscription management, upload and search of data, encryption and decryption and validation of data. DDC services are designed for the web and can power applications with or without backend servers. Follow the [developer’s guide](developer-guide) to get started with the SDKs.


{% hint style="warning" %}
[Source and diagrams](https://github.com/Cerebellum-Network/ddc-bucket-contract/blob/main/bucket/README.md)
{% endhint %}
