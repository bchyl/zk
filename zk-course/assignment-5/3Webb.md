# Anchor and VAnchor
Anchor protocol(similar to the mixer which gadget is built to be deployed on Rust-based blockchain protocols)
Instead of proving the membership inside one Merkle tree, proving the inclusion in one of many Merkle trees which can live on many different blockchains, and if the Merkle tree states are synced across chains to make cross-chain anonymous transactions. 

A higher-level overview of how Anchor works:

- computing the leaf commitment(Poseidon hash): `secret (private input)`, `nullifier (private input)` and `chain id (public input)`.
- computing the nullifier hash(Poseidon hash): `nullifier (private input)`
- calculating the root hash using the `calculated leaf` and the `path (private input)`
- checking if the `calculated root` is inside the `set (public input)`(SetGadget).


VAnchor(Variable Anchor): variable deposit amounts
It supports anonymous join-split functionality which allows joining multiple previous deposits into multiple new deposits. It also supports cross-chain transactions. 

Higher-level overview of how VAnchor works:

- calculating the root hashes for each Utxo: `input Utxos` and `corresponding Merkle paths`.
- checking if the root hash of each Utxo is a member of a root set(SetGadget).
- proving the leaf creation from `private inputs` using the output Utxos .
- checking sum of input amounts plus the public amount is equal to the sum of output amounts.

# UTXO scheme works on the VAnchor contract
a variable-denominated shielded pool system extends the shielded pool system into a bridged system and allows for join/split transactions.

It is built on top the `VAnchorBase/AnchorBase/LinkableTree` system which allows it to be linked to other VAnchor contracts through a simple graph-like interface where anchors maintain edges of their neighboring anchors.

It requires users to create and deposit UTXOs for:
- the supported ERC20 asset into the smart contract 
- insert a commitment into the underlying merkle tree of the form: 
  `commitment = Poseidon(chainID, amount, pubKey, blinding)`
The hash input is the UTXO data. 

All deposits/withdrawals are unified under a common `transact` function which requires a zkSNARK proof that the UTXO commitments are well-formed (i.e. that the deposit amount matches the sum of new UTXOs' amounts).
	
Information regarding the commitments:
	- Poseidon is a zkSNARK friendly hash function
	- destinationChainID is the chainId of the destination chain, where the withdrawal
	  is intended to be made
	- Details of the UTXO and hashes are below:
```
	UTXO = { destinationChainID, amount, pubkey, blinding }
	commitment = Poseidon(destinationChainID, amount, pubKey, blinding)
	nullifier = Poseidon(commitment, merklePath, sign(privKey, commitment, merklePath))
```
Commitments adhering to different hash functions and formats will invalidate any attempt at withdrawal.
	
Using the preimage / UTXO of the commitment, users can generate a zkSNARK proof that the UTXO is located in one-of-many VAnchor merkle trees and that the commitment's destination chain id matches the underlying chain id of the VAnchor where the transaction is taking place. The chain id opcode is leveraged to prevent any tampering of this data.
## ref
[contracts/vanchors](https://github.com/webb-tools/protocol-solidity/tree/main/contracts/vanchors)

# how the tornado relayer works for for the deposit
for TornadoContract deposit event, first to get 3 params:
`commitment`、`leaf_index` and `chain_id`.
first two as `value` to insert to db.

## insert value to sled db
`SledStore` is a store that stores the history of events in a Sled-based database.

key is contract address value is tupple for `(commitment, leaf_index)`
`
store.insert_leaves((chain_id, contract.address()), &[value])
`
- `open_tree` for leaves with chaind and constrat addr
- forloop tree leaf nodes, insert element `(id,commitment)` one by one
- 
## insert and get last number
```
store.insert_last_deposit_block_number(
                    (chain_id, contract.address()),
                    log.block_number,
)
```
- `open_tree` for `last_deposit_block_number`
- insert `HistoryStoreKey`
- return old value number or block_number

## ref
[store/sled](zk-course/assignment-5/relayer/src/store/sled.rs)



