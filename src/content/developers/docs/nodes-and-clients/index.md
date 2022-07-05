---
title: Nodes and clients
description: An overview of Ethereum nodes and client software, plus how to set up a node and why you should do it.
lang: en
sidebar: true
sidebarDepth: 2
preMergeBanner: true
---

Ethereum is a distributed network of computers running software (known as nodes) that can verify blocks and transaction data. You need an application, known as a client, on your computer to "run" a node.

**Note: it is still possible to run an execution client on its own. However, this will no longer be possible after [The Merge](/upgrades/merge). After The Merge, both execution and consensus clients must be run together in order for a user to gain access to the Ethereum network. Some testnets (e.g. Kiln, Ropsten) have already been through their versions of The Merge, meaning execution clients alone are already insufficient for accessing those networks unless they are coupled to a consensus client that can keep track of the head of the chain.**

## Prerequisites {#prerequisites}

You should understand the concept of a peer-to-peer network and the [basics of the EVM](/developers/docs/evm/) before diving deeper and running your own instance of an Ethereum client. Take a look at our [introduction to Ethereum](/developers/docs/intro-to-ethereum/).

If you're new to the topic of nodes, we recommend first checking out our user-friendly introduction on [running an Ethereum node](/run-a-node).

## What are nodes and clients? {#what-are-nodes-and-clients}

"Node" refers to a running piece of client software. A client is an implementation of Ethereum that verifies all transactions in each block, keeping the network secure and the data accurate.

An Ethereum node consists of two pieces of software: execution client and consensus client. 
- The execution client, also known as the Execution Engine or formerly the Eth1 client, listens to new transactions broadcasted in the network, executes them in EVM and holds the latest state, database of all current Ethereum data. 
- The consensus client, also known as the Beacon Node or formerly the Eth2 client, implements the proof-of-stake consensus algorithm which enables network to achieve agreement based on validated data from the execution client. 

