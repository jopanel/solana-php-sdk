## 🛠 Usage

To get started, include the Composer autoloader and instantiate the `SolanaRPC` and endpoint classes.

```php
require_once 'vendor/autoload.php';

use JosephOpanel\SolanaSDK\SolanaRPC;
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Transaction;

$rpc = new SolanaRPC();
$account = new Account($rpc);
$block = new Block($rpc);
$transaction = new Transaction($rpc);
```

### Methods

## `getAccountInfo` <a name="getaccountinfo"></a>

### What is `getAccountInfo`?

The `getAccountInfo` RPC method retrieves detailed information about an account, including its data, owner, lamport balance, and more.

- **RPC Method:** `getAccountInfo`
- **Parameters:**
  - **`account`** (string): The public key of the account to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `lamports` (integer): The balance of the account in lamports.
  - `owner` (string): The public key of the program that owns the account.
  - `data` (mixed): The raw data stored in the account.
  - `executable` (boolean): Whether the account is executable.
  - `rentEpoch` (integer): The next epoch in which rent is due.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get information about an account
$accountInfo = $account->getAccountInfo('4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg');
print_r($accountInfo);

```

---

## `getBalance` <a name="getbalance"></a>

### What is `getBalance`?

The `getBalance` RPC method retrieves the balance of an account in lamports. Lamports are the smallest unit of SOL, where 1 SOL equals 1 billion lamports.

- **RPC Method:** `getBalance`
- **Parameters:**
  - **`account`** (string): The public key of the account to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - Returns the account balance in lamports as an integer.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get the balance of an account
$balance = $account->getBalance('4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg');
print_r($balance);
```

---

## `getBlock` <a name="getblock"></a>

### What is `getBlock`?

The `getBlock` RPC method retrieves details about a specific block, including its transactions, signatures, and parent information.

- **RPC Method:** `getBlock`
- **Parameters:**
  - **`slot`** (integer): The slot number of the block to fetch.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the transaction data encoding (e.g., `json`, `base64`, `jsonParsed`).
    - `transactionDetails` (string): Specifies the level of transaction details (`full`, `signatures`, or `none`).
    - `rewards` (boolean): Whether to include rewards in the response.

- **Response:**
  - `blockhash` (string): The hash of the block.
  - `parentSlot` (integer): The slot of the block's parent.
  - `transactions` (array): Details of the transactions included in the block.
  - `rewards` (array): Details of the staking rewards in the block, if applicable.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get details about a specific block
$blockDetails = $block->getBlock(123456, [
    'commitment' => 'finalized',
    'encoding' => 'json',
    'transactionDetails' => 'full',
    'rewards' => true,
]);
print_r($blockDetails);
```

----

## `getBlockCommitment` <a name="getblockcommitment"></a>

### What is `getBlockCommitment`?

The `getBlockCommitment` RPC method retrieves the commitment levels for a given block. This provides information on how many votes each commitment level has received for the block.

- **RPC Method:** `getBlockCommitment`
- **Parameters:**
  - **`block`** (integer): The slot number of the block whose commitment is being fetched.

- **Response:**
  - `totalStake` (integer): The total stake in lamports committed to the block.
  - `commitment` (array): An array of integers representing the stake weight at each level of commitment.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get the commitment for a specific block
$blockCommitment = $block->getBlockCommitment(123456);
print_r($blockCommitment);
```

---

#### `getBlockHeight`

Retrieve the current block height of the Solana blockchain.

```php
$blockHeight = $block->getBlockHeight();
print_r($blockHeight);
```

---

## `getBlockProduction` <a name="getblockproduction"></a>

### What is `getBlockProduction`?

The `getBlockProduction` RPC method retrieves block production information from the current or a specified epoch. This provides insight into the leader schedule and the number of blocks produced by each validator.

- **RPC Method:** `getBlockProduction`
- **Parameters:**
  - **Optional:**
    - `config` (array):
      - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
      - `range` (object):
        - `firstSlot` (integer): The first slot in the block production range.
        - `lastSlot` (integer): The last slot in the block production range.
      - `identity` (string): The validator's public key to filter the results.

- **Response:**
  - `byIdentity` (object): A mapping of validator identities to the number of leader slots they were assigned and the number of blocks they produced.
  - `range` (object): The slot range for which block production information was retrieved.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get block production information for a specific range
$blockProduction = $block->getBlockProduction([
    'commitment' => 'finalized',
    'range' => ['firstSlot' => 123400, 'lastSlot' => 123500],
]);
print_r($blockProduction);
```

---

## `getBlockTime` <a name="getblocktime"></a>

### What is `getBlockTime`?

The `getBlockTime` RPC method retrieves the estimated Unix timestamp at which a block was produced. This is useful for understanding the timing of specific blocks.

- **RPC Method:** `getBlockTime`
- **Parameters:**
  - **`block`** (integer): The slot number of the block whose timestamp is being fetched.

- **Response:**
  - Returns the estimated block time as a Unix timestamp (integer).

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get the estimated time for a specific block
$blockTime = $block->getBlockTime(123456);
print_r($blockTime);
```

---

