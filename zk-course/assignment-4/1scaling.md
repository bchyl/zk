# scalable solutions
## solutions comparison
comparison table please see
[scaling Solutios](https://www.yuque.com/docs/share/5989be87-4117-4765-993a-f9ab723c2bb2?)

note: Eth2.0 is not currently included in the comparison

## security: biggest problem with the Plasma

Need to periodically watch the network (liveness requirement) or delegate this responsibility to someone else to ensure the security of your funds.

## ref
[eth scaling](https://ethereum.org/en/developers/docs/scaling/)

# zk-rollups
## key features
- all funds are held by a smart contract on the main chain,
- it performs computation and storage off-chain，
- validity of the side chains is ensured by zero-knowledge proofs

replace fraud challenges in op-rollups with zero-knowledge proofs.
[zk-rollup architecture](https://miro.medium.com/max/1400/0*Gt0cuKzQMkK5VC1O)
### Merkle Trees
[Accounts and balances MT](https://miro.medium.com/max/1400/0*HYu6KMtXVNgUHqEb)
Accounts and balances are represented by separate Merkle Trees.
roots are both stored in a smart contract on Ethereum which provides a succinct representation of the state of the sidechain. All other data is stored off-chain.
### builds blocks and state updates
[zk-rollups package](https://miro.medium.com/max/1400/1*WH5HqEzie8PZqNbW1JEx3A.png)
The change in state is hashed, and this is an input of a SNARK (a type of zero-knowledge proof) which includes proof of validity for each transaction in a Rollup block.
### aggregated transactions
tx aggregated together, signed(a zero-knowledge proof known as ZK-SNARK) for and committed to the main chain with just the header. 
### CALLDATA
two Merkle roots of the address book and balances/nonces on-chain 

## benefits or shortcomings
see the above comparison table.
More comparisons below：
### benefits
- Faster transaction finality time
- Increased throughput and scalability
- Cheaper transaction fees
- Faster withdrawal times
- Decentralized 
- secure
### shortcomings
- Complex validity proofing: difficult and time-consuming
- Trust: honest to validate rollup data
- Smart contract support: EVM and Solidity compatible, but zkevm can 
### zk-rollups vs op-rollups
free up the mainnet by computing trasactions off-chain, on L1.

While op-rollups utilize fraud proofs to confirm credibility of trasactions to be written to the mainnet.

ZK-rollups employ validity proofs to quickly verify transactions batches.
## ref
[ZK-Rollups Guide](https://learn.bybit.com/blockchain/zk-rollups-eth-scalability/)
[Scaling Ethereum on L2](https://medium.com/interdax/ethereum-l2-optimistic-and-zk-rollups-dffa58870c93)
[snark-circuit](https://miro.medium.com/max/1400/1*A9lxrYaX9RWgU8qJv8KLqQ.png)
[How Zk-Rollups Work](https://medium.com/fcats-blockchain-incubator/how-zk-rollups-work-8ac4d7155b0e)
[Scroll: a Layer-2 ecosystem based on zk-Rollup](https://scroll-official.medium.com/scroll-a-layer-2-ecosystem-based-on-zk-rollup-186ff0d764c)
# stateless
## concept of stateless client
allows the creation of light nodes: contains only the chain of headers(see blockHeader structure) without the execution of any transactions or associated states.

fully stateless since it will hold zero information regarding state. Over time it will soak up the state as transactions touch upon it, putting together a more complete state with every change of state presented.
### ref
[The Stateless Client Concept](https://ethresear.ch/t/the-stateless-client-concept/172)
[Stateless Clients](https://docs.ethhub.io/ethereum-roadmap/ethereum-2.0/stateless-clients/)
## solve state bottleneck
never keep a copy of state, only grab the latest transactions together with the witness, and have everything it needs to execute the next block.

Partial-state nodes could keep a full state for just a short number of blocks.
Zero-state nodes running as light as possible, could rely entirely on witnesses to verify new blocks.

lower memory usage, disk, and I/O but higher bandwidth usage.
### ref
[The 1.x Files: The State of Stateless Ethereum](https://blog.ethereum.org/2019/12/30/eth1x-files-state-of-stateless-ethereum/)
## Zero-Knowledge improves
time and space inefficient for ultra light weight clients: cannot afford to download all epoch blocks (1 block per day)

ZK-based lightclient fast and cheap: Prover (validator/full nodes): generates a proof of committee transition
- inputs: [Epoch numbers, BLS public keys]
- perform epoch block: aggregate and check
- proof/aggregate proof(or recursively)
- output: Proof that includes the transition(epoch committee)
### ref
[Zero knowledge Proofs for Rollups & Stateless Clients](https://harmonyone.notion.site/Zero-knowledge-Proofs-for-Rollups-Stateless-Clients-648485e74d3f4e389e629e5656c9a759)



