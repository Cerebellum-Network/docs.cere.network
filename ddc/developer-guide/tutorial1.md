# Uploading and downloading files to DDC

In this tutorial, you'll learn how to use a React frontend to interact with the smart contract on the Cere blockchain that coordinates the exchange of storage space for $CERE tokens between users and providers. 

Like other online storage services, users that wish to store and access their data in real-time must pay the entity that does so. In the case of Cere DDC, this is no different. 

In the case of centralized services, funds are used to manage large server farms that are connected to the internet and allow access to your content at all times. 

However, this process is dependent on human input and the perpetuity of your content is ultimately subject to the decisions of a centralized set of individuals.

If we wish to decentralize this process, we must remove all human input from the transaction. This is where the smart contract comes in. In short, the smart contract creates an financial incentive for multiple providers to store and make your data available, while ensuring that even if one provider chooses to stop hosting your content, it will remain available via other providers. To learn more about this smart contract, see section [insert section here]

- What is the mechanism by which a new provider is given a piece of data when one provider stops giving access to it?

- Idea: Because the data is encryptped and providers cannot interpret the data that they are hosting, it becomes impossible for providers to coordinate to take down a piece of data. 


Insert more theory here. 

Interaction with the smart contract requires the Cere software development kit (SDK). Written in both Javascript and Kotlin, this set of tools allow users to upload content to DDC in just a few lines of code, without needing to worry about the details of interacting with our smart contract.

Note: Unlike centralized solutions, the process of storing data with DDC requires coding by design. App developers will use our SDK to build their front-end, and it is up to them to abstract the code behind their own user interfaces. In other words, we provide the SDK for app developers to seamlessly offer a decentralized storage solution to the data stored on their apps. We do so by designing a publicly accessible system that ensures a economic incentive between users and providers at all times. 