## `getBlocks` <a name="getblocks"></a>

### What is `getBlocks`?

The `getBlocks` RPC method retrieves a list of confirmed blocks between two slot numbers. This is useful for iterating over blocks within a range.

- **RPC Method:** `getBlocks`
- **Parameters:**
  - **`startSlot`** (integer): The starting slot of the block range.
  - **Optional:**
    - `endSlot` (integer): The ending slot of the block range.
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - Returns an array of block slot numbers within the specified range.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get a list of confirmed blocks between two slots
$blocks = $block->getBlocks(123400, 123500);
print_r($blocks);
```

---

## `getBlocksWithLimit` <a name="getblockswithlimit"></a>

### What is `getBlocksWithLimit`?

The `getBlocksWithLimit` RPC method retrieves a list of confirmed blocks starting from a given slot up to a specified limit. This is useful for fetching a specific number of blocks starting from a particular slot.

- **RPC Method:** `getBlocksWithLimit`
- **Parameters:**
  - **`startSlot`** (integer): The starting slot of the block range.
  - **`limit`** (integer): The maximum number of blocks to retrieve.

- **Response:**
  - Returns an array of block slot numbers within the specified range, up to the limit.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get a limited number of confirmed blocks starting from a specific slot
$blocksWithLimit = $block->getBlocksWithLimit(123400, 10);
print_r($blocksWithLimit);
```

---

## `getClusterNodes` <a name="getclusternodes"></a>

### What is `getClusterNodes`?

The `getClusterNodes` RPC method retrieves information about all nodes participating in the Solana cluster. This includes details about each node's public key, RPC address, gossip address, and software version.

- **RPC Method:** `getClusterNodes`
- **Parameters:** None.

- **Response:**
  - Returns an array of objects where each object contains:
    - `pubkey` (string): The public key of the node.
    - `gossip` (string): The gossip network address of the node.
    - `tpu` (string): The Transaction Processing Unit (TPU) address of the node.
    - `rpc` (string | null): The RPC address of the node, if available.
    - `version` (string | null): The software version of the node, if available.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get a list of all nodes in the Solana cluster
$clusterNodes = $system->getClusterNodes();
print_r($clusterNodes);
```

---

## `getEpochInfo` <a name="getepochinfo"></a>

### What is `getEpochInfo`?

The `getEpochInfo` RPC method retrieves information about the current epoch in the Solana blockchain. Epochs are periods in which validators rotate their roles, and this method provides details about the current slot, epoch progress, and other related metrics.

- **RPC Method:** `getEpochInfo`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `epoch` (integer): The current epoch number.
  - `slotIndex` (integer): The slot's index within the current epoch.
  - `slotsInEpoch` (integer): The total number of slots in the current epoch.
  - `absoluteSlot` (integer): The absolute slot number since the genesis block.
  - `blockHeight` (integer): The current block height.
  - `transactionCount` (integer | null): The total number of processed transactions (optional).

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get information about the current epoch
$epochInfo = $system->getEpochInfo('finalized');
print_r($epochInfo);
```

---

## `getEpochSchedule` <a name="getepochschedule"></a>

### What is `getEpochSchedule`?

The `getEpochSchedule` RPC method retrieves details about the epoch schedule of the Solana cluster. The epoch schedule defines how epochs are structured, including the number of slots per epoch, the warm-up period for stake activation, and the first normal epoch.

- **RPC Method:** `getEpochSchedule`
- **Parameters:** None.

- **Response:**
  - `slotsPerEpoch` (integer): The number of slots in each epoch.
  - `leaderScheduleSlotOffset` (integer): The number of slots before a leader schedule is computed.
  - `warmup` (boolean): Whether stake warm-up is enabled.
  - `firstNormalEpoch` (integer): The first epoch without stake warm-up.
  - `firstNormalSlot` (integer): The slot number of the first normal epoch.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get details about the epoch schedule
$epochSchedule = $system->getEpochSchedule();
print_r($epochSchedule);
```

---

## `getFeeForMessage` <a name="getfeeformessage"></a>

### What is `getFeeForMessage`?

The `getFeeForMessage` RPC method calculates the fee required to process a specific transaction message. This is useful for determining the cost of submitting a transaction to the Solana network.

- **RPC Method:** `getFeeForMessage`
- **Parameters:**
  - **`message`** (string): A base64-encoded transaction message to calculate the fee for.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `value` (integer | null): The fee in lamports required for the transaction. Returns `null` if the message cannot be processed.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Calculate the fee for a given transaction message
$fee = $system->getFeeForMessage('base64EncodedMessage', 'finalized');
print_r($fee);
```

---

## `getFirstAvailableBlock` <a name="getfirstavailableblock"></a>

### What is `getFirstAvailableBlock`?

The `getFirstAvailableBlock` RPC method retrieves the slot number of the first available block in the ledger. This is useful for determining the starting point of stored block data on the current node.

- **RPC Method:** `getFirstAvailableBlock`
- **Parameters:** None.

- **Response:**
  - `slot` (integer): The slot number of the first available block.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get the first available block in the ledger
