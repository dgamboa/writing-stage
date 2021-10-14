# Wallet Tutorial

In this tutorial, you'll learn to develop a wallet for the [Solana](https://solana.com/) protocol. As you build, we'll dive into some of the fundamentals of Web3 so you can start cultivating an understanding for blockchain technology and decentralized applications (dApps). 

## Step 1: Up and Running

### Prerequisites
There are no formal blockchain or Web3 prerequisites for this tutorial, but ideally you have some experience with [TypeScript](https://www.typescriptlang.org/) and [React](https://reactjs.org/). You should also know what [Solana](https://solana.com/) is at a very high level (i.e. a public blockchain protocol capable of running smart contracts with significant scalability advantages), even if that means nothing more than having read a blog post or listened to a podcast about it.

Feel free to give it a shot even if you don't meet the above prerequisites since you'll likely get a lot out of it anyway. Just be extra patient because it may be more difficult to navigate the pre-built app and the Solana functionality we'll be developing.

### System Dependencies
Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Node](https://nodejs.org/en/) and [yarn](https://yarnpkg.com/getting-started/install) installed.

### Setting Up
You'll be building Solana interactions on top of a Next.js app that we have pre-built for you. This way you can skip the development phases you're familiar with (e.g. writing React components and Next pages) and dive right into the blockchain-specific phases.

First, clone [the repo](https://github.com) in your system and install the app dependencies by running the following three commands in your terminal:

***Listing 1.1
```
$ git clone --> add repo link here
$ cd wallet-tutorial
$ yarn
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

Anyone with access to these keys has access to the locker, which is why it's important for users to keep them private - hence their name, private keys. The locker or account is called a public key. And as opposed to physical lockers, but similar to mailing letters or sending dollars to a bank account, anyone with the public key can send digital assets to that account. That's why public keys are also referred to as public addresses. 

We'll be building a type of wallet called a Hierarchical Deterministic (HD) wallet. We don't need to dive into the full definition of HD wallets here, but it's important to know that they enable the ability to store the private key as a 12-, 18-, or 24-word phrase referred to as a secret recovery phrase or mnemonic phrase. You'll be using a JavaScript library called Bip39 to facilitate the generation of this phrase, which in turn can be converted into a private key to create a wallet.

<!-- Potential for an aside box here on a brief history of wallets to drive home the point that most wallets today are HD wallets and conform to BIP39 -->

### Challenge
If you haven't already, open the app in your browser at [http://localhost:3000](http://localhost:3000). You'll notice the frontend offers the user the ability to create a new wallet. Clicking on `Create New Wallet` routes the user to a generate page signaling to our user that by clicking `Generate`, the app will generate a key phrase (i.e. create a wallet). But when we click `Generate`, nothing shows up in the phrase container.

For the app to work, we need to implement a function that generates a phrase and uses it to create a wallet when this page renders. Navigate to `pages/phrase.tsx` in your editor and follow the steps included as comments to finish writing the function. We include a description along with a link to the documentation you need to review in order to implement each line. The relevant code block is also included in [Listing 1.3]() below.

***Listing 2.1
```
--> code here
```

### Implementation
In order to generate the phrase, we need to leverage an external library that satisfies the BIP39 specification. Fortunately, there's [Bip39](https://github.com/bitcoinjs/bip39), which provides us with the functionality we need to generate the phrase and later convert it into the seed we need to generate our Solana wallet keys.

We have already installed the library when we ran `yarn` during set up because we included it into the `package.json` included in the pre-built app. So all that's left is to import it:

```javascript
import * as Bip39 from "bip39";
```

Recall that secret recovery phrases are also called mnemonics and Bip39 includes a method for generating mnemonic phrases, `generateMnemonic()`. We can call it and replace the variable currently assigned to an empty string:

```javascript
const generatedMnemonic = Bip39.generateMnemonic();
```

This allows us to set the phrase in state and display it for our wallet user to store safely. In fact, with this we have already created our wallet or account because the phrase by itself will allow its holder to access the account that matches it. You could say that blockchain accounts aren't created. Because these accounts are mathematical addresses in a system where the number of accounts is established by the its architecture, it's more accurate to think of the mnemonic as obtaining the key to access a pre-existing account.

<!-- Consider aside on why phrases are also called mnemonics -->

But we need one more step to establish a connection to the account. We need to convert the phrase into a form that the blockchain can understand. Mnemonic phrases after all are abstractions that translate a long, archaic number into a more human-friendly form.

We need to convert the phrase into bytes so the Solana web3.js library can use it to generate an account. An account is referred to as a keypair. That is, a public key that can encrypt data and a private key that can decrypt data.

<!-- Potential aside on keypairs -->

By reviewing Solana's web3.js documentation, we can see that there is a Keypair class defined as "an account keypair used for signing transactions." This is exactly what we need to generate using the mnemonic phrase.

We notice that the Keypair class has a method, `fromSeed` that generates a keypair from a 32-byte seed. It also mentions that the seed needs to be a `Uint8Array`, which means we'll need a way to convert our `string` phrase into a `Uint8Array`.

Going back to the Bip39 library we see a method called `mnemonicToSeedSync(mnemonic)` that returns some sort of `Buffer` object that looks like a list of hexadecimal numbers. We can test this by adding running it in our application and passing in the mnemonic we generated:

```javascript
const seed = Bip39.mnemonicToSeedSync(generatedMnemonic)
console.log(seed)

// console:
> Uint8Array(64)
```

It looks like we're close. The Keypair class requires a 32-byte `Uint8Array` and we're getting a 64-byte `Uint8Array`. We can slice the seed and keep only the first 32 bytes:

```javascript
const seed = Bip39.mnemonicToSeedSync(generatedMnemonic).slice(0, 32)
console.log(seed)

// console:
> Uint8Array(32)
```

With the seed in that format, we can use Keypair's `fromSeed` method to generate an account keypair:

```javascript
const newAccount = Keypair.fromSeed(seed);
```

We then set the account into the context state manager and we have access to a Solana wallet. If you click on the `Finish` button, you'll be routed to the wallet page that displays the account's dashboard.

You'll notice this page includes a few other features. We'll discuss the Network dropdown in [Step 3]() while implementing the crucial functionality of showing users their balance in the next step. In [Step 4]() we'll dive into airdrops and make the Airdrop button functional, and in [Step 5]() we'll enable the Send button and transfer funds.

***Listing 2.2: Code for Creating a Wallet
```javascript
const generatedMnemonic = Bip39.generateMnemonic();
setMnemonic(generatedMnemonic);

const seed = Bip39.mnemonicToSeedSync(generatedMnemonic).slice(0, 32)
const newAccount = Keypair.fromSeed(seed);
setAccount(newAccount);
```

## Step 3: Fetching a Balance
Crypto wallets serve one key function. By storing your private keys, they allow you to manage - transfer, receive, organize - your digital assets. Part of that function requires the wallet to retrieve certain data that is stored on the blockchain to display on your user dashboard. Balances are one of the most important pieces of information users view on their wallet dashboards.

A balance represents a certain amount of cryptocurrency or tokens held by a specific account. If you think of the blockchain as a database that keeps ownership records, and of the public keys as the owner IDs, then you can think of the balances as an integer column in the database that tracks how much of a certain token each owner ID holds.

We'll be connecting to one of Solana's networks, and fetching the balance for the account we just created. Later in [Step 4](), we'll be funding the account with test tokens using the airdrop function.

The concept of various networks for a single protocol is similar to that of different environments for an app (e.g. development, test, production, etc). Typically blockchain protocols have a main network or mainnet, which refers to the production blockchain with real economic value and official transactions, and at least one experimentation network, which refers to an identical blockchain used to test features before they go live on mainnet.

Solana has a production network called mainnet and two exploration networks called testnet and devnet. Solana's devnet is designed for developers and users to play with various features and debug dApps before launching on mainnet with real economic consequences. The testnet is where the Solana protocol tests new releases and potential protocol updates.

You'll notice the wallet includes a dropdown at the top-right that allows users to select what network they want to connect to. This allows the wallet to manage assets specific to the connected network. Our default network is devnet since we'll be using it to leverage test tokens in [Step 4]() and then transfer them in [Step 5](), but the functionality we'll build will work for any of the Solana networks.

### Challenge
In the last step, we discussed how a wallet is more like a keychain that holds keypairs representing an account address and the key to access it. We built a function that allowed us to generate a unique account and the corresponding phrase that works like a password for accessing the account. Now we need to connect with the Solana blockchain so we can fetch the account's balance, which at this point should be zero since we just created it.

If you open the browser's console from the `/wallet` page, you'll notice a message that reads, "Balance functionality not implemented yet!". Navigate to `utils/index.ts` in your editor and follow the steps included as comments to finish writing the `refreshBalance` function. We include a description along with a link to the documentation you need to review in order to implement each line. The relevant code block is also included in [Listing 3.1]() below. 

***Listing 3.1
```
--> code here
```

### Implementation
The first step for interacting with any blockchain is to instantiate a connection. Conveniently there's a Connection class in web3.js designed to do just that. By reviewing the documentation, we notice the Connection constructor requires two argument: `endpoint: string` and `commitmentOrConfig?: Commitment | ConnectionConfig`.

The description for `endpoint` mentions that it's a "URL to the fullnode JSON RPC endpoint". Since we don't have a URL for the connection, we need to either find one from Solana or look for a function that will return a URL. By searching the web3.js docs for "URL", we see that there's a function called `clusterApiUrl` that returns the "RPC API URL for the specified cluster". Moreover, if we review the Cluster type, we see that it refers to the network we want to connect to.

As for `commitmentOrConfig`, it looks like the definition for `Commitment` types describes it as the "level of commitment desired when querying state", which is a meaningless definition to us at this point. However it looks like `Commitment` can be one of several strings so we should choose one and test the function. In this case, we can choose "confirmed" as a reasonable guess and move forward.

<!-- Consider aside on technical sophistication and give shout out to Hartl -->

Putting the above together, we can build our connection instance:

```javascript
const connection = new Connection(clusterApiUrl(network), "confirmed");
console.log(connection)

// console:
> Connection {_commitment: 'confirmed', _confirmTransactionInitialTimeout: undefined, _rpcEndpoint: 'https://api.devnet.solana.com', _rpcWsEndpoint: 'wss://api.devnet.solana.com/', _rpcClient: ClientBrowser, …}
```

Now that we have a connection, we need to fetch our account's balance. We might speculate that there should be a `getBalance` function that takes an account's public address as a parameter, and returns the account's balance. Searching for the keyword "balance" in the web3.js docs, we can see that, not only is there a `getBalance` method, but it's a method of the Connection class.

Reviewing the `getBalance` method on Connection, we can see it expects the account's public key as a parameter.

```javascript
const publicKey = account.publicKey;
const balance = await connection.getBalance(publicKey);
console.log(balance)
```

There are a few ways to structure this part of the function. We've chosen to assign a variable, `publicKey`, to the account's public key, and then pass that into `getBalance` to query the network for the balance. From the docs, we know `getBalance` returns a promise so we use `await` and assign its return value to the `balance` variable. By logging `balance`, we can see our account has zero balance as expected.

But that's a bit anticlimactic. We know it's a new account and it should have zero balance. Nothing changed on the frontend because the default value was zero. We need to fund the account to see the balance change, and we'll do just that in the next step.

***Listing 3.2: Code for Fetching a Balance
```javascript
const refreshBalance = async (network: Cluster, account: Keypair | null) => {
  if (!account) return;

  try {
    const connection = new Connection(clusterApiUrl(network), "confirmed");
    const publicKey = account.publicKey;
    const balance = await connection.getBalance(publicKey);;
    return balance
  } catch (error) {
    console.log(error);
    return 0;
  }
};
```

## Step 4: Airdropping Funds
We've now generated a wallet and connected it to the Solana blockchain. In the previous step, we confirmed that our account's balance is zero but wouldn't it be nice to fund our account so we can see the balance change? You might be wondering whether we'll now need to transfer real money so we can test the account, but as we discussed in [Step 3](), a blockchain's devnet typically provides a way to test transactions without risking real economic value.

In this step, we'll be building functionality to allow users to "airdrop" SOL tokens into their devnet account. In the crypto world, an airdrop is a way for the protocol to distribute tokens to account holders for free. In this case, we'll be tapping into the native devnet airdrop functionality built into Solana to fund our account. This is in contrast to mainnet airdrops performed by blockchain protocols and crypto projects, which are usually issued to reward early adopters or contributors.

<!-- Consider an aside on famous project airdrops and mention it "pays" to be an early adopter in crypto -->

On the right of the wallet dashboard, you'll find a button with the label Airdrop. You might notice that this button goes away if you change the network. This is because the functionality we're implementing is only active for devnet. Moreover, you'll notice a tooltip help icon informing users that they'll receive 1 devnet SOL by clicking on Airdrop.

Once we've completed it this step, our balance will automatically increase when we click Airdrop. This will position us well for [Step 5]() where we'll build functionality to send funds to other Solana accounts.

### Challenge
If you open the browser's console from the `/wallet` page, and click on the Airdrop button, you'll notice a message that reads, "Airdrop functionality not implemented yet!". Navigate to `utils/index.ts` in your editor and follow the steps included as comments to finish writing the `handleAirdrop` function. We include a description along with a link to the documentation you need to review in order to implement each line. The relevant code block is also included in [Listing 4.1]() below. 

***Listing 4.1
```
--> code here
```

### Implementation
In [Step 3]() we learned how to instantiate a connection to one of Solana's networks, and how to assign our account's public key property to a variable, so we can apply that same code here:

```javascript
const connection = new Connection(clusterApiUrl(network), "confirmed");
const publicKey = account.publicKey;
```

Following our previous heuristic of searching the docs for keywords, we can now search for "airdrop" to see if perhaps there's a function we can leverage. Lo an behold the Connection class has a `requestAirdrop` function that looks promising. It takes in a public key and a lamports number, and returns a promise that resolves to a string. It's not clear from the documentation, but after a little research you can confirm that the returned string represents a confirmation ID or signature.

From a function design standpoint, it seems reasonable that to request an airdrop we should pass in the account address (public key) and the amount of funds we're requesting. You might be wondering what a Lamport is. Solana's native token, SOL, is divisible into 1 billion Lamports. You can think of Lamports as the cents to SOL's dollar. 

<!-- Consider an aside on Leslie Lamport -->

```javascript
const confirmation = await connection.requestAirdrop(
  publicKey,
  1000000000
);
```

The above function will work just fine and our users will be able to airdrop 1 SOL every time they click on the button. But you can clean it up a bit by leveraging the `LAMPORTS_PER_SOL` constant from web3.js.

```javascript
const confirmation = await connection.requestAirdrop(
  publicKey,
  LAMPORTS_PER_SOL
);
```

Finally, before we can refresh our account's balance automatically, we need a way to make sure the blockchain ledger has been updated with our airdropped funds before we call `refreshBalance`. As with any database-type operations, blockchain state changes are asynchronous. In fact, given the decentralized nature of most blockchain protocols, some updates [can take a while]().

<!-- Consider an async aside or link to article on an Ethereum slow down -->

With that in mind, we need to wait for the airdrop to be confirmed before refreshing the balance. Searching the docs once again, we can see that our Connection class includes a confirmTransaction method that takes in a confirmation signature and commitment, and returns a promise that resolves once the transaction is confirmed by the network.

```javascript
await connection.confirmTransaction(confirmation, "confirmed");
```

Note that we don't need to assign a variable to the confirmation here. Once our wallet knows that the transaction was confirmed by the network, it can call `refreshBalance` and update the balance for the account.

```javascript
return await refreshBalance(network, account);
```

We've come a long way and should have a better idea of how blockchain protocols work at a high level. We built a wallet by creating a keypair, connected to the network, fetched data from it, and successfully requested test tokens. We've covered some of the basic Web 3 interactions except the most important one - transferring funds. We'll do that next in [Step 5](), so buckle up.

***Listing 4.2: Code for Airdropping Funds
```javascript
const handleAirdrop = async (network: Cluster, account: Keypair | null) => {
  if (!account) return;

  try {
    const connection = new Connection(clusterApiUrl(network), "confirmed");
    const publicKey = account.publicKey;
    const confirmation = await connection.requestAirdrop(
      publicKey,
      LAMPORTS_PER_SOL
    );
    const result = await connection.confirmTransaction(confirmation, "confirmed");
    return await refreshBalance(network, account);
  } catch (error) {
    console.log(error);
    return;
  }
};
```

## Step 5: Transferring Funds

### Challenge

### Implementation

## Step 6: Recovering an Account

### Challenge

### Implementation

## Conclusion
<!-- Review the application layout to solidify how everything is working together and where everything lives -->
