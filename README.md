# Run-a-node-in-Geth-Developer-Mode
This repository guides the users how to c**reate a single-node Ethereum test network with no connections to any external peers.** 

Also, this repository helps to spin up a local Geth into a testnet and deploy a simple smart contract written using the Remix online integrated development environment (IDE). 

That is, this repository enables developers to **experiment with Geth's source code or develop new applications without having to sync to a pre-existing public network.** 

Note : Starting Geth in developer mode does the following:
* Initializes the data directory with a **testing genesis block**
* Sets **max peers to 0** (meaning Geth does not search for peers)
* **Turns off discovery** by other nodes (meaning the node is invisible to other nodes)
* Sets the **gas price to 0** (no cost to send transactions)
* Uses the **Clique proof-of-authority** consensus engine which allows blocks to be mined as-needed without excessive CPU and memory consumption
* Uses **on-demand block generation**, producing blocks when transactions are waiting to be mined

 Blocks are only mined when there are pending transactions. Developers can break things on this network without affecting other users. This page will demonstrate 

## Step - 1 : Start Geth in Dev Mode
On the Terminal type : 
```
geth --dev --http --http.api eth,web3,net --http.corsdomain "http://remix.ethereum.org"
```
![Geth-dev-mode_1](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/8e03b3e62f39850af7994ac4c6d57566dbf2f861/Screenshot%20from%202023-08-23%2000-52-50.png)

- The terminal will display the following logs, confirming Geth has started successfully in developer mode:

![Geth-dev-mode_2](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/98b0eb874d16206a4ee69358bfc487f1266ab267/Screenshot%20from%202023-08-23%2001-06-36.png)

## Step - 2 : Attach a Javascript console and interact with it
```
geth attach /tmp/geth.ipc
```
**1. To display the existing accounts**
```
eth.accounts
```
Note:
- This returns an array containing a single address, eventhough no accounts having yet been explicitly created.
- This is the **coinbase** account.
- The coinbase address is the recipient of the total amount of ether created at the local network genesis.
```
eth.coinbase==eth.accounts[0]
```
**2. Querying the ether balance of the coinbase account**
```
eth.getBalance(eth.coinbase)/1e18
```
```
eth.getBalance(eth.coinbase)
```
```
web3.fromWei(eth.getBalance(eth.coinbase))
```
![Geth Console]()
Note:
- The return value is in units of Wei, which is divided by 118 to give units of ether.
- This can be done explicitly or by calling the web3.FromWei() function

## Step - 3 : Create an another account using clef
**1. Open a new terminal, and create a folder for the new node :** 
```
mkdir geth_node
```
![Clef account]()

## Acknowledgements
* [Go Etehreum - Developer Mode](https://geth.ethereum.org/docs/developers/dapp-developer/dev-mode)
  
* [Remix Ethereum IDE - Docs](https://remix-ide.readthedocs.io/en/latest/run.html#more-about-external-http-provider)

* [Basic Writing & Formatting Syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