$firstAvailableBlock = $block->getFirstAvailableBlock();
print_r($firstAvailableBlock);
```

---

## `getGenesisHash` <a name="getgenesishash"></a>

### What is `getGenesisHash`?

The `getGenesisHash` RPC method retrieves the genesis hash of the cluster. The genesis hash is a unique identifier for the genesis block of a Solana network and can be used to distinguish between different networks (e.g., mainnet-beta, testnet, devnet).

- **RPC Method:** `getGenesisHash`
- **Parameters:** None.

- **Response:**
  - `genesisHash` (string): The genesis hash of the cluster.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get the genesis hash of the cluster
$genesisHash = $block->getGenesisHash();
print_r($genesisHash);

```

---

## `getHealth` <a name="gethealth"></a>

### What is `getHealth`?

The `getHealth` RPC method checks the health of the node. It can be used to determine whether the node is operational and in sync with the cluster.

- **RPC Method:** `getHealth`
- **Parameters:** None.

- **Response:**
  - `"ok"` (string): Indicates the node is healthy and operational.
  - Other values may indicate the node is unhealthy or not in sync.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Check the health of the node
$healthStatus = $system->getHealth();
print_r($healthStatus);
```

---

## `getHighestSnapshotSlot` <a name="gethighestsnapshotslot"></a>

### What is `getHighestSnapshotSlot`?

The `getHighestSnapshotSlot` RPC method retrieves the highest slot for which a snapshot is available on the node. This is useful for understanding the state of snapshots used for validator restart or catching up with the cluster.

- **RPC Method:** `getHighestSnapshotSlot`
- **Parameters:** None.

- **Response:**
  - `full` (integer): The highest slot for a full snapshot.
  - `incremental` (integer | null): The highest slot for an incremental snapshot, if available.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the highest snapshot slot available
$highestSnapshotSlot = $system->getHighestSnapshotSlot();
print_r($highestSnapshotSlot);
```

---

## `getIdentity` <a name="getidentity"></a>

### What is `getIdentity`?

The `getIdentity` RPC method retrieves the identity of the node. The identity is represented by the node's public key and is useful for distinguishing between nodes in the cluster.

- **RPC Method:** `getIdentity`
- **Parameters:** None.

- **Response:**
  - `identity` (string): The public key of the node.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the identity of the node
$nodeIdentity = $system->getIdentity();
print_r($nodeIdentity);
```

---

## `getInflationGovernor` <a name="getinflationgovernor"></a>

### What is `getInflationGovernor`?

The `getInflationGovernor` RPC method retrieves the current inflation governor settings for the Solana cluster. These settings determine the cluster’s inflation schedule, including base rates and terminal rates.

- **RPC Method:** `getInflationGovernor`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `initial` (float): The initial inflation rate.
  - `terminal` (float): The terminal inflation rate.
  - `taper` (float): The rate at which inflation tapers.
  - `foundation` (float): The percentage allocated to the foundation.
  - `foundationTerm` (float): The duration of foundation rewards.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the inflation governor settings
$inflationGovernor = $system->getInflationGovernor('finalized');
print_r($inflationGovernor);
```

---

## `getInflationRate` <a name="getinflationrate"></a>

### What is `getInflationRate`?

The `getInflationRate` RPC method retrieves the current inflation rate for the Solana cluster. This method provides insight into the rate of inflation for a given epoch, which impacts staking rewards and token supply.

- **RPC Method:** `getInflationRate`
- **Parameters:** None.

- **Response:**
  - `total` (float): The total annualized inflation rate.
  - `validator` (float): The portion of inflation allocated to validators.
  - `foundation` (float): The portion of inflation allocated to the foundation.
  - `epoch` (integer): The epoch for which the inflation rate applies.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the current inflation rate
$inflationRate = $system->getInflationRate();
print_r($inflationRate);
```

---

## `getInflationReward` <a name="getinflationreward"></a>

### What is `getInflationReward`?

The `getInflationReward` RPC method retrieves the inflation rewards earned by a list of accounts for a specified epoch. This is useful for understanding staking rewards distribution.

- **RPC Method:** `getInflationReward`
- **Parameters:**
  - **`addresses`** (array): An array of account public keys to query.
  - **Optional:**
    - `epoch` (integer): The epoch for which to fetch rewards. Defaults to the most recent epoch with rewards available.
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - An array of objects, where each object contains:
    - `amount` (integer): The reward amount in lamports.
    - `epoch` (integer): The epoch in which the reward was issued.
    - `effectiveSlot` (integer): The slot at which the reward was effective.
    - `postBalance` (integer): The balance of the account after the reward was applied.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get inflation rewards for a list of accounts
$rewards = $system->getInflationReward(['4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg'], ['epoch' => 200]);
print_r($rewards);
```

---

## `getLargestAccounts` <a name="getlargestaccounts"></a>

### What is `getLargestAccounts`?

The `getLargestAccounts` RPC method retrieves a list of the largest accounts on the Solana cluster by lamport balance. This is useful for identifying high-balance accounts, such as stake accounts or large wallets.

