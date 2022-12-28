# 📖 Overview

## What is the DDC Network?

The Decentralized Data Cloud (DDC) protocol is built from the ground up for Web3 and is capable of powering a future of interoperable, privacy-preserving, and serverless data.

## Benefits of the DDC

The DDC enables you to build Web3 dApps, integrating real-world use cases on a Decentralized Data Cloud where the user owns the data. It provides multiple services on the protocol level and a toolset to quickly get started building dApps and serverless applications. The DDC offers flexible data management, secure, reliable and GDPR compliant data storage, fast data delivery, built-in file sharing, and serverless app hosting. 

## Network participants

Users are developers looking to store and retrieve data, or make DDC services available to their serverless dApps.

Providers provide data storage and compute resources to the DDC via their hardware in exchange for payments.

Buckets represent ownership and access rights to data or services.

Clusters are collections of provider nodes, which automatically coordinate resource use and payments. A Cluster is managed by a set of rules and by a number of Cluster Validators. Users connect to services by creating Buckets in Clusters.

Validators provide a range of services necessary to run the DDC. Validator Nodes are coordinated and rewarded through DDC smart contracts running on and protected by the Cere blockchain.

Users do not need to trust Providers to provide correctly and to keep their data safe and Providers do not need to trust Users to pay them. Instead, Users and Providers meet around a Cluster that defines and enforces the rules by which Providers are paid and Users operate. A Cluster is chosen by Users and Providers; this allows Users and Providers to come and go in and out of a Cluster and to be easily replaced by others.


## Services

There are multiple services in the DDC:

- Blockchain for payments and management.

- Storage for persistent and searchable data. [Details.](storage-nodes)

- CDN to access data and power apps. [Details.](cdn-nodes)

- Name service to give names to your objects.

- NFT registry to find assets associated with NFTs, such as image or video files.


## Scalability

The DDC is similar to blockchain and other peer-2-peer networks, in that it is trustless, persistent and decentralized. The key difference is that data and services on the DDC are partitioned with a small number of nodes assigned to each partition (unlike blockchains which replicate everything on every node across the network). 

Clusters can grow by adding nodes. Financial incentives attract Providers and encourage them to provide more hardware capacity.

The ecosystem can grow with new use-cases, new services, new levels of security, and new payment models by creating new Clusters.


## Security

The most critical rules that govern the relations between Users, Providers, and Validators, are executed on the Cere blockchain.

All data is replicated over a number of independent providers. Failing or malicious providers are detected and replaced.

The state and history of all nodes is also represented on-chain using cryptography to validate the amount and quality of service provided.

All data is identified by cryptographic identifiers and signatures that let both apps and validators verify the integrity and provenance of the data.

A system of data encryption and data sharing is provided to protect private data.


## Payments and resources

Resource management, pricing, and payments work per cluster.  Providers and Users agree on the terms when they join a Cluster.

Users can reserve resources (e.g., storage, bandwidth). Reserved resources are paid in regular intervals in CERE tokens. Payments are received by clusters and distributed to Providers according to their relative contributions of resources and quality of service. Some upfront payment is taken from Users and put into an escrow contract to guarantee revenues to Providers. Revenues are also locked away from Providers until proper service has been provided and validated.


## Markets

Markets are formed by multiple Providers and multiple Clusters offering similar services. Each Cluster can have a different pricing.

Users choose which Clusters they want to subscribe to based on, for example, their price, their service quality, or their reputation.


## Tools

The DDC offers a variety of tools to build applications. The DDC SDKs contain many functionalities including subscription management, upload and search of data, encryption and decryption, and validation of data. DDC services are designed for the web and can power applications with or without backend servers. Check the [Developer’s Guide](developer-guide) to get started with the SDKs.


{% hint style="warning" %}
[Source and diagrams](https://github.com/Cerebellum-Network/ddc-bucket-contract/blob/main/bucket/README.md)
{% endhint %}