Before [The Merge](/upgrades/merge/), network only consisted of what are now execution clients. One client software provided both execution enviroment and consensus verification of blocks produced by miners. As the network transitions to proof-of-stake, consensus is separted into its own client implementation.
Modular design with various pieces of software working together can be referred to as [encapusalted complexity](https://vitalik.ca/general/2022/02/28/complexity.html). This approach makes it easier to execute The Merge in a seamless way and also enables reuse of individual clients, for example in the L2 ecosystem. 

![Ethereum client](./eth1eth2client.png)
Simplified diagram of a coupled execution and consensus client.

### Client diversity {#client-diversity}

Both [Execution clients](/developers/docs/nodes-and-clients/#consensus-clients) and [Conesnsus clients](/developers/docs/nodes-and-clients/#execution-clients) exist, in a variety of programming languages such as Go, Rust, JavaScript, Typescript, Python, C# .NET, Nim and Java. 

Multiple client implementations make the network stronger and more diverse. The ideal goal is to achieve diversity without any client dominating, thereby reducing any single points of failure. The variety of languages also invites a broader developer community and allows them to create integrations in their preferred language.

Learn more about [Client diversity](/developers/docs/client-diversity). 

What these implementations have in common is they all follow a formal specification. Specifications dictate how the Ethereum network and blockchain functions. Every technical detail is defined and specifications can be found as: 
 
- Originally, the [Ethereum Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf)
- [Execution specs](https://github.com/ethereum/execution-specs/)
- [Consensus specs](https://github.com/ethereum/consensus-specs)
- [EIPs](https://eips.ethereum.org/) implemented in various [network upgrades](/history/)


### Tracking nodes in the network {#network-overview}

There are multiple trackers which offer a real-time overview of nodes in the Ethereum network. Note that due to the nature of decentralized networks, these crawlers can only offer a limited view of the network and might report different results. 

- [Map of nodes](https://etherscan.io/nodetracker) by Etherscan
- [Ethernodes](https://ethernodes.org/) by Bitfly
- [Ethereum Node Crawler](https://crawler.ethereum.org/), an [open project](https://github.com/ethereum/node-crawler)


## Node types {#node-types}

If you want to [run your own node](/developers/docs/nodes-and-clients/run-a-node/), you should understand that there are different types of node that consume data differently. In fact, clients can run 3 different types of node - light, full and archive. There are also options of different sync strategies which enable faster synchronization time. Synchronization refers to how quickly it can get the most up-to-date information on Ethereum's state.

### Full node {#full-node}

- Stores full blockchain data (although this is periodically pruned so a full node does not store all state data back to genesis)
- Participates in block validation, verifies all blocks and states.
- All states can be derived from a full node (although very old states are reconstructed from requests made to archive nodes).
- Serves the network and provides data on request.

### Light node {#light-node}

Instead of downloading every block, light nodes download block headers. These headers only contain summary information about the contents of the blocks. Any other information required by the light node gets requested from a full node. The light node can then independently verify the data they receive against the state roots in the block headers. Light nodes enable users to participate in the Ethereum network without the powerful hardware or high bandwidth required to run full nodes. Eventually, light nodes might run on mobile phones or embedded devices. The light nodes do not participate in consensus (i.e. they cannot be miners/validators), but they can access the Ethereum blockchain with the same functionality as a full node.

The execution client Geth includes a [light sync](https://github.com/ethereum/devp2p/blob/master/caps/les.md) option. However, a light Geth node relies upon full nodes serving light node data. Few full nodes opt to serve light node data, meaning light nodes often fail to find peers. There are currently no production-ready light clients on the consensus layer; however, several are in development.

There are also potential routes to providing light client data over the [gossip network](https://www.ethportal.net/). This is advantageous because the gossip network could support a network of light nodes without requiring full nodes to serve requests.

Ethereum does not support a large population of light nodes yet, but light node support is an area expected to develop rapidly in the near future.

### Archive node {#archive-node}

- Stores everything kept in the full node and builds an archive of historical states. It is needed if you want to query something like an account balance at block #4,000,000, or simply and reliably test your own transactions set without mining them using tracing. 
- This data represents units of terabytes, which makes archive nodes less attractive for average users but can be handy for services like block explorers, wallet vendors, and chain analytics.

Syncing clients in any mode other than archive will result in pruned blockchain data. This means, there is no archive of all historical states but the full node is able to build them on demand. 

## Why should I run an Ethereum node? {#why-should-i-run-an-ethereum-node}

Running a node allows you to directly, trustlessly and privately use Ethereum while supporting the network by keeping it more robust and decentralized. 

### Benefits to you {#benefits-to-you}

Running your own node enables you to use Ethereum in a truly private, self-sufficient and trustless manner. You don't need to trust the network because you can verify the data yourself with your client. "Don't trust, verify" is a popular blockchain mantra.

- Your node verifies all the transactions and blocks against consensus rules by itself. This means you don’t have to rely on any other nodes in the network or fully trust them.
- You can use an Ethereum wallet with your own node. You can use dapps more securely and privately because you won't have to leak your addresses and balances to random nodes. Everything can be checked with your own client. [MetaMask](https://metamask.io), [Frame](https://frame.sh/) and [many other wallets](/wallets/find-wallet/) can be easily pointed to your own local node. (Check wallets with `RPC importing` filter.)
- You can run and self-host other services which depend on data from Ethereum. Most importantly for example Beacon Chain validator but also software like L2 infrastracture, block explorers, payment processors, etc. 
- You can provide your own custom RPC endpoints. Publicly for the community or even privately hosted Ethereum endpoint enables people to use your node and avoid big centralized providers. 
- You can connect to your node using **Inter-process Communications (IPC)** or rewrite the node to load your program as a plugin. This grants low latency, which helps a lot, e.g. when processing a lot of data using web3 libraries or when you need to replace your transactions as fast as possible (i.e. frontrunning).

![How you access Ethereum via your application and nodes](./nodes.png)

### Network benefits {#network-benefits}

A diverse set of nodes is important for Ethereum’s health, security and operational resiliency.

- Full nodes enforce the consensus rules so they can’t be tricked into accepting blocks that don't follow them. This provides extra security in the network because if all the nodes were light nodes, which don't do full verification, validators could attack the network. 
- In case of an attack which overcomes the crypto-economic defenses of [proof-of-stake](/developers/docs/consensus-mechanisms/pos/#what-is-pos), a social recovery can be performed by full nodes choosing to follow the honest chain.
- More nodes in the network result in a more diverse and robust network. This is the ultimate goal of decentralization, which enables censorship resistant and reliable system.
- They provide access to blockchain data for lightweight clients that depend on it. In high peaks of usage, there need to be enough full nodes to help light nodes sync. Light nodes don't store the whole blockchain, instead they verify data via the [state roots in block headers](/developers/docs/blocks/#block-anatomy). They can request more information from blocks if they need it.

If you run a full node, the whole Ethereum network benefits from it.

## Running your own node {#running-your-own-node}

Interested in running your own Ethereum client?

For a beginner-friendly introduction visit our [run a node](/run-a-node) page to learn more.

If you're more of a technical user, dive into more details and options on how to [spin up your own node](/developers/docs/nodes-and-clients/run-a-node/).


## Alternatives {#alternatives}

Running your own node can be difficult and you don’t always need to run your own instance. In this case, you can use a third party API provider like [Infura](https://infura.io), [Alchemy](https://alchemyapi.io), or [QuikNode](https://www.quiknode.io). Alternatively, [ArchiveNode](https://archivenode.io/) is a community-funded Archive node that hopes to bring archive data on the Ethereum blockchain to independent developers who otherwise couldn't afford it. For an overview of using these services, check out [nodes as a service](/developers/docs/nodes-and-clients/nodes-as-a-service/).

If somebody runs an Ethereum node with a public API in your community, you can point your light wallets (like MetaMask) to a community node [via Custom RPC](https://metamask.zendesk.com/hc/en-us/articles/360015290012-Using-a-Local-Node) and gain more privacy than with some random trusted third party.

On the other hand, if you run a client, you can share it with your friends who might need it.

## Execution clients (formerly 'Eth1 clients') {#execution-clients}

The Ethereum community maintains multiple open-source execution clients (previously known as 'Eth1 clients', or just 'Ethereum clients'), developed by different teams using different programming languages. 

This table summarizes the different clients. All of them are actively maintained to stay updated with network upgrades, follow current specifications and pass [client tests](https://github.com/ethereum/tests). 

| Client                                                                    | Language | Operating systems     | Networks                                   | Sync strategies     | State pruning   |
| ------------------------------------------------------------------------- | -------- | --------------------- | ------------------------------------------ | ------------------- | --------------- |
| [Geth](https://geth.ethereum.org/)                                        | Go       | Linux, Windows, macOS | Mainnet, Görli, Rinkeby, Ropsten           | Snap, Full          | Archive, Pruned |
| [Nethermind](http://nethermind.io/)                                       | C#, .NET | Linux, Windows, macOS | Mainnet, Görli, Ropsten, Rinkeby, and more | Snap, Fast, Beam | Archive, Pruned |
| [Besu](https://pegasys.tech/solutions/hyperledger-besu/)                  | Java     | Linux, Windows, macOS | Mainnet, Rinkeby, Ropsten, Görli, and more | Fast, Full          | Archive, Pruned |
| [Erigon](https://github.com/ledgerwatch/erigon)                           | Go       | Linux, Windows, macOS | Mainnet, Görli, Rinkeby, Ropsten           | Full                | Archive, Pruned |

For more on supported networks, read up on [Ethereum networks](/developers/docs/networks/).

Each client has unique use cases and advantages, so you should choose one based on your own preferences. Diversity allows implementations to be focused on different features and user audiences. You may want to choose a client based on features, support, programming language, or licences.

#### Go Ethereum {#geth}

Go Ethereum (Geth for short) is one of the original implementations of the Ethereum protocol. Currently, it is the most widespread client with the biggest user base and variety of tooling for users and developers. It is written in Go, fully open source and licensed under the GNU LGPL v3.

#### Nethermind {#nethermind}

Nethermind is an Ethereum implementation created with the C# .NET tech stack, running on all major platforms including ARM. It offers great performance with:

- an optimized virtual machine
- state access
- networking and rich features like Prometheus/Grafana dashboards, seq enterprise logging support, JSON RPC tracing, and analytics plugins.

Nethermind also has [detailed documentation](https://docs.nethermind.io), strong dev support, an online community and 24/7 support available for premium users.

#### Besu {#besu}

Hyperledger Besu is an enterprise-grade Ethereum client for public and permissioned networks. It runs all of the Ethereum Mainnet features, from tracing to GraphQL, has extensive monitoring and is supported by ConsenSys, both in open community channels and through commercial SLAs for enterprises. It is written in Java and is Apache 2.0 licensed.

#### Erigon {#erigon}

Erigon, formerly known as Turbo‐Geth, started as a fork of Go Ethereum oriented toward speed and disk‐space efficiency. Erigon is a completely re-architected implementation of Ethereum, currently written in Go but with implementations in other languages in work, e.g. [Akula](https://medium.com/@vorot93/meet-akula-the-fastest-ethereum-implementation-ever-built-58eaca244c39). Erigon's goal is to provide a faster, more modular, and more optimized implementation of Ethereum. It can perform a full archive node sync using around 2TB of disk space, in under 3 days. 

## Consensus clients (formerly 'Eth2' clients) {#consensus-clients}

There are multiple consensus clients (previously known as 'Eth2' clients) to support the [consensus upgrades](/upgrades/beacon-chain/). They are running the Beacon Chain and will provide a proof-of-stake consensus mechanism to execution clients after [The Merge](/upgrades/merge/).

[View consensus clients](/upgrades/get-involved/#clients).

| Client                                                      | Language   | Operating systems     | Networks                              |
| ----------------------------------------------------------- | ---------- | --------------------- | ------------------------------------- |
| [Teku](https://pegasys.tech/teku)                           | Java       | Linux, Windows, macOS | Beacon Chain, Prater                  |
| [Nimbus](https://nimbus.team/)                              | Nim        | Linux, Windows, macOS | Beacon Chain, Prater                  |
| [Lighthouse](https://lighthouse-book.sigmaprime.io/)        | Rust       | Linux, Windows, macOS | Beacon Chain, Prater, Pyrmont         |
| [Lodestar](https://lodestar.chainsafe.io/)                  | TypeScript | Linux, Windows, macOS | Beacon Chain, Prater                  |
| [Prysm](https://docs.prylabs.network/docs/getting-started/) | Go         | Linux, Windows, macOS | Beacon Chain, Gnosis, Prater, Pyrmont |

### Teku

Teku is a consensus client implementation written in Java under Apache-2.0 license.

### Nimbus

Nimbus is a consensus client implementation written in Nim under Apache-2.0 license.

### Lighthouse

Lighthouse is a consensus client implementation written in Rust under Apache-2.0 license.

### Lodestar

Lodestar is a consensus client implementation written in Typescript under LGPL-3.0 license 

### Prysm

Prysm is a consensus client implementation written in Go under GPL-3.0 license.


### Synchronization modes {#sync-modes}

To follow and verify current data in the network, the Ethereum client needs to sync with the latest network state. This is done by downloading data from peers, cryptographically verifying their integrity, and building a local blockchain database.

Synchronization modes represent different approaches to this process with various trade-offs. Clients also vary in their implementation of sync algorithms. Always refer to the official documentation of your chosen client for specifics on implementation.

#### Full sync {#full-sync}

Full sync downloads all blocks (including headers, transactions, and receipts) and generates the state of the blockchain incrementally by executing every block from genesis.

- Minimizes trust and offers the highest security by verifying every transaction.
- With an increasing number of transactions, it can take days to weeks to process all transactions.

#### Fast sync

Fast sync downloads all blocks (including headers, transactions, and receipts), verifies all headers, downloads the state and verifies it against the headers.

- Relies on the security of the consensus mechanism.
- Synchronization takes only a few hours.

#### Light sync

Light client mode downloads all block headers, block data, and verifies some randomly. Only syncs tip of the chain from the trusted checkpoint.

- Gets only the latest state while relying on trust in developers and consensus mechanism.
- Client ready to use with current network state in a few minutes.

[More on Light clients](https://www.parity.io/blog/what-is-a-light-client/)

#### Snap sync

The latest approach to syncing the client, pioneered by Geth. Using dynamic snapshots served by peers retrieves all the account and storage data without downloading intermediate trie nodes and then reconstructs the Merkle trie locally.

- Fastest sync strategy, currently default in Geth and Nethermind 
- Saves a lot of disk usage and network bandwidth without sacrificing security.

[More on Snap](https://github.com/ethereum/devp2p/blob/master/caps/snap.md)


#### Beam sync

Implemented by Nethermind and Trinity. Works like fast sync but also downloads the data needed to execute latest blocks, which allows you to query the chain within the first few minutes from starting.

- Syncs state first and enables you to query RPC in a few minutes.
- Still in development and not fully reliable, background sync is slowed down and RPC responses might fail.

[More on Beam](https://medium.com/@jason.carver/intro-to-beam-sync-a0fd168be14a)

#### Optimistic sync

Optimistic sync is a post-merge synchronization strategy designed to be opt-in and backwards compatible, allowing execution nodes to sync via established methods. The execution engine can *optimistically* import beacon blocks without fully verifying them, find the latest head, and then start syncing the chain with the above methods. Then, after the execution client has caught up, it will inform the consensus client of the validity of the transactions in the Beacon Chain.

[More on Optimistic sync](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md)

## Further reading {#further-reading}

There is a lot of information about Ethereum clients on the internet. Here are few resources that might be helpful.

- [Ethereum 101 - Part 2 - Understanding Nodes](https://kauri.io/ethereum-101-part-2-understanding-nodes/48d5098292fd4f11b251d1b1814f0bba/a) _– Wil Barnes, 13 February 2019_
- [Running Ethereum Full Nodes: A Guide for the Barely Motivated](https://medium.com/@JustinMLeroux/running-ethereum-full-nodes-a-guide-for-the-barely-motivated-a8a13e7a0d31) _– Justin Leroux, 7 November 2019_
- [Running an Ethereum Node](https://docs.ethhub.io/using-ethereum/running-an-ethereum-node/) _– ETHHub, updated often_
- [Analyzing the hardware requirements to be an Ethereum full validated node](https://medium.com/coinmonks/analyzing-the-hardware-requirements-to-be-an-ethereum-full-validated-node-dc064f167902) _– Albert Palau, 24 September 2018_
- [Running a Hyperledger Besu Node on the Ethereum Mainnet: Benefits, Requirements, and Setup](https://pegasys.tech/running-a-hyperledger-besu-node-on-the-ethereum-mainnet-benefits-requirements-and-setup/) _– Felipe Faraggi, 7 May 2020_

## Related topics {#related-topics}

- [Blocks](/developers/docs/blocks/)
- [Networks](/developers/docs/networks/)

## Related tutorials {#related-tutorials}

- [Running a Node with Geth](/developers/tutorials/run-light-node-geth/) _– How to download, install and run Geth. Covering syncmodes, the JavaScript console, and more._
- [Turn your Raspberry Pi 4 into a validator node just by flashing the MicroSD card – Installation guide](/developers/tutorials/run-node-raspberry-pi/) _– Flash your Raspberry Pi 4, plug in an ethernet cable, connect the SSD disk and power up the device to turn the Raspberry Pi 4 into a full Ethereum node running the execution layer (Mainnet) and / or the consensus layer (Beacon Chain / validator)._