- **RPC Method:** `getLargestAccounts`
- **Parameters:**
  - **Optional:**
    - `filter` (string): Specifies the account type to filter by (e.g., `circulating`, `nonCirculating`).
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - An array of objects, where each object contains:
    - `address` (string): The public key of the account.
    - `lamports` (integer): The lamport balance of the account.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the largest accounts in the cluster
$largestAccounts = $system->getLargestAccounts(['filter' => 'circulating']);
print_r($largestAccounts);
```

---

## `getLatestBlockhash` <a name="getlatestblockhash"></a>

### What is `getLatestBlockhash`?

The `getLatestBlockhash` RPC method retrieves the latest blockhash of the cluster. This blockhash is required for creating and submitting transactions, as it ensures transactions are processed in the correct block order.

- **RPC Method:** `getLatestBlockhash`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `blockhash` (string): The latest blockhash of the cluster.
  - `lastValidBlockHeight` (integer): The last block height at which the blockhash will remain valid.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get the latest blockhash
$latestBlockhash = $block->getLatestBlockhash('finalized');
print_r($latestBlockhash);
```

---

## `getLeaderSchedule` <a name="getleaderschedule"></a>

### What is `getLeaderSchedule`?

The `getLeaderSchedule` RPC method retrieves the leader schedule for a specific slot range or epoch. This schedule shows which validators are responsible for producing blocks during specific slots.

- **RPC Method:** `getLeaderSchedule`
- **Parameters:**
  - **Optional:**
    - `slot` (integer): The slot for which to fetch the leader schedule. If omitted, the current leader schedule is returned.
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `identity` (string): The public key of a validator to filter the results.

- **Response:**
  - A mapping of validator public keys to an array of slots for which they are the leader.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the leader schedule for the current slot
$leaderSchedule = $system->getLeaderSchedule();
print_r($leaderSchedule);

// Get the leader schedule for a specific slot range
$leaderScheduleForSlot = $system->getLeaderSchedule(123400, ['commitment' => 'finalized']);
print_r($leaderScheduleForSlot);
```

---

## `getMaxRetransmitSlot` <a name="getmaxretransmitslot"></a>

### What is `getMaxRetransmitSlot`?

The `getMaxRetransmitSlot` RPC method retrieves the highest slot that the node has retransmitted to its peers. This is useful for understanding how far the node has propagated its data.

- **RPC Method:** `getMaxRetransmitSlot`
- **Parameters:** None.

- **Response:**
  - `slot` (integer): The highest slot that the node has retransmitted.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the maximum retransmit slot
$maxRetransmitSlot = $system->getMaxRetransmitSlot();
print_r($maxRetransmitSlot);
```

---

## `getMaxShredInsertSlot` <a name="getmaxshredinsertslot"></a>

### What is `getMaxShredInsertSlot`?

The `getMaxShredInsertSlot` RPC method retrieves the highest slot for which the node has inserted shreds (data fragments). This is useful for understanding how much data the node has processed from the network.

- **RPC Method:** `getMaxShredInsertSlot`
- **Parameters:** None.

- **Response:**
  - `slot` (integer): The highest slot for which the node has inserted shreds.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the maximum shred insert slot
$maxShredInsertSlot = $system->getMaxShredInsertSlot();
print_r($maxShredInsertSlot);
```

---

## `getMinimumBalanceForRentExemption` <a name="getminimumbalanceforrentexemption"></a>

### What is `getMinimumBalanceForRentExemption`?

The `getMinimumBalanceForRentExemption` RPC method calculates the minimum balance, in lamports, required to exempt an account of a given size from rent. This is useful for ensuring that an account remains rent-exempt.

- **RPC Method:** `getMinimumBalanceForRentExemption`
- **Parameters:**
  - **`dataLength`** (integer): The size of the account's data, in bytes.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `lamports` (integer): The minimum balance required to make the account rent-exempt.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the minimum balance for rent exemption for an account of size 128 bytes
$minimumBalance = $system->getMinimumBalanceForRentExemption(128, 'finalized');
print_r($minimumBalance);
```

---

## `getMultipleAccounts` <a name="getmultipleaccounts"></a>

### What is `getMultipleAccounts`?

The `getMultipleAccounts` RPC method retrieves information about multiple accounts in a single request. This is useful for batching queries to reduce overhead when working with several accounts at once.

- **RPC Method:** `getMultipleAccounts`
- **Parameters:**
  - **`addresses`** (array): An array of account public keys to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the account data encoding (e.g., `base64`, `jsonParsed`).

- **Response:**
  - An array of account objects, where each object contains:
    - `lamports` (integer): The balance of the account in lamports.
    - `owner` (string): The public key of the program that owns the account.
    - `data` (mixed): The raw data stored in the account.
    - `executable` (boolean): Whether the account is executable.
    - `rentEpoch` (integer): The next epoch in which rent is due.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get information about multiple accounts
