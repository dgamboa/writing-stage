# Step 1: Template and Configuration

Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Node](https://nodejs.org/en/) and [yarn](https://yarnpkg.com/getting-started/install) installed.

## Setting Up

You'll be building the blog application on top of a [Next.js](https://nextjs.org/) template that we have pre-built. This way you can quickly scaffold your way into developing the Web 3 dApp with minimal configuration.

**This will change depending on whether we use the template**
>>>>>>>>>>>>>>> Start

First, clone [the repo]() in your system and install the app dependencies by running the following three commands in your terminal:

##### _Listing 1.1: Set up commands to run on your terminal_
```text
$ git clone [_]
$ cd mirror-tutorial
$ yarn
```
>>>>>>>>>>>>>>> End

Next, you'll need to copy the example environment file `.env.local.example` to set the environment variables we'll need to write and deploy the smart contract. 

```text
$ cp .env.local.example .env.local
```

Navigate to `./.env.local` and set the four environment variables based on the instructions included in the file.

```text
ETHERSCAN_API_KEY=Etherscan API Key. Go to https://info.etherscan.com/api-keys/ to learn how to create an api key.
MAINNET_NODE_URL=https://matic-mainnet.chainstacklabs.com
TESTNET_NODE_URL=https://matic-mumbai.chainstacklabs.com
PRIVATE_KEY=Your private key exported from Metamask. Go to https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key to learn how to export your private key.
```

{% sidenote title="Box 1.1: What is MetaMask?" %}
[MetaMask](https://metamask.io/) is a crypto wallet built as a browser extension that allows you to interact with Ethereum and Ethereum-compatible blockchains as well as dApps built on top of them.

> **Warning**: The **private key** environment variable is required to deploy the smart contract in [Step X](). Despite the fact that the `.env.local` file has been placed in `.gitignore`, we recommend you create a wallet whose sole purpose is its use in tutorials and local development. That way, you can mitigate the risk of exposing your actual wallet's private keys.

With the environment variables in place, we're ready to compile and run the dApp.

```text
$ yarn web3:compile
```

Then, run `yarn dev` in your terminal to start the development server on port 3000. The terminal should print output similar to that in [Listing 1.2](#listing-12-terminal-message-on-app-start-up) informing you that the app is available to be viewed at [http://localhost:3000](http://localhost:3000).

##### _Listing 1.2: Terminal message on app start up_
```text
yarn run v1.22.11
$ next dev
ready - started server on 0.0.0.0:3000, url: http://localhost:3000
info  - Using webpack 5. Reason: Enabled by default https://nextjs.org/docs/messages/webpack5
event - compiled successfully
```

## Reviewing the Template

Before we dive into the code, we should understand what the template comes with. Along with the standard folder structure and dependencies found in most NextJS applications, we have added two Web 3-specific libraries that we'll leverage throughout the tutorial - [ethers.js](https://docs.ethers.io/) and [Hardhat](https://hardhat.org/).

Ethers is a JavaScript library designed to interact with Ethereum and Ethereum-compatible blockchains. We'll first encounter ethers in [Step 2]() to interface with [MetaMask]() and connect our wallet.

Hardhat is a development environment for Ethereum and Ethereum-compatible blockchains. It allows you to write and compile Solidity locally, run tests, and deploy smart contracts, all while fully supporting TypeScript. We'll leverage Hardhat in [Step X]() to manage development of the smart contract used to mint and transfer NFTs related to blog posts.

>>>>>>>>> Start
Should we describe the non-standard directories like web3, context, fetchers, etc here? My sense is that it's probably helpful to provide context on the template before we use it in subsequent steps.
>>>>>>>>> End
