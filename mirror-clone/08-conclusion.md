# Conclusion

Congratulations! You've built a Web 3-native blog that resembles the functionality of [Mirror.xyz](https://mirror.xyz/).

By leveraging a handful of protocols and tools like Polygon, Arweave, Hardhat, ethers.js, and [ArDB](https://github.com/textury/ardb), you built a full stack dApp in no time. In [Step 1]() we leveraged a Next.js template to scaffold a simple application and we immediately got to work on turning it into a dApp in [Step 2]() where we built functionality to connect to a user's [MetaMask]() wallet.

We then gave our users the ability to publish posts by ### in [Step 3](). Not only that but because Arweave stores data forever (or pretty much forever), we gave our users the ability to publish censorship-resistant, immutable posts over which they have full control.

Expanding from write to read functionality, in [Step 4]() we implemented the application's fetch function that retrieves data from Arweave by leveraging a transaction id and the address tag on our Arweave application deployment. This allowed us to display individual posts using the transaction id in the URI params.

In [Step 5](), we implemented similar functions to fetch the last 10 posts and display them on the dApp's home page. By clicking on these, users call the functionality implemented in [Step 4]() to display individual posts.

Not satisfied with a simple blog, and keen to explore the possibilities provided by leveraging the economics layer native to crypto rails, we established a smart contract to mint and manage NFTs for each post in [Step 6](). The contract automatically mints an NFT owned by the author when the post is published.

Finally, [Step 7]() completed the NFT feature by giving users the ability to transfer their NFTs. An author can choose to transfer their post's NFT by clicking a button and providing the recipient's address. Thereafter, the NFT owner can do the same with any other user.

In the process of building the blog, you learned how to connect a wallet to effectively authenticate. Hopefully you expanded your understanding of Web 3 primitives like providers, signers and NFTs. 

You also learned how the frontend of the stack is similar to the Web 2.0 stack you're used to, and expanded your backend toolbox by interacting with Arweave for storage and Polygon for tracking NFT ownership. You became familiar with ethers.js, the JavaScript library for connecting a user's wallet to the dApp and interacting with blockchain protocols. Moreover, you gained exposure to Arweave, which is one of the most important Web 3-native data storage protocols for its ease of use, censorship resistance and scalability.

Web 3 offers developers a new world in which to build the future of internet and re-imagine the possibilities. It expands the building blocks and in so doing enhances the ability to create new, innovative applications to solve worthwhile problems. 

The opportunities are endless and we wish you well on your journey!

_If you want to connect with an amazing community of developers, join us on [Discord](https://discord.gg/fszyM7K)._

![Figure x: With ten miles behind me and ten thousand more to go…]()
##### _Figure x: With ten miles behind me and ten thousand more to go…_

## Additional Resources

- [TBD]()