$accountsInfo = $account->getMultipleAccounts([
    '4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg',
    '5nQyMBaFChGmzfhNZZm9oTNDgNr5LTCfjoJGRhcfAcGQ',
], ['commitment' => 'finalized']);
print_r($accountsInfo);
```

---

## `getProgramAccounts` <a name="getprogramaccounts"></a>

### What is `getProgramAccounts`?

The `getProgramAccounts` RPC method retrieves all accounts owned by a specific program. This is useful for querying accounts managed by a particular on-chain program.

- **RPC Method:** `getProgramAccounts`
- **Parameters:**
  - **`programId`** (string): The public key of the program to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the account data encoding (e.g., `base64`, `jsonParsed`).
    - `filters` (array): An array of filter objects to filter accounts by data or size.

- **Response:**
  - An array of account objects, where each object contains:
    - `pubkey` (string): The public key of the account.
    - `account` (object):
      - `lamports` (integer): The balance of the account in lamports.
      - `owner` (string): The public key of the program that owns the account.
      - `data` (mixed): The raw data stored in the account.
      - `executable` (boolean): Whether the account is executable.
      - `rentEpoch` (integer): The next epoch in which rent is due.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get all accounts owned by a specific program
$programAccounts = $account->getProgramAccounts(
    'TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA',
    ['commitment' => 'finalized', 'encoding' => 'jsonParsed']
);
print_r($programAccounts);
```

---

## `getRecentPerformanceSamples` <a name="getrecentperformancesamples"></a>

### What is `getRecentPerformanceSamples`?

The `getRecentPerformanceSamples` RPC method retrieves performance samples from the Solana cluster for recent slots. This provides insight into cluster performance, including the number of transactions and slots processed over a given period.

- **RPC Method:** `getRecentPerformanceSamples`
- **Parameters:**
  - **Optional:**
    - `limit` (integer): The maximum number of samples to return. Defaults to all available samples.

- **Response:**
  - An array of performance sample objects, where each object contains:
    - `slot` (integer): The slot at which the sample was taken.
    - `numTransactions` (integer): The total number of transactions in the sample.
    - `numSlots` (integer): The total number of slots in the sample.
    - `samplePeriodSecs` (integer): The duration of the sample in seconds.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get recent performance samples
$performanceSamples = $system->getRecentPerformanceSamples(5);
print_r($performanceSamples);
```

---

## `getRecentPrioritizationFees` <a name="getrecentprioritizationfees"></a>

### What is `getRecentPrioritizationFees`?

The `getRecentPrioritizationFees` RPC method retrieves the recent prioritization fees used in the cluster. These fees are part of Solana's fee market and provide information about fees paid to prioritize transactions.

- **RPC Method:** `getRecentPrioritizationFees`
- **Parameters:** None.

- **Response:**
  - An array of objects, where each object contains:
    - `slot` (integer): The slot for which the fee information is provided.
    - `prioritizationFee` (integer): The prioritization fee paid for transactions in this slot.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get recent prioritization fees
$prioritizationFees = $system->getRecentPrioritizationFees();
print_r($prioritizationFees);
```

---

## `getSignatureStatuses` <a name="getsignaturestatuses"></a>

### What is `getSignatureStatuses`?

The `getSignatureStatuses` RPC method retrieves the statuses of given transaction signatures. This is useful for tracking the state of transactions and determining whether they have been confirmed, processed, or finalized.

- **RPC Method:** `getSignatureStatuses`
- **Parameters:**
  - **`signatures`** (array): An array of transaction signatures to query.
  - **Optional:**
    - `searchTransactionHistory` (boolean): If `true`, searches the transaction history. Defaults to `false`.

- **Response:**
  - An array of objects corresponding to each signature, where each object contains:
    - `slot` (integer | null): The slot in which the transaction was processed.
    - `confirmations` (integer | null): The number of confirmations.
    - `status` (object | null): The transaction processing status.
    - `err` (object | null): Any error encountered during transaction processing.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Transaction;

$transaction = new Transaction($rpc);

// Get statuses for specific transaction signatures
$signatureStatuses = $transaction->getSignatureStatuses(
    ['5g3QjG1gN6bRmFcyCn4ufFq3q7xZ5vn9YWfByx2bt2Hv', '2vQPjXQWYmLx24CeKsjoG8t23jWzHufhAf79KfqzH2n3'],
    true
);
print_r($signatureStatuses);
```

---

## `getSignaturesForAddress` <a name="getsignaturesforaddress"></a>

### What is `getSignaturesForAddress`?

The `getSignaturesForAddress` RPC method retrieves confirmed transaction signatures for a given account address. This is useful for tracking transaction history associated with an account.

- **RPC Method:** `getSignaturesForAddress`
- **Parameters:**
  - **`address`** (string): The public key of the account to query.
  - **Optional:**
    - `limit` (integer): The maximum number of signatures to return. Defaults to 1,000.
    - `before` (string): Only returns signatures before the specified transaction signature.
    - `until` (string): Only returns signatures until the specified transaction signature.
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - An array of objects, where each object contains:
    - `signature` (string): The transaction signature.
    - `slot` (integer): The slot in which the transaction was confirmed.
    - `err` (object | null): Any error encountered during transaction processing.
    - `blockTime` (integer | null): The Unix timestamp of the transaction.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Transaction;

$transaction = new Transaction($rpc);

// Get transaction signatures for an address
$signatures = $transaction->getSignaturesForAddress(
    '4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg',
    ['limit' => 10, 'commitment' => 'finalized']
);
print_r($signatures);
```

