# Bitcoin Name System (BNS)

Register your domain

{% code overflow="wrap" %}
```solidity
git clone https://github.com/trustlesscomputer/smart-contract-examplescd smart-contract-examplesnpm installnpx hardhat compilenpx hardhat register --domain yourname.btc \--contract 0x9c40182CE3C2B3f9eE196F43930476E79570826 \--privatekey 0xafafafafafafafafafafafafafafafafafafafafafafafafafafafafafafafaf 
# your private key
This will output the transaction hash, which can be used to track its status via the 
explorer
. After it is confirmed, you can try resolving that name by calling the BNS contractnpx hardhat resolve --domain yourname.btc \--contract 0x9c40182CE3C2B3f9eE196F43930476E79570826 \​
```
{% endcode %}

Let's build the Bitcoin Name System (BNS), a decentralized, open, and permissionless naming system on Bitcoin.

* buy and sell BNS name
* register a human-readable name like 'kevin.bit' or 'bob.btc'
* map a human-readable name like 'jill.btc' to a machine-readable id like a Bitcoin address

### Write the BNS smart contract <a href="#write-the-bns-smart-contract" id="write-the-bns-smart-contract"></a>

It turns out that writing the Bitcoin Name System smart contract is very simple. Here is a basic contract to provide a DNS-like naming system on BitcoinEVM.

{% code overflow="wrap" %}
```solidity
// SPDX-License-Identifier: MIT 
Progama solidity ^0.8.0;​import "@openzeppelin/contracts/token/ERC721/ERC721.sol";import "@openzeppelin/contracts/utils/Counters.sol";​contract BNS is ERC721 {​using Counters for Counters.Counter;Counters.Counter private _tokenIds;​mapping(bytes => uint256) public registry;mapping(bytes => bool) public registered;​mapping(uint256 => address) public resolver;​constructor() ERC721("Bitcoin Name System", "BNS") {}​function register(address owner, bytes memory name)publicreturns (uint256){require(!registered[name]);​uint256 id = _tokenIds.current();_mint(owner, id);registry[name] = id;registered[name] = true;resolver[id] = owner;​_tokenIds.increment();return id;}​function map(uint256 tokenId, address to) public {require(msg.sender == ownerOf(tokenId));resolver[tokenId] = to;}}
```
{% endcode %}

Extending ERC-721, we only need to implement the `register()` function to register a new name as an NFT and the `map()` function to map a human-readable name like 'bob.sat' to a machine-readable id like a wallet address.Sending and receiving a BNS name is now as simple as sending and receiving an NFT. You can also trade the BNS name on open markets since it's an ERC-721.

### Clone the smart contract examples <a href="#clone-the-smart-contract-examples" id="clone-the-smart-contract-examples"></a>

We've prepared a few different examples for you to get started. The BNS example is located at **smart-contract-examples/contracts/BNS.sol**.git clone https://github.com/bitcoinevm/smart-contract-examples.git​

### Compile the contracts <a href="#compile-the-contracts" id="compile-the-contracts"></a>

To compile your contracts, use the built-in `hardhat compile` task.cd smart-contract-examplesnpm installnpx hardhat compile​

### Deploy the contracts <a href="#deploy-the-contracts" id="deploy-the-contracts"></a>

Review config file `hardhat.config.ts`. The network configs should look like this.networks:

{% code overflow="wrap" %}
```solidity
 {mynw: {url: "http://localhost:10002",accounts: {mnemonic: "<your mnemonic with funds>"},timeout: 100_000,},blockscoutVerify: {blockscoutURL: "http://localhost:4000", // your explorer URL...}}Run the deploy scripts using hardhat-deploy.npx hardhat deploy --tags BNS
```
{% endcode %}

Make sure the accounts in hardhat.config.ts have some BTC.​

### Interact with the contracts <a href="#interact-with-the-contracts" id="interact-with-the-contracts"></a>

Once the contracts are deployed, you can interact with them. We've prepared a few `hardhat tasks` to make it easy for you to interact with the contracts.

{% code overflow="wrap" %}
```solidity
#register 

a namenpx hardhat register --domain "jill.btc"​ 

#create a mapping 
npx hardhat map "jill.btc" <a-wallet-address> ​

#resolve 

a human-readable name to a machine-readable idnpx hardhat resolve --domain "jill.btc"
```
{% endcode %}
