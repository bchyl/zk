# comment contract for UpdateState/Deposit/Withdraw/update_verifyProof
[comment contract for UpdateState/Deposit/Withdraw/update_verifyProof](https://github.com/bchyl/RollupNC/commit/ca77f45bef8a0a5c70c0b5b1deaadae689fbca4e)

# ZKSync UT & functionalities
## UT result
must set env such as ZKSYNC_HOME 
```
    Finished test [unoptimized + debuginfo] target(s) in 0.63s
     Running unittests (target/debug/deps/block_revert-6d9078aad31b7e98)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/db_test_macro-d5fa1545160bb447)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/flamegraph_target-7f665c7241a98843)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/key_generator-4d7c5b1bc301d72c)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/loadnext-80139b8dd3c1de49)

running 11 tests
test report_collector::metrics_collector::tests::histogram_item_addition ... ok
test report_collector::metrics_collector::tests::histogram_percentile ... ok
test report_collector::metrics_collector::tests::histogram_window_size ... ok
test report_collector::metrics_collector::tests::histogram_ranges ... ok
test corrupted_tx::tests::bad_zksync_signature ... ok
test corrupted_tx::tests::nonexistent_token ... ok
test corrupted_tx::tests::zero_fee ... ok
test corrupted_tx::tests::not_packable_fee ... ok
test corrupted_tx::tests::bad_eth_signature ... ok
test corrupted_tx::tests::too_big_amount ... ok
test corrupted_tx::tests::not_packable_amount ... ok

test result: ok. 11 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 2.86s

     Running unittests (target/debug/deps/loadnext-28b0c228f9e6944e)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/parse_pub_data-492e7af010498c89)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/remove_proofs-330aad5a5e85120b)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/vlog-3495c9edadc476d7)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests (target/debug/deps/zksync-c595b637c5c9277e)

running 4 tests
test utils::tests::test_private_key_from_seed_too_short ... ok
test utils::tests::test_zero_conversion ... ok
test utils::tests::test_biguint_with_msb_conversion ... ok
test utils::tests::test_biguint_u256_conversion ... ok

test result: ok. 4 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running tests/integration.rs (target/debug/deps/integration-8c44946323cf8f4a)

running 5 tests
test batch_transfer ... ignored
test comprehensive_test ... ignored
test full_exit_test ... ignored
test nft_test ... ignored
test simple_transfer ... ignored

test result: ok. 0 passed; 0 failed; 5 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running tests/unit.rs (target/debug/deps/unit-ce6ebb8518b97412)

running 21 tests
test test_tokens_cache ... ok
test utils_with_vectors::test_fee_packing ... ok
test utils_with_vectors::test_formatting ... ok
test utils_with_vectors::test_token_packing ... ok
test wallet_tests::test_wallet_account_id ... ok
test wallet_tests::test_wallet_address ... ok
test wallet_tests::test_wallet_ethereum ... ok
test primitives_with_vectors::test_signature ... ok
test wallet_tests::test_wallet_account_info ... ok
test wallet_tests::test_wallet_get_balance_committed ... ok
test wallet_tests::test_wallet_get_balance_committed_not_existent ... ok
test wallet_tests::test_wallet_get_balance_verified_not_existent ... ok
test wallet_tests::test_wallet_refresh_tokens ... ok
test wallet_tests::test_wallet_get_balance_verified ... ok
test wallet_tests::test_wallet_is_signing_key_set ... ok
test signatures_with_vectors::test_mint_nft_signature ... ok
test signatures_with_vectors::test_withdraw_nft_signature ... ok
test signatures_with_vectors::test_forced_exit_signature ... ok
test signatures_with_vectors::test_transfer_signature ... ok
test signatures_with_vectors::test_change_pubkey_signature ... ok
test signatures_with_vectors::test_withdraw_signature ... ok

test result: ok. 21 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 2.78s

     Running unittests (target/debug/deps/zksync_api-e3422987604edba0)

running 42 tests
test api_server::rest::forced_exit_requests::v01::tests::test_disabled_forced_exit_requests ... ignored
test api_server::rest::forced_exit_requests::v01::tests::test_forced_exit_requests_get_fee ... ignored
test api_server::rest::forced_exit_requests::v01::tests::test_forced_exit_requests_submit ... ignored
test api_server::rest::forced_exit_requests::v01::tests::test_forced_exit_requests_wrong_tokens_number ... ignored
test api_server::rest::v02::account::tests::accounts_scope ... ignored
test api_server::rest::v02::block::tests::blocks_scope ... ignored
test api_server::rest::v02::config::tests::config_scope ... ignored
test api_server::rest::v02::fee::tests::fee_scope ... ignored
test api_server::rest::v02::status::tests::status_scope ... ignored
test api_server::rest::v02::token::tests::tokens_scope ... ignored
test api_server::rest::v02::transaction::tests::transactions_scope ... ignored
test api_server::web3::tests::block_number ... ignored
test api_server::web3::tests::create_logs ... ignored
test api_server::web3::tests::erc20_calls ... ignored
test api_server::web3::tests::erc721_calls ... ignored
test api_server::web3::tests::get_balance ... ignored
test api_server::web3::tests::get_block ... ignored
test api_server::web3::tests::get_block_transaction_count ... ignored
test api_server::web3::tests::get_logs ... ignored
test api_server::web3::tests::get_transaction_by_hash ... ignored
test api_server::web3::tests::get_transaction_receipt ... ignored
test api_server::web3::tests::ipfs_cid ... ignored
test api_server::web3::tests::static_methods ... ignored
test fee_ticker::tests::test_error_api ... ignored
test fee_ticker::tests::test_error_coingecko_api ... ignored
test api_server::tx_sender::tests::test_scaling_user_fee_by_two ... ok
test api_server::rpc_server::test::tx_fee_type_serialization ... ok
test api_server::rpc_server::ip_insert_middleware::tests::insert_ip_test ... ok
test api_server::rpc_server::ip_insert_middleware::tests::prevent_user_from_overriding_metadata ... ok
test api_server::tx_sender::tests::test_scaling_user_fee_by_one_cent ... ok
test api_server::tx_sender::tests::test_scaling_user_fee_by_5_percent ... ok
test api_server::rpc_server::ip_insert_middleware::tests::insert_ip_incorrect_call_test ... ok
test fee_ticker::validator::tests::get_real_token_amount ... ignored
test fee_ticker::ticker_api::coinmarkercap::test::parse_coinmarket_cap_responce ... ok
test fee_ticker::validator::tests::check_tokens ... ok
test fee_ticker::tests::test_zero_price_token_fee ... ok
test eth_checker::tests::actual_data_check ... ok
test fee_ticker::tests::test_ticker_subsidy ... ok
test fee_ticker::tests::test_ticker_formula ... ok
```
## maintains transactions memory pool and commits new blocks
code: `core/bin/zksync_core/src/mempool` and `core/bin/zksync_core/src/committer`

### `MempoolState` structure

```
// MempoolState: two acct maps(addr-acctid-nonce) and txQueue
struct MempoolState {
    // account and last committed nonce
    account_nonces: HashMap<Address, Nonce>,
    account_ids: HashMap<AccountId, Address>,
    transactions_queue: MempoolTransactionsQueue,  // VecDeque RevertedTxVariant/SignedTxVariant/PriorityOp and BinaryHeap MempoolPendingTransaction
}
```

### `MempoolTransactionsHandlerBuilder`  handler MempoolTransactionRequest request to update  tx mempool state

```
struct MempoolTransactionsHandlerBuilder {
    db_pool: ConnectionPool,
    mempool_state: Arc<RwLock<MempoolState>>,
    max_block_size_chunks: usize,
}
```
request type and corresponding hander:
- NewTx: call `add_tx` to StorageProcessor insert_tx and write mempool_state
- NewTxsBatch: with params eth_signatures call `add_batch` to StorageProcessor insert_batch and write mempool_state
- NewPriorityOps: call `add_priority_ops` to OperationsSchema retain, then insert_priority_ops to update, last and write mempool_state


### `MempoolBlocksHandler`  handler block mempool request to update  tx mempool state
```
struct MempoolBlocksHandler {
    mempool_state: Arc<RwLock<MempoolState>>,
    requests: mpsc::Receiver<MempoolBlocksRequest>,
    max_block_size_chunks: usize,
}
```
request type and corresponding hander:
- GetBlock: propose_new_block, then send the proposed block to the request initiator
- UpdateNonces: foreach updates list,type Create/Delete/UpdateBalance/ChangePubKeyHash to update MempoolState map

### committer: `handle_new_commit_task`
- SealIncompleteBlock: seal_incomplete_block: get storage -> transaction StorageProcessor -> block_transactions total_priority_ops -> get state_schema to commit_state_update -> get block_schema to save_incomplete_block -> save_block_metadata -> StorageProcessor commit

- PendingBlock:  get storage -> transaction StorageProcessor -> get block_schema to save_pending_block -> get state_schema to commit_state_update -> StorageProcessor commit

- FinishBlock:  get storage -> transaction StorageProcessor -> get block_schema to get_data_to_complete_block/finish_incomplete_block -> StorageProcessor commit

- RemoveRevertedBlock: get mempool_schema to remove_reverted_block

### `poll_for_new_proofs_task`

ticker to access_storage aggregatting committer:
- create_aggregated_commits_storage
- create_aggregated_prover_task_storage
- create_aggregated_publish_proof_operation_storage
- create_aggregated_execute_operation_storage

## eth_sender finalizes the blocks by sending corresponding Ethereum transactions to the L1 smart contract
code: `core/bin/zksync_eth_sender`

structure

```
struct ETHSender<DB: DatabaseInterface> {
    /// Ongoing operations queue.
    ongoing_ops: VecDeque<ETHOperation>,
    /// Connection to the database.
    db: DB,
    /// Ethereum intermediator.
    ethereum: EthereumGateway,
    /// Queue for ordered transaction processing.
    tx_queue: TxQueue,
    /// Utility for managing the gas price for transactions.
    gas_adjuster: GasAdjuster<DB>,
    /// Settings for the `ETHSender`.
    options: ETHSenderConfig,
}
```
construct
```
pub async fn new(options: ETHSenderConfig, db: DB, ethereum: EthereumGateway) -> Self
```
Main routine:
- in forloop loading routine every X seconds 
```
    /// Gets the incoming operations from the database and adds them to the
    /// transactions queue.
    async fn load_new_operations(&mut self) -> anyhow::Result<()> {
```

- proceed txs
```  
   /// 1. Pops all the available transactions from the `TxQueue` and sends them.
    /// 2. Sifts all the ongoing operations, filtering the completed ones and
    ///   managing the rest (e.g. by sending a supplement txs for stuck operations).
    ///
    /// It returns ethereum block number for which these things were done.
async fn proceed_next_operations(&mut self, last_used_block: u64) -> u64
```

```
        // In `perform_commitment_step` we request the states of transactions. We have requested
        // states for the block with number `last_used_block` on the previous call of
        // `proceed_next_operations`, so if the block number hasn't changed we shouldn't request
        // states because it would be spare requests.
        // The ongoing operations list would be the same for the next step
```

update gas
``` 
    /// Performs an actualization routine for `GasAdjuster`:
    /// This method is intended to be invoked periodically, and it updates the
    /// current max gas price limit according to the configurable update interval.
    pub async fn keep_updated(&mut self, ethereum: &EthereumGateway, db: &DB) {
```

## creates input data required for provers to prove blocks
code: `core/bin/zksync_witness_generator`

```
pub fn run_prover_server<DB: DatabaseInterface>(
    database: DB,
    prover_api_opts: ProverApiConfig,
    prover_opts: ProverConfig,
) -> JoinHandle<()> 
```

`WitnessGenerator` structure and maintain
```
/// This will generate and store in db witnesses for blocks with indexes
/// start_block, start_block + block_step, start_block + 2*block_step, ...
pub struct WitnessGenerator<DB: DatabaseInterface> {
    /// Connection to the database.
    database: DB,
    /// Routine refresh interval.
    rounds_interval: time::Duration,

    start_block: BlockNumber,
    block_step: BlockNumber,
}
```

block_on for `maintain` always

```
    /// Starts the thread running `maintain` method.
    pub fn start(self, panic_notify: mpsc::Sender<bool>) {
        ...
                runtime.block_on(async move {
                    self.maintain().await;
                });
        ...
    }
```

`maintain`
```
/// Updates witness data in database in an infinite loop

async fn maintain(self) {
...
    /// Returns status of witness for block with index block_number
    async fn should_work_on_block(
        &self,
        block_number: BlockNumber,
    ) -> Result<BlockInfo, anyhow::Error>
...
    /// Returns next block for generating witness
    fn next_witness_block(
        current_block: BlockNumber,
        block_step: BlockNumber,
        block_info: &BlockInfo,
    ) -> BlockNumber 
...
async fn prepare_witness_and_save_it(&self, block: Block) -> anyhow::Result<()> 
...
}
```
`prepare_witness_and_save_it`
- load_account_tree: database.load_account_tree_cache -> database.load_committed_state -> circuit_account_tree.insert -> load_state_diff -> updated_accounts -> circuit_account_tree.insert -> database.store_account_tree_cache
- build_block_witness: WitnessBuilder -> foreach `ZkSyncOp` , applies the operation to the Circuit account tree
```
pub enum ZkSyncOp {
    Deposit(Box<DepositOp>),
    Transfer(Box<TransferOp>),
    /// Transfer to new operation is represented by `Transfer` transaction,
    /// same as `Transfer` operation. The difference is that for `TransferToNew` operation
    /// recipient account doesn't exist and has to be created.
    TransferToNew(Box<TransferToNewOp>),
    Withdraw(Box<WithdrawOp>),
    WithdrawNFT(Box<WithdrawNFTOp>),
    #[doc(hidden)]
    Close(Box<CloseOp>),
    FullExit(Box<FullExitOp>),
    ChangePubKeyOffchain(Box<ChangePubKeyOp>),
    ForcedExit(Box<ForcedExitOp>),
    MintNFTOp(Box<MintNFTOp>),
    /// `NoOp` operation cannot be directly created, but it's used to fill the block capacity.
    Noop(NoopOp),
    Swap(Box<SwapOp>),
}

   /// Applies the operation to the Circuit account tree, generating the witness data.
    fn apply_tx(tree: &mut CircuitAccountTree, op: &Self::OperationType) -> Self;
```
- store_witness to db

# ZKPorter
zkPorter: off-chain data availability(fee-sensitive)
zkRollup: on-chain data availability(all data available on-chain)

Support for any number of shards, each with its own data availability scheme (defined by the smart contract for that shard). Meanwhile, The choice of The shard is controlled at The inpidual account level.

contracts and accounts on the zkRollup side will be able to seamlessly interact with accounts on the zkPorter side, and vice versa.

zkPorter accounts will be secured by zkSync token holders, termed Guardians. 

They will keep track of state on the zkPorter side by signing blocks to confirm data availability of zkPorter accounts. Guardians participate in proof of stake (PoS) with the zkSync token, so any failure of data availability will cause them to get slashed. 

This gives cryptoeconomic guarantees of the data availability.

## zkPorter vs optimistic rollups
stronger security guarantees(attack costs and gains)

## zkPorter vs Vadium(Volition)
Vadium is a solution that differs from Rollup in that it puts the transaction data in the chain and uses zero-knowledge proof to package the off-chain transactions together. Moreover, the zero-knowledge proof scheme they use is called STARKs. Vadium is a compromise solution that improves throughput at the expense of decentralization and security.

Volition is an architecture that L2 can use (pioneered by Starkware), where users choose validium or ZK-rollup for each transaction.

In Volition scheme, users can select data storage method based on each transaction, while in zkPorter scheme, users can select transaction settlement method based on each account (zkPorter account can only generate transactions through off-chain DA). 

In addition, zkPorter's off-chain DA system is more decentralized, as its DA is secured by a Guardian network powered by zkSync native tokens, rather than a centralized DAC.