---

## `getSlot` <a name="getslot"></a>

### What is `getSlot`?

The `getSlot` RPC method retrieves the current slot of the Solana blockchain as processed by the node. This is useful for tracking the node's progress in processing the ledger.

- **RPC Method:** `getSlot`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `slot` (integer): The current slot as processed by the node.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the current slot
$currentSlot = $system->getSlot('finalized');
print_r($currentSlot);
```

---

## `getSlotLeader` <a name="getslotleader"></a>

### What is `getSlotLeader`?

The `getSlotLeader` RPC method retrieves the identity of the current slot leader. The slot leader is the validator responsible for producing blocks during a specific slot.

- **RPC Method:** `getSlotLeader`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `leader` (string): The public key of the current slot leader.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the current slot leader
$slotLeader = $system->getSlotLeader('finalized');
print_r($slotLeader);
```

---

## `getSlotLeaders` <a name="getslotleaders"></a>

### What is `getSlotLeaders`?

The `getSlotLeaders` RPC method retrieves the list of slot leaders for a range of slots. This is useful for identifying which validators are responsible for producing blocks during specific slots.

- **RPC Method:** `getSlotLeaders`
- **Parameters:**
  - **`startSlot`** (integer): The starting slot of the range.
  - **`limit`** (integer): The number of slot leaders to retrieve.

- **Response:**
  - An array of public keys representing the slot leaders for the specified range.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the slot leaders for a range of slots
$slotLeaders = $system->getSlotLeaders(123400, 10);
print_r($slotLeaders);
```

---

## `getStakeMinimumDelegation` <a name="getstakeminimumdelegation"></a>

### What is `getStakeMinimumDelegation`?

The `getStakeMinimumDelegation` RPC method retrieves the minimum stake delegation required in the Solana cluster. This is useful for validators or delegators to determine the minimum amount of stake necessary to participate.

- **RPC Method:** `getStakeMinimumDelegation`
- **Parameters:** None.

- **Response:**
  - `lamports` (integer): The minimum stake delegation required in lamports.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the minimum stake delegation
$minimumStakeDelegation = $system->getStakeMinimumDelegation();
print_r($minimumStakeDelegation);
```

---

## `getSupply` <a name="getsupply"></a>

### What is `getSupply`?

The `getSupply` RPC method retrieves information about the total token supply in the Solana cluster. This includes both the circulating and non-circulating supply.

- **RPC Method:** `getSupply`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `total` (integer): The total supply of tokens in lamports.
  - `circulating` (integer): The circulating supply of tokens in lamports.
  - `nonCirculating` (integer): The non-circulating supply of tokens in lamports.
  - `nonCirculatingAccounts` (array): An array of public keys that hold non-circulating tokens.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the token supply information
$supply = $system->getSupply('finalized');
print_r($supply);
```

---

## `getTokenAccountBalance` <a name="gettokenaccountbalance"></a>

### What is `getTokenAccountBalance`?

The `getTokenAccountBalance` RPC method retrieves the token balance of a specific token account. This is useful for querying SPL token balances held in a token account.

- **RPC Method:** `getTokenAccountBalance`
- **Parameters:**
  - **`account`** (string): The public key of the token account to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `uiAmount` (float | null): The token balance in a user-friendly format (e.g., adjusted for decimals).
  - `amount` (string): The raw token balance as a string.
  - `decimals` (integer): The number of decimals configured for the token.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get the token account balance
$tokenBalance = $account->getTokenAccountBalance('4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg');
print_r($tokenBalance);
```

---

## `getTokenAccountsByDelegate` <a name="gettokenaccountsbydelegate"></a>

### What is `getTokenAccountsByDelegate`?

The `getTokenAccountsByDelegate` RPC method retrieves all token accounts delegated to a specific delegate. This is useful for tracking accounts that a delegate has control over within the Solana cluster.

- **RPC Method:** `getTokenAccountsByDelegate`
- **Parameters:**
  - **`delegate`** (string): The public key of the delegate to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the account data encoding (e.g., `base64`, `jsonParsed`).
    - `filters` (array): An array of filter objects to refine the query.

- **Response:**
  - An array of objects, where each object contains:
    - `pubkey` (string): The public key of the token account.
    - `account` (object):
      - `lamports` (integer): The balance of the account in lamports.
      - `owner` (string): The public key of the program that owns the account.
      - `data` (mixed): The raw data stored in the account.
      - `executable` (boolean): Whether the account is executable.
      - `rentEpoch` (integer): The next epoch in which rent is due.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get all token accounts by a specific delegate
