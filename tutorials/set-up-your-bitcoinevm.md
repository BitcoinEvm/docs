# Set up your BitcoinEVM

Welcome to the BitcoinEVM developer guides. These developer guides show you how to set up your own BitcoinEVM and build Bitcoin dapps using Solidity smart contracts.

BitcoinEVM is a layer-1 protocol that aims to make Bitcoin as generalized as possible — usable for far more than just a currency. It enables developers to create DAO, DEX, NFT, token, auction, lending, data storage, and many other use cases on Bitcoin.

Follow the guide in this repo for easy setup: ​

curl -s https://raw.githubusercontent.com/Bitcoinevm/bitcoinevm-node-easy/main/setup-mainnet.sh -o setup-bitcoinevm.sh

chmod +x setup-bitcoinevm.sh

sh ./setup-bitcoinevm.sh

For manual setup follow below:

Install Bitcoin Core

​[Bitcoin Core](https://bitcoincore.org/) provides both a Bitcoin full node and a wallet. Working with BitcoinEVM requires a Bitcoin full node with RPC enabled.&#x20;

After installing Bitcoin Core, run bitcoind with -server=1 :

./bitcoind -server=1\


It may take some time for your Bitcoin full node to be fully synced.

Install BitcoinEVM

BitcoinEVM is a Turing-complete virtual machine. It is written in Go, and pre-built binaries are available on the [download page](https://github.com/TrustlessComputer/tc-prebuilds)[.](https://github.com/bitcoinevm/smart-contract-examples)\


After downloading the binary, run chmod +x \<filename> to allow executable permission. On MacOS, you may also have to go to Privacy & Security to allow the file to run.

Start your BitcoinEVM:

./tc -c \<btc-node-cookie-file-path> -i \<your-tc-address> # specify your BitcoinEVM native address

If you set up your Bitcoin full node with a username and password, add those parameters.

./tc -u \<btc-node-username> -p \<btc-node-password> -i \<your-tc-address>



Since BitcoinEVM reuses EVM, your BTC native address is similar to an Ethereum address. You can create a new BTC address with any EVM-compatible wallet. We recommend MetaMask.

Add BitcoinEVM to MetaMask

In MetaMask, click on Networks --> Add Network --> Add a network manually. Use the following settings to point MetaMask to the BitcoinEVM running on your machine.

\
Name: BitcoinEVM\
\
URL: http://localhost:10002\
\
ChainID: 2203\
\
Symbol: BTC



BTC is the native cryptocurrency of BitcoinEVM. Similar to ETH, you can use BTC to pay transaction fees, deploy smart contracts, and spend in dapps.

​

Setup BitcoinEVM Explorer



BitcoinEVM reuses [Blockscout](https://www.blockscout.com/) for blockchain exploration data such as blocks, transactions, and addresses.&#x20;

Launch BitcoinEVM Explorer:

docker compose -f docker/docker-compose-blockscout.yml up

And open this URL on your browser:

http://0.0.0.0:4000

​

Setup a development environment\


There are many different tools for smart contract development. For this developer guide, we use [Hardhat](https://hardhat.org/), but you're free to use a different tool.

npm install --save-dev hardhat\


If you're brand new to smart contracts, start with the [Introduction to Smart Contracts](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html).
