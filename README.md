## Run-a-node-in-Geth-Developer-Mode
# Step - 1 : Start Geth in Dev Mode
On the Terminal type : 
```
geth --dev --http --http.api eth,web3,net --http.corsdomain "http://remix.ethereum.org"
```
![Geth-dev-mode_1](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/8e03b3e62f39850af7994ac4c6d57566dbf2f861/Screenshot%20from%202023-08-23%2000-52-50.png)

- The terminal will display the following logs, confirming Geth has started successfully in developer mode:

![Geth-dev-mode_2](https://github.com/LifnaJos/Run-a-node-in-Geth-Developer-Mode/blob/98b0eb874d16206a4ee69358bfc487f1266ab267/Screenshot%20from%202023-08-23%2001-06-36.png)
## Acknowledgements
* [Go Etehreum - Developer Mode](https://geth.ethereum.org/docs/developers/dapp-developer/dev-mode)
* [Remix Ethereum IDE - Docs](https://remix-ide.readthedocs.io/en/latest/run.html#more-about-external-http-provider)
