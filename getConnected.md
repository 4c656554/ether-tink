#Ethereum Private Blockchain Tinkering:

Guidlines for getting connected to the private network.

##Overview.

 The network consists of Raspberry pi with dhcp server (ip address `192.168.0.1`) running a media wiki server. This is for sharing EnodeURLs (the peer to peer networking addresses) and Account numbers.
 
 After connecting to the network, in your browser, got to `192.168.0.1/index.html`
 

The intent is to create a network of local computers over which a private blockchain can be created. To help do this, follow the guidance below for different clients:
 
**Instructions for Go-ethereum "geth":**
 
1. Install go-ethereum or other client; https://ethereum.github.io/go-ethereum/install/
2. Establish ethernet connection to pi - connect cable to switch (or wifi to router) and make sure network interface "obtains IP address automatically" in your adapter settings.
3. Prepare a directory on your machine for blockchain data. Save the below as a file called `genesis.json`

```json
{
  "alloc": {},
  "nonce": "0x0000000000000042",
  "difficulty": "0x050000",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "timestamp": "0x00",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "extraData": "0x",
  "gasLimit": "0x2fefd8"
}
```

*This defines the genesis block for the private blockchain. This must be the same for all participants*

4. On command line run, `geth --datadir /path/to/your/directory --networkid 12345 --port 33333 init /path/to/your/directory/genesis.json`
   *This initialises the first block. You are now primed to connect to the priveate Ethereum p2p network*
5. Now do:
       `geth --datadir /path/to/your/directory --networkid 12345 --port 33333 `
   *This gets the machine ready to find the local network. The networkid and port were arbitrarily chosen when I set up the pi. N.b. `--verbosity 5`  will give lots of messages about the network and what is going on.*
   If unsuccessful, check your firewall (allow connections on TCP port 33333).
6. Next, in a new command window (leave the other one open):
       `geth --datadir /path/to/your/directory --networkid 12345 --port 33333 attach`
  *This gives you a javascript console to interact with; a `>` prompt* [command reference](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#management-api-reference)
7. Next do:
       `admin.addPeer("enode://6c461262a4cdb658d7852af41a433668dea467da87168390d02e22c6191fc136063a7df859e46a65786de6c3f73670f00d2378b5e17ec3142a84ded8029e1728@192.168.0.1:33333")`
       Replacing the enode URL with one from the wiki.
       
  This connects the link (TCP) for your machine in the local network. It creates the first Peer to Peer link for your machine and results in a connection to the RaspberryPi. The address enodeURL was taken from the RPi's node.
8. Do `admin.peers` to check, you should see a list of connected peers.
9. `personal.newAccount("yourPassword")` to create a new accoount - it will give you a hex format accnt no
10. `miner.setEtherbase("yourAccount")`
11. `miner.start()`
    *this will start mining, with the rewards going to your account. You are now in a position to start playing with smart contracts on the private network!*
12. To send a transaction

`eth.sendTransaction(<amount to send here, e.g. 100000000>, {from:eth.accounts[0],to:"0X<insert account no>,gas:3000000})`

Or open a UI, e.g. [mist](https://github.com/ethereum/mist/releases), metamask chrome plugin (https://metamask.io/)
