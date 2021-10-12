# Wallet Tutorial

In this tutorial, you'll learn to develop a wallet for the [Solana](https://solana.com/) protocol. As you build, we'll dive into some of the fundamentals of Web3 so you can start cultivating an understanding for blockchain technology and decentralized applications (dApps). 

## Step 1: Up and Running

### Prerequisites
There are no formal blockchain or Web3 prerequisites for this tutorial, but ideally you have some experience with [TypeScript](https://www.typescriptlang.org/) and [React](https://reactjs.org/). Feel free to give it a shot even if you don't since you'll likely get a lot out of it anyway. Just be extra patient because it may be more difficult to navigate how the app works while you build the interactions with Solana.

### System Dependencies
Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Node](https://nodejs.org/en/) and [yarn](https://yarnpkg.com/getting-started/install) installed.

### Setting Up
You'll be building Solana interactions on top of a Next.js app that we have pre-built for you. This way you can skip the development phases you're familiar with (e.g. writing React components and Next pages) and dive right into the blockchain-specific phases.

First, clone [the repo](https://github.com) in your system and install the app dependencies by running the following three commands in your terminal:

***Listing 1.1
```
git clone --> add repo link here
cd wallet-tutorial
yarn
```

Then, run `yarn dev` in your terminal to start the development server on port 3000. The terminal should print output similar to that in [Listing 1.2]() prompting you that the app is available to be viewed at [http://localhost:3000](http://localhost:3000).

***Listing 1.2
```
yarn run v1.22.11
$ next dev
ready - started server on 0.0.0.0:3000, url: http://localhost:3000
info  - Using webpack 5. Reason: Enabled by default https://nextjs.org/docs/messages/webpack5
event - compiled successfully
```

### App Preview
By the time you're done with this tutorial, you'll have a functional wallet capable of changing the state of the Solana blockchain by receiving and transferring on-chain funds. In [Step 2](), we'll create a wallet by generating a special phrase unique to that wallet.

***Insert GIF of wallet creation

Then we'll build a function to fetch the balance of our newly created wallet in [Step 3]() before leverage functionality to issue "test tokens" through something called an "airdrop" that we'll explore in [Step 4](). We'll build the ability to transfer funds to another wallet in [Step 5]().

***Insert GIF of wallet dashboard, airdrop and transfer

Finally, we'll use the phrase we generated in [Step 2]() to sign into an account and recover access to its funds and functionality in [Step 6]().

***Insert GIF of wallet recovery

Although building a full-blown, non-custodial wallet like Phantom or MetaMask is beyond the scope of this tutorial, by the end of it you'll have developed enough judgment to better allocate your learning time as you explore new projects. We'll end the tutorial with a list of additional resources that you'll be well prepared to expand into.

## Step 2: Creating a Wallet
Crypto wallets are one of the critical pieces of user-facing blockchain infrastructure. In the same way that the browser brought the web to a mass audience by providing an easy-to-use gateway into the underlying servers and databases, wallets provide users with an easy way to interact with a blockchain protocol.

A wallet is less like the wallet you use for you credit cards, and a lot more like a key chain. If you think of a blockchain as a giant room of digital storage lockers, wallets are the key chains where you keep the keys to your locker. And once you have access, you can manage the contents of that locker by receiving, sending, and spending the digital assets it contains.

## Step 3: Fetching a Balance

## Step 4: Airdropping Funds

## Step 5: Transferring Funds

## Step 6: Recovering an Account

## Conclusion