$tokenAccounts = $account->getTokenAccountsByDelegate(
    'DelegatePublicKeyHere',
    ['commitment' => 'finalized', 'encoding' => 'jsonParsed']
);
print_r($tokenAccounts);
```

---

## `getTokenAccountsByOwner` <a name="gettokenaccountsbyowner"></a>

### What is `getTokenAccountsByOwner`?

The `getTokenAccountsByOwner` RPC method retrieves all token accounts owned by a specific account. This is useful for identifying all SPL token accounts associated with a given owner.

- **RPC Method:** `getTokenAccountsByOwner`
- **Parameters:**
  - **`owner`** (string): The public key of the account owner to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the account data encoding (e.g., `base64`, `jsonParsed`).
    - `filters` (array): An array of filter objects to refine the query.

- **Response:**
  - An array of objects, where each object contains:
    - `pubkey` (string): The public key of the token account.
    - `account` (object):
      - `lamports` (integer): The balance of the account in lamports.
      - `owner` (string): The public key of the program that owns the account.
      - `data` (mixed): The raw data stored in the account.
      - `executable` (boolean): Whether the account is executable.
      - `rentEpoch` (integer): The next epoch in which rent is due.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get all token accounts owned by a specific account
$tokenAccounts = $account->getTokenAccountsByOwner(
    '4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg',
    ['commitment' => 'finalized', 'encoding' => 'jsonParsed']
);
print_r($tokenAccounts);
```

---

## `getTokenLargestAccounts` <a name="gettokenlargestaccounts"></a>

### What is `getTokenLargestAccounts`?

The `getTokenLargestAccounts` RPC method retrieves the largest accounts holding a specific SPL token. This is useful for identifying accounts with significant holdings of a given token.

- **RPC Method:** `getTokenLargestAccounts`
- **Parameters:**
  - **`mint`** (string): The public key of the token mint to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - An array of objects, where each object contains:
    - `address` (string): The public key of the token account.
    - `amount` (string): The raw token balance as a string.
    - `decimals` (integer): The number of decimals configured for the token.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get the largest accounts holding a specific SPL token
$largestTokenAccounts = $account->getTokenLargestAccounts(
    'TokenMintPublicKeyHere',
    ['commitment' => 'finalized']
);
print_r($largestTokenAccounts);
```

---

## `getTokenSupply` <a name="gettokensupply"></a>

### What is `getTokenSupply`?

The `getTokenSupply` RPC method retrieves the total supply of a specific SPL token. This is useful for tracking tokenomics and understanding the overall availability of a token.

- **RPC Method:** `getTokenSupply`
- **Parameters:**
  - **`mint`** (string): The public key of the token mint to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `uiAmount` (float | null): The total supply in a user-friendly format (e.g., adjusted for decimals).
  - `amount` (string): The raw token supply as a string.
  - `decimals` (integer): The number of decimals configured for the token.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Account;

$account = new Account($rpc);

// Get the total supply of a specific SPL token
$tokenSupply = $account->getTokenSupply(
    'TokenMintPublicKeyHere',
    ['commitment' => 'finalized']
);
print_r($tokenSupply);
```

---

## `getTransaction` <a name="gettransaction"></a>

### What is `getTransaction`?

The `getTransaction` RPC method retrieves detailed information about a specific transaction using its signature. This is useful for inspecting transaction details, including instructions, accounts, and outcomes.

- **RPC Method:** `getTransaction`
- **Parameters:**
  - **`signature`** (string): The transaction signature to query.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the transaction data encoding (e.g., `json`, `jsonParsed`, `base64`).

- **Response:**
  - An object containing:
    - `slot` (integer): The slot in which the transaction was processed.
    - `meta` (object): Metadata about the transaction, including status and fee information.
    - `transaction` (object): The full transaction object, including instructions, account keys, and signatures.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Transaction;

$transaction = new Transaction($rpc);

// Get detailed information about a transaction
$transactionDetails = $transaction->getTransaction(
    'TransactionSignatureHere',
    ['commitment' => 'finalized', 'encoding' => 'json']
);
print_r($transactionDetails);
```

---

## `getTransactionCount` <a name="gettransactioncount"></a>

### What is `getTransactionCount`?

The `getTransactionCount` RPC method retrieves the total number of transactions processed by the cluster. This is useful for monitoring network activity and performance.

- **RPC Method:** `getTransactionCount`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `count` (integer): The total number of transactions processed by the cluster.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the total transaction count
$transactionCount = $system->getTransactionCount('finalized');
print_r($transactionCount);
```

---

## `getVersion` <a name="getversion"></a>

### What is `getVersion`?

The `getVersion` RPC method retrieves the current software version of the node. This is useful for checking compatibility and ensuring the node is running the desired version.

- **RPC Method:** `getVersion`
- **Parameters:** None.

- **Response:**
  - `solana-core` (string): The version of the Solana core software.
  - `feature-set` (integer | null): The feature set identifier, if available.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get the current software version of the node
$version = $system->getVersion();
print_r($version);
```

---

## `getVoteAccounts` <a name="getvoteaccounts"></a>

### What is `getVoteAccounts`?

The `getVoteAccounts` RPC method retrieves information about the current vote accounts in the Solana cluster. This includes details about both active and delinquent validators.

- **RPC Method:** `getVoteAccounts`
- **Parameters:**
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `current` (array): An array of active vote account objects, each containing:
    - `votePubkey` (string): The public key of the vote account.
    - `nodePubkey` (string): The public key of the validator node.
    - `commission` (integer): The commission charged by the validator.
    - `lastVote` (integer): The last slot in which the validator voted.
    - `activatedStake` (integer): The amount of active stake delegated to the vote account.
  - `delinquent` (array): An array of delinquent vote account objects with the same fields as `current`.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Get information about vote accounts
$voteAccounts = $system->getVoteAccounts('finalized');
print_r($voteAccounts);
```

