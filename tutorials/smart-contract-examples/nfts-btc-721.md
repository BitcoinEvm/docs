# NFTs (BTC-721)

The BTC-721 token standard on Bitcoin is similar to ERC-721 on Ethereum. It can represent virtually anything in Bitcoin:

* collectible items
* memberships
* lottery tickets
* in-game items
* and more

​

### Write a BTC-721 smart contract <a href="#write-a-brc-721-smart-contract" id="write-a-brc-721-smart-contract"></a>

Extending the OpenZeppelin ERC-721 contract, we can create a hypothetical PFP NFT collection (CryptoWizards) on Bitcoin

<pre data-overflow="wrap"><code>// SPDX-License-Identifier: 
MITpragma solidity ^0.8.0;​import 
"@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";contract CryptoWizards 
is ERC721URIStorage {constructor() ERC721("CryptoCollections", "Col") 
<strong>{}​function safeMint(address to, uint256 tokenId, string memory uri) 
</strong>public {_safeMint(to, tokenId);_setTokenURI(tokenId, uri);}}​
</code></pre>

### Compile the contracts <a href="#compile-the-contracts" id="compile-the-contracts"></a>

To compile your contracts, use the built-in `hardhat compile` task.cd smart-contract-examplesnpm installnpx hardhat compile​

### Deploy the contracts <a href="#deploy-the-contracts" id="deploy-the-contracts"></a>

Review config file `hardhat.config.ts`. The network configs should look like this.networks:

{% code overflow="wrap" %}
```
 {mynw: {url: "http://localhost:10002",accounts: {mnemonic: "<your mnemonic with funds>"},timeout: 100_000,},blockscoutVerify: {blockscoutURL: "http://localhost:4000", // your explorer URL...}}Run the deploy scripts using hardhat-deploy.npx hardhat deploy --tags NFTMake sure the accounts in hardhat.config.ts 
 
```
{% endcode %}

Interact with the contracts

Once the contracts are deployed, you can interact with them. We've prepared a few `hardhat tasks` to make it easy for you to interact with the contracts.#&#x20;

<pre class="language-solidity" data-overflow="wrap"><code class="lang-solidity">./CryptoCollection/Collection100.jpgnpx 
<strong>hardhat mint-nft --metadata wizard100.json --tokenid 100​# output should contain the URInpx hardhat get-nft --uri bfs://2203/0xbe86c97c2ef568d26e3524f54dc2202324092ffd/0xd4b1932Dff95a39c6024428454a3040F5e727980/100​
</strong></code></pre>
