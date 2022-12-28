# NFT-commerce SDK

Integrating the NFT-commerce SDK in your webshop enables you to offer your customers curated experiences through NFTs, such as exclusive content, event tickets, or memberships. Through the SDK, potential customers can decrypt and consume content that is stored on the Cere DDC right on your website through engagement templates that are created with Cere RXB (Real time experience builder) and populated by NFTs that were minted with Cere Freeport. This NFT experience on your website can be further extended and seamlessly integrated with the Cere whitelabel NFT marketplace to enable tradeability and create a collector base.

### What are the prerequisites to integrate the SDK?

- Payment for the NFT is conducted via your normal webshop checkout
- An API Key for secure communication with the Cere API is needed
- Having UserIDs mapped to your customers

### What does the typical event flow look like?

1. Your customer puts the NFT, like any item from your webshop, in the cart/basket
2. The customer pays on the checkout page
3. After successful payment, your server sends a request to check the availability of NFTs
4. If NFTs are available, your server sends another request to Cere API with parameters to allocate NFT
5. Cere API checks if the user exists based on provided UserID
6. If the customer does not exist, a custodial wallet is created
7. If a wallet is created/exists, the NFT is allocated respectively
8. Once the allocation is finished, Cere API notifies your server with a status parameter

### How are the engagment templates generated and maintained?

The engagement templates integrated through the SDK are curated by the Cere team through Cere RXB (Real time experience builder) at the moment. A new engagement template based on your customers' needs can be made available within a couple of hours. In the future RXB will be available as a self-service tool.

### Where do the NFTs come from?

NFTs integrated through the SDK are minted with Cere Freeport. During the minting Freeport securely stores the content on the Cere DDC and associates it with a freshly created ERC-1155 token on the Polygon blockchain. Based on their NFTid they will populate the engagement template.

### How to integrate the CERE NFT-commerce SDK?

As with any other SDK your frontend application has to be extended with just a few lines of code. Following the documentation below you are integrating a demo RXB engagement template on your website and will allocate test NFT tokens to potential buyers.

## [📓 Web](web-sdk.md)
## [📗 Android](android-sdk.md)
## [📙 IOS](ios-sdk.md)
## [📘 React Native](react-native-sdk.md)
