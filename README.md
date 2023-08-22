# Run-a-node-in-Geth-Developer-Mode
This repository guides the users
1. **[Create a single-node Ethereum test network with no connections to any external peers.](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/main/README.md#tutorial---1--create-a-single-node-ethereum-test-network-with-no-connections-to-any-external-peers)**
2. **[Convert a local Geth into a testnet and deploy a simple smart contract written using the Remix](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/main/README.md#tutorial---2--convert-a-local-geth-into-a-testnet-and-deploy-a-simple-smart-contract-written-using-the-remix)** online integrated development environment (IDE). 

Note : Starting Geth in developer mode does the following:
* Initializes the data directory with a **testing genesis block**
* Sets **max peers to 0** (meaning Geth does not search for peers)
* **Turns off discovery** by other nodes (meaning the node is invisible to other nodes)
* Sets the **gas price to 0** (no cost to send transactions)
* Uses the **Clique proof-of-authority** consensus engine which allows blocks to be mined as-needed without excessive CPU and memory consumption
* Uses **on-demand block generation**, producing blocks when transactions are waiting to be mined

# Tutorial - 1 : Create a single-node Ethereum test network with no connections to any external peers

## Step - 1 : Start Geth in Dev Mode
On the Terminal type : 
```
geth --dev --http --http.api eth,web3,net --http.corsdomain "http://remix.ethereum.org"
```
![Geth-dev-mode_1](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/8e03b3e62f39850af7994ac4c6d57566dbf2f861/Screenshot%20from%202023-08-23%2000-52-50.png)

- The terminal will display the following logs, confirming Geth has started successfully in developer mode:

![Geth-dev-mode_2](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/98b0eb874d16206a4ee69358bfc487f1266ab267/Screenshot%20from%202023-08-23%2001-06-36.png)

## Step - 2 : Create an another account using clef
- Open a new terminal, and create a folder for the new node : 
```
mkdir geth_node
```
```
clef newaccount --keystore geth_node
```
Note :
- When prompted type the password.
- For quick reference, note it. Ex. gethnode123
- It generates the Public key and stores the secret key in the folder, geth_node
  
![Clef account](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/7034aef0fab964f3b728977a557d02b21d64fe66/Screenshot%20from%202023-08-23%2002-04-21.png)

## Step - 3 : Attach a Javascript console and interact with it
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
![Geth Console](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/ace725de0de5d9ee7ed14e06ab26f0a3375e8dae/images/Screenshot%20from%202023-08-23%2002-17-57.png)
Note:
- The return value is in units of Wei, which is divided by 118 to give units of ether.
- This can be done explicitly or by calling the web3.FromWei() function

**3. Query the ehther balance of the new account created via clef in step 2**
```
eth.getBalance("0x9Ec0a126c014C31456384746d83071F19925aAA2")
```
**4. Send funds from coinbase to the new account created via clef**
```
eth.sendTransaction({from: eth.coinbase, to: "0x9Ec0a126c014C31456384746d83071F19925aAA2", value: web3.toWei(50, "ether")})
```
- If successful, transaction hash is generated as a repsonse.
- After the funds transfer check the balance of coinbase and the receipient account.

**5. To check the transaction details**
```
eth.getTransaction("0x19ec8c7da780b5492c1cca65408f24f1ad8b5a38114a0a5c862e47e875f6b6b8")
```
- Here, **0x19ec8c7da780b5492c1cca65408f24f1ad8b5a38114a0a5c862e47e875f6b6b8** is the hash generated when the transaction was submitted.
  
![GEth_Console](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/c37b04a7cbc1d3c129862065a5aa63343f3d3661/images/Screenshot%20from%202023-08-23%2002-43-33.png)

- Now go to the first terminal, where our geth is running in dev mode.
- Here, we can see that the first block is mined.

![Geth Dev mode](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/5174383bd35b9b78f6ffeb62a279ba2be14b422c/images/Screenshot%20from%202023-08-23%2002-46-55.png)

# Tutorial - 2 : Convert a local Geth into a testnet and deploy a simple smart contract written using the Remix

## Step - 1 : Start Geth in Dev Mode
```
geth --http --http.corsdomain="https://remix.ethereum.org" --http.api web3,eth,debug,personal,net --vmdebug --datadir geth_node --dev console
```
![geth dev 1](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/7dd08fd5ca97bf38eb3234b7f05d54500a2bf989/images/Screenshot%20from%202023-08-23%2003-22-08.png)

- The terminal will display the following logs, confirming Geth has started successfully in developer mode.
- The node is waiting for the transaction to mine the block

![geth dev 2](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/7dd08fd5ca97bf38eb3234b7f05d54500a2bf989/images/Screenshot%20from%202023-08-23%2003-22-27.png)

## Step - 2 : Open a Smart Contract in Remix IDE
**1. Open the Storage.sol file available at [Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.18+commit.87f61d96.js)**
  
![Remix_IDE - Storage.sol](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/52e10be7769b6ec319c3c49587fee1f98fe21dc8/images/Screenshot%20from%202023-08-23%2002-57-30.png)

**2. Compile and deploy using Remix IDE**
- Click the green play above the contract file

![Compile](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/f49c1109f572267b0442f63b39909ab0b53c075c/images/Screenshot%20from%202023-08-23%2003-01-08.png)

- Click the **Deploy** menu in Remix IDE.
- In the drop-down menu labelled **ENVIRONMENT**, select **Custom - External Http Provider**.

![Menu](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/1a5ace708de3ec845a6bd7e18839ea31425275d7/images/Screenshot%20from%202023-08-23%2003-04-48.png)
  
- This will open an information pop-up with instructions for configuring Geth
- Click OK.

![Congig](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/5c84dbdc83420910abc718e8a304667536c0336e/images/Screenshot%20from%202023-08-23%2003-12-20.png)

- The **ACCOUNT** field should automatically populate with the address of the account created earlier using the Geth Javascript console.

![Account](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/e316ccf5a30f79ea459c6b2d631b8a90b1edf064/images/Screenshot%20from%202023-08-23%2003-34-51.png)

- This account is same as the one displayed on the Javascript Console

![Console](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/70a18adfb0dc5bb0bfaf2cc91988fad4014f976e/images/Screenshot%20from%202023-08-23%2003-39-42.png)

- To deploy Storage.sol, click **DEPLOY**.
- A transaction occurs and block is mined
 
![Deploy](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/7ea97c0bf9688066479a0538754dc1df0dd811ec/images/Screenshot%20from%202023-08-23%2003-44-15.png)

- The following logs in the Geth terminal confirm that the contract was successfully deployed.

![Geth dev node](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/4ed3e63fff8df0920798a4b69e146c0d35eb3055/images/Screenshot%20from%202023-08-23%2003-52-56.png)

- To fetch the transaction details from the terminal

![Transaction](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/c761fae4f781979c6ea7d8bba000bcef9243b9a4/images/Screenshot%20from%202023-08-23%2003-54-28.png)

**3. Interact with contract using Remix**
- The contract is now **deployed on a local testnet version of the Ethereum blockchain**.
- This means there is a **contract address that contains executable bytecode that can be invoked by sending transactions with instructions**
- After deployment, the **Deployed Contracts** tab in the sidebar automatically populates with the public functions exposed by Storage.sol.
- To send a value to the contract storage, type a number in the field adjacent to the store button, then click the button.
- Transaction occurs and block is mined
  
Note : 
- The **from** address is the account that sent the transaction,
- the **to** address is the **deployment address of the contract**.
- The **value** entered into Remix is now in **storage at that contract address**.
- This can be retrieved using Remix by calling the retrieve function - to do this simply click the retrieve button.
  
![send](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/cc9a4cbcb0bf5325a55da65de5a1f61646b3b233/images/Screenshot%20from%202023-08-23%2004-06-00.png)

- Check the transaction mined and added to the block from the JavaScript Console

![Transaction console](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/d467c62b28a45ff43328a7dad876e097406808f0/images/Screenshot%20from%202023-08-23%2004-09-05.png)

- To retrive value, click the button - **retrieve**

![Retrieve](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/cc9a4cbcb0bf5325a55da65de5a1f61646b3b233/images/Screenshot%20from%202023-08-23%2004-05-42.png)

- Alternatively, it can be retrieved using **web3.getStorageAt** using the Geth Javascript console.
```
web3.eth.getStorageAt("0xe5c5aaa604c12273b92d8e48d133158abbf44a5f", 0)
```
- The following command returns the value in the contract storage
- Here, **0xe5c5aaa604c12273b92d8e48d133158abbf44a5f** (replace the given address of the receipient in the Geth logs).
-  Its the storage address

![Output](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/7e4d51b122a109bec68f73aba0ff8ffe6590c53f/images/Screenshot%20from%202023-08-23%2004-27-52.png)

- Here, **0x000000000000000000000000000000000000000000000000000000000000002d** is the hexadecimal value for 45 with which the number was reinitialized.

## Acknowledgements
* [Go Etehreum - Developer Mode](https://geth.ethereum.org/docs/developers/dapp-developer/dev-mode)
  
* [Remix Ethereum IDE - Docs](https://remix-ide.readthedocs.io/en/latest/run.html#more-about-external-http-provider)

* [Basic Writing & Formatting Syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
