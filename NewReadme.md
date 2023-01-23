## New Changes

For clonning and changing into ethereum POS based chain.

First clone go-ethereum code from https://github.com/ethereum/go-ethereum

Prysm is an implementation of the Ethereum proof-of-stake consensus specification. In this quickstart, you’ll use Prysm to run an Ethereum node and optionally a validator. This will let you stake 32 ETH using hardware that you manage.

At a high level, we'll walk through the following flow:

Configure an execution node using an execution-layer client.
Configure a beacon node using Prysm, a consensus-layer client.
Configure a validator and stake ETH using Prysm (optional).

Clone prysm code
Generate JWT token

Follow the below steps :

Clone go-ethereum Code
https://github.com/ethereum/go-ethereum

Make changes like 
1. gas fees in the file /home/admin-pc/Documents/InfoGrainTask/go-ethereum/params/protocol_params.go
Change variable value TxGas from 21000 to 18000 

2. Minimum transcation per block is mentioned in /home/admin-pc/Documents/InfoGrainTask/go-ethereum/eth/protocols/eth/broadcast.go
Change maxTxPacketSize to the desired value

3. For transaction time will have to see

4. Changed geth word in the code with infogeth

5. Change folder name  cmd/geth cmd/infogeth

Note : For launching own chain we will have to do lot many changes like bootnode, protocol version, denomination, web3, config. This has to be defined as per required .

Once done compile the code 

make infogeth

For compiling we need go installed 

Fallow this url https://go.dev/doc/install

Once done compile the code 

## Building the source

For prerequisites and detailed build instructions please read the [Installation Instructions](https://geth.ethereum.org/docs/getting-started/installing-geth).

Building `infogeth` requires both a Go (version 1.18 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

```shell
make infogeth
```

or, to build the full suite of utilities:

```shell
make all
```

## Executables

The go-ethereum project comes with several wrappers/executables found in the `cmd`
directory.

|  Command   | Description                                                                                                                                                                                                                                                   
## It will start execution client

We will have to run beacon chain using prysm.sh 

We will have to run validator node


This will setup complete setup of POS based ethereum node

## Install Prysm 

mkdir prysm && cd prysm
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh

## Generate JWT Secret

## Optional. This command is necessary only if you've previously configured USE_PRYSM_VERSION
USE_PRYSM_VERSION=v3.1.2

## Required.
./prysm.sh beacon-chain generate-auth-secret

## Run a beacon node using Prysm

./prysm.sh beacon-chain --execution-endpoint=http://localhost:8551 --jwt-secret=path/to/jwt.hex

## Run a validator using Prysm

Download staking cli from https://github.com/ethereum/staking-deposit-cli/releases

./deposit new-mnemonic --num_validators=1 --mnemonic_language=english
./prysm.sh validator accounts import --keys-dir=<YOUR_FOLDER_PATH>
./prysm.sh validator --wallet-dir=<YOUR_FOLDER_PATH>                                                                                                                        

### Hardware Requirements

Minimum:

* CPU with 2+ cores
* 4GB RAM
* 1TB free storage space to sync the Mainnet
* 8 MBit/sec download Internet service

Recommended:

* Fast CPU with 4+ cores
* 16GB+ RAM
* High-performance SSD with at least 1TB of free space
* 25+ MBit/sec download Internet service

### Full node on the main Ethereum network

By far the most common scenario is people wanting to simply interact with the Ethereum
network: create accounts; transfer funds; deploy and interact with contracts. For this
particular use case, the user doesn't care about years-old historical data, so we can
sync quickly to the current state of the network. To do so:

```shell
$ infogeth console
```

This command will:
 * Start `infogeth` in snap sync mode (default, can be changed with the `--syncmode` flag),
   causing it to download more data in exchange for avoiding processing the entire history
   of the Ethereum network, which is very CPU intensive.
 * Start the built-in interactive [JavaScript console](https://infogeth.ethereum.org/docs/interface/javascript-console),
   (via the trailing `console` subcommand) through which you can interact using [`web3` methods](https://github.com/ChainSafe/web3.js/blob/0.20.7/DOCUMENTATION.md) 
   (note: the `web3` version bundled within `infogeth` is very old, and not up to date with official docs),
   as well as `infogeth`'s own [management APIs](https://infogeth.ethereum.org/docs/rpc/server).
   This tool is optional and if you leave it out you can always attach it to an already running
   `infogeth` instance with `infogeth attach`.

### A Full node on the Görli test network

Transitioning towards developers, if you'd like to play around with creating Ethereum
contracts, you almost certainly would like to do that without any real money involved until
you get the hang of the entire system. In other words, instead of attaching to the main
network, you want to join the **test** network with your node, which is fully equivalent to
the main network, but with play-Ether only.

```shell
$ infogeth --goerli console
```

The `console` subcommand has the same meaning as above and is equally
useful on the testnet too.

Specifying the `--goerli` flag, however, will reconfigure your `infogeth` instance a bit:

 * Instead of connecting to the main Ethereum network, the client will connect to the Görli
   test network, which uses different P2P bootnodes, different network IDs and genesis
   states.
 * Instead of using the default data directory (`~/.ethereum` on Linux for example), `infogeth`
   will nest itself one level deeper into a `goerli` subfolder (`~/.ethereum/goerli` on
   Linux). Note, on OSX and Linux this also means that attaching to a running testnet node
   requires the use of a custom endpoint since `infogeth attach` will try to attach to a
   production node endpoint by default, e.g.,
   `infogeth attach <datadir>/goerli/infogeth.ipc`. Windows users are not affected by
   this.

*Note: Although some internal protective measures prevent transactions from
crossing over between the main network and test network, you should always
use separate accounts for play and real money. Unless you manually move
accounts, `infogeth` will by default correctly separate the two networks and will not make any
accounts available between them.*


#### Defining the private genesis state

First, you'll need to create the genesis state of your networks, which all nodes need to be
aware of and agree upon. This consists of a small JSON file (e.g. call it `genesis.json`):

```json
{
  "config": {
    "chainId": <arbitrary positive integer>,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "londonBlock": 0
  },
  "alloc": {},
  "coinbase": "0x0000000000000000000000000000000000000000",
  "difficulty": "0x20000",
  "extraData": "",
  "gasLimit": "0x2fefd8",
  "nonce": "0x0000000000000042",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp": "0x00"
}
```

The above fields should be fine for most purposes, although we'd recommend changing
the `nonce` to some random value so you prevent unknown remote nodes from being able
to connect to you. If you'd like to pre-fund some accounts for easier testing, create
the accounts and populate the `alloc` field with their addresses.

```json
"alloc": {
  "0x0000000000000000000000000000000000000001": {
    "balance": "111111111"
  },
  "0x0000000000000000000000000000000000000002": {
    "balance": "222222222"
  }
}
```

With the genesis state defined in the above JSON file, you'll need to initialize **every**
`infogeth` node with it prior to starting it up to ensure all blockchain parameters are correctly
set:

```shell
$ infogeth init path/to/genesis.json
```



## License

The go-ethereum library (i.e. all code outside of the `cmd` directory) is licensed under the
[GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html),
also included in our repository in the `COPYING.LESSER` file.

The go-ethereum binaries (i.e. all code inside of the `cmd` directory) are licensed under the
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html), also
included in our repository in the `COPYING` file.