---

## `isBlockhashValid` <a name="isblockhashvalid"></a>

### What is `isBlockhashValid`?

The `isBlockhashValid` RPC method checks whether a given blockhash is still valid. This is useful for determining if a blockhash can be used in a transaction.

- **RPC Method:** `isBlockhashValid`
- **Parameters:**
  - **`blockhash`** (string): The blockhash to validate.
  - **Optional:**
    - `commitment` (string): Specifies the state of the ledger to query (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - `valid` (boolean): Indicates whether the blockhash is valid.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Check if a blockhash is valid
$isValid = $block->isBlockhashValid(
    'BlockhashHere',
    ['commitment' => 'finalized']
);
print_r($isValid);
```

---

## `minimumLedgerSlot` <a name="minimumledgerslot"></a>

### What is `minimumLedgerSlot`?

The `minimumLedgerSlot` RPC method retrieves the lowest slot that the node has information about in its ledger. This is useful for determining the earliest available data on the current node.

- **RPC Method:** `minimumLedgerSlot`
- **Parameters:** None.

- **Response:**
  - `slot` (integer): The lowest slot available in the ledger.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Block;

$block = new Block($rpc);

// Get the minimum ledger slot available
$minimumSlot = $block->minimumLedgerSlot();
print_r($minimumSlot);
```

---

## `requestAirdrop` <a name="requestairdrop"></a>

### What is `requestAirdrop`?

The `requestAirdrop` RPC method requests a specific amount of SOL to be airdropped into a given account. This is primarily useful for testing purposes on devnet or testnet.

- **RPC Method:** `requestAirdrop`
- **Parameters:**
  - **`pubkey`** (string): The public key of the account to receive the airdrop.
  - **`lamports`** (integer): The amount of SOL (in lamports) to airdrop.

- **Response:**
  - `signature` (string): The transaction signature of the airdrop request.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\System;

$system = new System($rpc);

// Request an airdrop of 1 SOL (1 SOL = 1,000,000,000 lamports)
$airdropSignature = $system->requestAirdrop(
    '4izNYzN7uQac8jBDcD7NmuCpS8PqvYiHVSLXF5bY9Zrg',
    1000000000
);
print_r($airdropSignature);
```

---

## `sendTransaction` <a name="sendtransaction"></a>

### What is `sendTransaction`?

The `sendTransaction` RPC method submits a signed transaction to the Solana network for processing. This is a fundamental method for interacting with the blockchain, enabling the execution of various operations such as transfers, program instructions, and more.

- **RPC Method:** `sendTransaction`
- **Parameters:**
  - **`signedTransaction`** (string): The signed transaction in base64 encoding.
  - **Optional:**
    - `skipPreflight` (boolean): If `true`, skips the preflight transaction checks. Defaults to `false`.
    - `preflightCommitment` (string): Specifies the commitment level for preflight checks (e.g., `finalized`, `confirmed`, or `processed`).
    - `encoding` (string): Specifies the transaction data encoding (e.g., `base64`).

- **Response:**
  - `signature` (string): The transaction signature, which can be used to track its status.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Transaction;

$transaction = new Transaction($rpc);

// Send a signed transaction to the network
$transactionSignature = $transaction->sendTransaction(
    'SignedTransactionBase64Here',
    ['skipPreflight' => true, 'preflightCommitment' => 'confirmed']
);
print_r($transactionSignature);
```

---

## `simulateTransaction` <a name="simulatetransaction"></a>

### What is `simulateTransaction`?

The `simulateTransaction` RPC method simulates the execution of a signed transaction without broadcasting it to the network. This is useful for testing and debugging transaction logic before submitting it to the blockchain.

- **RPC Method:** `simulateTransaction`
- **Parameters:**
  - **`signedTransaction`** (string): The signed transaction in base64 encoding to simulate.
  - **Optional:**
    - `sigVerify` (boolean): If `true`, verifies the transaction signatures during simulation. Defaults to `false`.
    - `commitment` (string): Specifies the commitment level for the simulation (e.g., `finalized`, `confirmed`, or `processed`).

- **Response:**
  - An object containing:
    - `err` (object | null): Any error encountered during simulation.
    - `logs` (array): The list of log messages produced during simulation.
    - `accounts` (array | null): Information about accounts involved in the simulation, if requested.

---

### Example Usage

```php
use JosephOpanel\SolanaSDK\Endpoints\JsonRPC\Transaction;

$transaction = new Transaction($rpc);

// Simulate a signed transaction
$simulationResult = $transaction->simulateTransaction(
    'SignedTransactionBase64Here',
    ['sigVerify' => true, 'commitment' => 'processed']
);
print_r($simulationResult);
```

---