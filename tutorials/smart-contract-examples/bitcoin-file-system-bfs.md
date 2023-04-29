# Bitcoin File System (BFS)

Let's build the Bitcoin File System (BFS), a decentralized, open, and permissionless file storage on Bitcoin.

* write a file to Bitcoin
* read a file from Bitcoin
* support large files

### Write the BFS smart contract <a href="#write-the-bfs-smart-contract" id="write-the-bfs-smart-contract"></a>

It turns out that writing the Bitcoin File System smart contract is very simple. Here is a basic contract to provide decentralized file storage on Bitcoinevm.



```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/Counters.sol";

error FileExists();

contract BFS {
        using Counters for Counters.Counter;
        Counters.Counter private idCounter;
        mapping(address => mapping(string => mapping(uint256 => bytes))) public dataStorage;
        mapping(address => mapping(string => uint256)) public chunks; // max chunk index
        mapping(address => mapping(string => uint256)) public bfsId;

        constructor () {
                idCounter.increment(); // start from 1
        }

        function store(string memory filename, uint256 chunkIndex, bytes memory _data) external {
                if (dataStorage[msg.sender][filename][chunkIndex].length > 0) {
                        revert FileExists();
                }
                dataStorage[msg.sender][filename][chunkIndex] = _data;
                if (chunks[msg.sender][filename] < chunkIndex) {
                        chunks[msg.sender][filename] = chunkIndex;
                }
                bfsId[msg.sender][filename] = idCounter.current();
                idCounter.increment();
        }

        function load(address addr, string memory filename, uint256 chunkIndex) public view returns (bytes memory, int256) {
                uint256 temp = chunkIndex + 1;
                int256 nextChunk = (temp > chunks[addr][filename]) ? -1 : int256(temp);
                return (dataStorage[addr][filename][chunkIndex], nextChunk);
        }

        function count(address addr, string memory filename) public view returns (uint256) {
                return chunks[addr][filename];
        }

        function getId(address addr, string memory filename) public view returns (uint256) {
                return bfsId[addr][filename];
        }
}
```

The `store()` function saves a chunk of a file. The `load()` function returns the data of a specific chunk. And the `getId()` function returns the ID of the file.

You can save a large file onto Bitcoin by splitting it into smaller chunks, committing them to Bitcoin, and retrieving & merging them back as needed.

BFS files are immutable.

## Clone the smart contract examples

We've prepared a few different examples for you to get started. The BNS example is located at **smart-contract-examples/contracts/BNS.sol**.

```solidity
git clone https://github.com/bitcoinevm/smart-contract-examples.git
```

## Compile the contracts

To compile your contracts, use the built-in `hardhat compile` task.

```solidity
cd smart-contract-examples
npm install
npx hardhat compile
```

## Deploy the contracts

Review config file `hardhat.config.ts`. The network configs should look like this.

{% code overflow="wrap" %}
```solidity
  networks: {
    mynw: {
      url: "http://localhost:10002",
      accounts: {
        mnemonic: "<your mnemonic with funds>"
      },
      timeout: 100_000,
    },
    blockscoutVerify: {
      blockscoutURL: "http://localhost:4000", // your explorer URL
      ...
    }
  }
```
{% endcode %}

Run the deploy scripts using `hardhat-deploy`.

{% hint style="warning" %}
Make sure the accounts in hardhat.config.ts have some BTC.
{% endhint %}

## Interact with the contracts

Once the contracts are deployed, you can interact with them. We've prepared a few `hardhat tasks` to make it easy for you to interact with the contracts.

{% code overflow="wrap" %}
```solidity
# store data
echo "this is some text" > data.txt
npx hardhat write-storage --filename ./data.txt
npx hardhat read-storage --filename ./data.txt # read data, print it as readable text
```
{% endcode %}
