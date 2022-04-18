# bridge process works 

## Ethereum =>  Harmony

- Harmony: Bridge smart contract, Ethereum Light Client (ELC) smart contract, Ethereum Verifier (EVerifier) smart contract  
- Ethereum Relayer: relays every Ethereum header information to ELC
- Ethereum Prover (EProver): Ethereum full node or a client that has access to a full node

## Harmony => Ethereum

- Ethereum: Bridge smart contract, Harmony Light Client (HLC) smart contract, Harmony Verifier (HVerifier) smart contract
- Harmony Relayer: relays every checkpoint block header information to HLC 
- Harmony Prover (HProver): Harmony full node or a client that has access to a full node

## Ethereum to Harmony asset transfer
- User locks ERC20 on Ethereum by transferring to bridge smart contract and obtains the hash of this transaction from blockchain
- User sends the hash to EProver and receives proof-of-lock
- User sends the proof-of-lock to bridge smart contract on Harmony
- Bridge smart contract on Harmony invokes ELC and EVerifier to verify the proof-of-lock and mints HRC20 (equivalent amount)

## Harmony to Ethereum asset redeem
- User burns HRC20 on Harmony using Bridge smart contract and obtains the hash of this transaction from blockchain
- User sends the hash to HProver and receives proof-of-burn
- User sends the proof-of-burn to bridge smart contract on Ethereum
- Bridge smart contract on Ethereum invokes HLC and HVerifier to verify the proof-of-burn and unlocks ERC20 (equivalent amount)

## ref
[horizon readme](https://github.com/harmony-one/horizon/blob/main/README.md)

## comments contract code 
[little comments](https://github.com/bchyl/horizon/commit/4c57e5c225633d54962b88fdd1eee89d6b59d24a)

## MMR root
checkpoint block for its header contains an MMR root calculated over all block headers added to the chain since the previous checkpoint block. 

Therefore, epoch blocks are considered checkpoint blocks.

Updating requires O(log(N)) values along the right spine of the trie, and any binary interval range corresponds to an internal node of the trie.

## ps
groth16 -> plonk as final-project ok?
prover
```
let mut transitions = (start_block..=opts.end_block)
        .step_by(epoch_duration)
        .enumerate()
        .map|(i, num)|{...});
let first_epoch = transitions.remove(0).block;
let parameters = Parameters {
        epochs: epoch_proving_key,
        hash_to_bits: hash_to_bits_proving_key,
    };

proof = prove(&parameters, num_validators, &first_epoch, &transitions)
```

verifier
```
export function verify(proof_data_val) {
    const retptr = wasm.__wbindgen_add_to_stack_pointer(-16)
    wasm.verify(retptr, addBorrowedObject(proof_data_val))
    var r0 = getInt32Memory0()[retptr / 4 + 0]
    var r1 = getInt32Memory0()[retptr / 4 + 1]
    var r2 = getInt32Memory0()[retptr / 4 + 2]
}

export function block_verify(block_data_val) {
  try {
    const retptr = wasm.__wbindgen_add_to_stack_pointer(-16)
    wasm.block_verify(retptr, addBorrowedObject(block_data_val))
    var r0 = getInt32Memory0()[retptr / 4 + 0]
    var r1 = getInt32Memory0()[retptr / 4 + 1]
}
```
### ref
plumo [prover](https://github.com/celo-org/plumo-prover) and [verifier](https://github.com/celo-org/plumo-verifier)


