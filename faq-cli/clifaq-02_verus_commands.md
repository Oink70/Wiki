# Options available to the Verusd coindaemon.

## Important General Information

### Verus CLI version 0.6.4-3

Usage: `verus [command]` Issue a command to the coindaemon

### Verus `Wallet.dat`, Chaindata & `VRSC.conf` standard locations

Linux:		`~/.Komodo/VRSC`
Mac OS: 	`~/Library/Application Support/Komodo/VRSC`
Windows 10: 	`%AppData%\Roaming\Komodo\VRSC\`

# Commands:

## Addressindex

### `getaddressbalance`
Returns the balance for an address(es) (requires addressindex to be enabled).

Arguments:
```
{
  "addresses"
    [
      "address"  (string) The base58check encoded address
      ,...
    ]
}
```
Result:
```
{
  "balance"  (string) The current balance in satoshis
  "received"  (string) The total number of satoshis received (including change)
}
```

### `getaddressdeltas`
Returns all changes for an address (requires addressindex to be enabled).

Arguments:
```
{
  "addresses"
    [
      "address"  (string) The base58check encoded address
      ,...
    ]
  "start" (number) The start block height
  "end" (number) The end block height
  "chainInfo" (boolean) Include chain info in results, only applies if start and end specified
}
```
Result:
```
[
  {
    "satoshis"  (number) The difference of satoshis
    "txid"  (string) The related txid
    "index"  (number) The related input or output index
    "height"  (number) The block height
    "address"  (string) The base58check encoded address
  }
]
```

### `getaddressmempool`
Returns all mempool deltas for an address (requires addressindex to be enabled).

Arguments:
```
{
  "addresses"
    [
      "address"  (string) The base58check encoded address
      ,...
    ]
}
```
Result:
```
[
  {
    "address"  (string) The base58check encoded address
    "txid"  (string) The related txid
    "index"  (number) The related input or output index
    "satoshis"  (number) The difference of satoshis
    "timestamp"  (number) The time the transaction entered the mempool (seconds)
    "prevtxid"  (string) The previous txid (if spending)
    "prevout"  (string) The previous transaction output index (if spending)
  }
]
```

### `getaddresstxids`
Returns the txids for an address(es) (requires addressindex to be enabled).

Arguments:
```
{
  "addresses"
    [
      "address"  (string) The base58check encoded address
      ,...
    ]
  "start" (number) The start block height
  "end" (number) The end block height
}
```
Result:
```
[
  "transactionid"  (string) The transaction id
  ,...
]
```

### `getaddressutxos`
Returns all unspent outputs for an address (requires addressindex to be enabled).

Arguments:
```
{
  "addresses"
    [
      "address"  (string) The base58check encoded address
      ,...
    ],
  "chainInfo"  (boolean) Include chain info with results
}
```
Result
```
[
  {
    "address"  (string) The address base58check encoded
    "txid"  (string) The output txid
    "height"  (number) The block height
    "outputIndex"  (number) The output index
    "script"  (strin) The script hex encoded
    "satoshis"  (number) The number of satoshis of the output
  }
]
```

### `getsnapshot`
Returns a snapshot of (address,amount) pairs at current height (requires addressindex to be enabled).

Arguments:
  "top" (number, optional) Only return this many addresses, i.e. top N richlist

Result:
```
{
   "addresses": [
    {
      "addr": "RMEBhzvATA8mrfVK82E5TgPzzjtaggRGN3",
      "amount": "100.0"
    },
    {
      "addr": "RqEBhzvATAJmrfVL82E57gPzzjtaggR777",
      "amount": "23.45"
    }
  ],
  "total": 123.45           (numeric) Total amount in snapshot
  "average": 61.7,          (numeric) Average amount in each address
  "utxos": 14,              (number) Total number of UTXOs in snapshot
  "total_addresses": 2,     (number) Total number of addresses in snapshot,
  "start_height": 91,       (number) Block height snapshot began
  "ending_height": 91       (number) Block height snapsho finished,
  "start_time": 1531982752, (number) Unix epoch time snapshot started
  "end_time": 1531982752    (number) Unix epoch time snapshot finished
}
```

## Blockchain
### `coinsupply <height>`
Return coin supply information at a given block height. If no height is given, the current height is used.

Arguments:
1. "height"     (integer, optional) Block height

Result:
```
{
  "result" : "success",         (string) If the request was successful.
  "coin" : "VRSC",              (string) The currency symbol of the native coin of this blockchain.
  "height" : 420,                 (integer) The height of this coin supply data
  "supply" : "777.0",           (float) The transparent coin supply
  "zfunds" : "0.777",           (float) The shielded coin supply (in zaddrs)
  "total" :  "777.777",         (float) The total coin supply, i.e. sum of supply + zfunds
}
```

### `getbestblockhash`
Returns the hash of the best (tip) block in the longest block chain.

Result
"hex"      (string) the block hash hex encoded

### `getblock "hash|height" ( verbosity )`
If verbosity is 0, returns a string that is serialized, hex-encoded data for the block.
If verbosity is 1, returns an Object with information about the block.
If verbosity is 2, returns an Object with information about the block and information about each transaction.

Arguments:
1. "hash|height"          (string, required) The block hash or height
2. verbosity              (numeric, optional, default=1) 0 for hex encoded data, 1 for a json object, and 2 for json object with transaction data

Result (for verbosity = 0):
"data"             (string) A string that is serialized, hex-encoded data for the block.

Result (for verbosity = 1):
```
{
  "hash" : "hash",       (string) the block hash (same as provided hash)
  "confirmations" : n,   (numeric) The number of confirmations, or -1 if the block is not on the main chain
  "size" : n,            (numeric) The block size
  "height" : n,          (numeric) The block height or index (same as provided height)
  "version" : n,         (numeric) The block version
  "merkleroot" : "xxxx", (string) The merkle root
  "finalsaplingroot" : "xxxx", (string) The root of the Sapling commitment tree after applying this block
  "tx" : [               (array of string) The transaction ids
     "transactionid"     (string) The transaction id
     ,...
  ],
  "time" : ttt,          (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "nonce" : n,           (numeric) The nonce
  "bits" : "1d00ffff",   (string) The bits
  "difficulty" : x.xxx,  (numeric) The difficulty
  "previousblockhash" : "hash",  (string) The hash of the previous block
  "nextblockhash" : "hash"       (string) The hash of the next block
}
```
Result (for verbosity = 2):
```
{
  ...,                     Same output as verbosity = 1.
  "tx" : [               (array of Objects) The transactions in the format of the getrawtransaction RPC. Different from verbosity = 1 "tx" result.
         ,...
  ],
  ,...                     Same output as verbosity = 1.
}
```

### `getblockchaininfo`
Returns an object containing various state info regarding block chain processing.

Note that when the chain tip is at the last block before a network upgrade activation,
consensus.chaintip != consensus.nextblock.

Result:
```
{
  "chain": "xxxx",        (string) current network name as defined in BIP70 (main, test, regtest)
  "blocks": xxxxxx,         (numeric) the current number of blocks processed in the server
  "headers": xxxxxx,        (numeric) the current number of headers we have validated
  "bestblockhash": "...", (string) the hash of the currently best block
  "difficulty": xxxxxx,     (numeric) the current difficulty
  "verificationprogress": xxxx, (numeric) estimate of verification progress [0..1]
  "chainwork": "xxxx"     (string) total amount of work in active chain, in hexadecimal
  "size_on_disk": xxxxxx,       (numeric) the estimated size of the block and undo files on disk
  "commitments": xxxxxx,    (numeric) the current number of note commitments in the commitment tree
  "softforks": [            (array) status of softforks in progress
     {
        "id": "xxxx",        (string) name of softfork
        "version": xx,         (numeric) block version
        "enforce": {           (object) progress toward enforcing the softfork rules for new-version blocks
           "status": xx,       (boolean) true if threshold reached
           "found": xx,        (numeric) number of blocks with the new version found
           "required": xx,     (numeric) number of blocks required to trigger
           "window": xx,       (numeric) maximum size of examined window of recent blocks
        },
        "reject": { ... }      (object) progress toward rejecting pre-softfork blocks (same fields as "enforce")
     }, ...
  ],
  "upgrades": {                (object) status of network upgrades
     "xxxx" : {                (string) branch ID of the upgrade
        "name": "xxxx",        (string) name of upgrade
        "activationheight": xxxxxx,  (numeric) block height of activation
        "status": "xxxx",      (string) status of upgrade
        "info": "xxxx",        (string) additional information about upgrade
     }, ...
  },
  "consensus": {               (object) branch IDs of the current and upcoming consensus rules
     "chaintip": "xxxxxxxx",   (string) branch ID used to validate the current chain tip
     "nextblock": "xxxxxxxx"   (string) branch ID that the next block will be validated under
  }
}
```

### `getblockcount`
Returns the number of blocks in the best valid block chain.

Result:
n    (numeric) The current block count

### `getblockdeltas "blockhash"`
Returns information about the given block and its transactions.

WARNING: getblockdeltas is disabled.
To enable it, restart zcashd with the `-experimentalfeatures` and `-insightexplorer` commandline options, or add these two lines to the `VRSC.conf` file:
```
experimentalfeatures=1
insightexplorer=1
```
Arguments:
1. "hash"          (string, required) The block hash

Result:
```
{
  "hash": "hash",              (string) block ID
  "confirmations": n,          (numeric) number of confirmations
  "size": n,                   (numeric) block size in bytes
  "height": n,                 (numeric) block height
  "version": n,                (numeric) block version (e.g. 4)
  "merkleroot": "hash",        (hexstring) block Merkle root
  "deltas": [
    {
      "txid": "hash",          (hexstring) transaction ID
      "index": n,              (numeric) The offset of the tx in the block
      "inputs": [                (array of json objects)
        {
          "address": "taddr",  (string) transparent address
          "satoshis": n,       (numeric) negative of spend amount
          "index": n,          (numeric) vin index
          "prevtxid": "hash",  (string) source utxo tx ID
          "prevout": n         (numeric) source utxo index
        }, ...
      ],
      "outputs": [             (array of json objects)
        {
          "address": "taddr",  (string) transparent address
          "satoshis": n,       (numeric) amount
          "index": n           (numeric) vout index
        }, ...
      ]
    }, ...
  ],
  "time" : n,                  (numeric) The block version
  "mediantime": n,             (numeric) The most recent blocks' ave time
  "nonce" : "nonce",           (hex string) The nonce
  "bits" : "1d00ffff",         (hex string) The bits
  "difficulty": n,             (numeric) the current difficulty
  "chainwork": "xxxx"          (hex string) total amount of work in active chain
  "previousblockhash" : "hash",(hex string) The hash of the previous block
  "nextblockhash" : "hash"     (hex string) The hash of the next block
}
```

### `getblockhash index`
Returns hash of block in best-block-chain at index provided.

Arguments:
1. index         (numeric, required) The block index

Result:
"hash"         (string) The block hash

### `getblockhashes timestamp`
Returns array of hashes of blocks within the timestamp range provided.

Arguments:
1. high         (numeric, required) The newer block timestamp
2. low          (numeric, required) The older block timestamp
3. options      (string, required) A json object
```
    {
      "noOrphans":true   (boolean) will only include blocks on the main chain
      "logicalTimes":true   (boolean) will include logical timestamps with hashes
    }
```
Result:
```
[
  "hash"         (string) The block hash
]
[
  {
    "blockhash": (string) The block hash
    "logicalts": (numeric) The logical timestamp
  }
]
```

### `getblockheader "hash" ( verbose )`
If verbose is false, returns a string that is serialized, hex-encoded data for blockheader 'hash'.
If verbose is true, returns an Object with information about blockheader <hash>.

Arguments:
1. "hash"          (string, required) The block hash
2. verbose           (boolean, optional, default=true) true for a json object, false for the hex encoded data

Result (for verbose = true):
```
{
  "hash" : "hash",     (string) the block hash (same as provided)
  "confirmations" : n,   (numeric) The number of confirmations, or -1 if the block is not on the main chain
  "height" : n,          (numeric) The block height or index
  "version" : n,         (numeric) The block version
  "merkleroot" : "xxxx", (string) The merkle root
  "finalsaplingroot" : "xxxx", (string) The root of the Sapling commitment tree after applying this block
  "time" : ttt,          (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
  "nonce" : n,           (numeric) The nonce
  "bits" : "1d00ffff", (string) The bits
  "difficulty" : x.xxx,  (numeric) The difficulty
  "previousblockhash" : "hash",  (string) The hash of the previous block
  "nextblockhash" : "hash"       (string) The hash of the next block
}
```
Result (for verbose=false):
"data"             (string) A string that is serialized, hex-encoded data for block 'hash'.

### `getchaintips`
Return information about all known tips in the block tree, including the main chain as well as orphaned branches.

Result:
```
[
  {
    "height": xxxx,         (numeric) height of the chain tip
    "hash": "xxxx",         (string) block hash of the tip
    "branchlen": 0          (numeric) zero for main chain
    "status": "active"      (string) "active" for the main chain
  },
  {
    "height": xxxx,
    "hash": "xxxx",
    "branchlen": 1          (numeric) length of branch connecting the tip to the main chain
    "status": "xxxx"        (string) status of the chain (active, valid-fork, valid-headers, headers-only, invalid)
  }
]
```
Possible values for status:
1.  "invalid"               This branch contains at least one invalid block
2.  "headers-only"          Not all blocks for this branch are available, but the headers are valid
3.  "valid-headers"         All blocks are available for this branch, but they were never fully validated
4.  "valid-fork"            This branch is not part of the active chain, but is fully validated
5.  "active"                This is the tip of the active main chain, which is certainly valid

### `getdifficulty`
Returns the proof-of-work difficulty as a multiple of the minimum difficulty.

Result:
n.nnn       (numeric) the proof-of-work difficulty as a multiple of the minimum difficulty.

### `getmempoolinfo`
Returns details on the active state of the TX memory pool.

Result:
```
{
  "size": xxxxx                (numeric) Current tx count
  "bytes": xxxxx               (numeric) Sum of all tx sizes
  "usage": xxxxx               (numeric) Total memory usage for the mempool
}
```

### `getrawmempool ( verbose )`
Returns all transaction ids in memory pool as a json array of string transaction ids.

Arguments:
1. verbose           (boolean, optional, default=false) true for a json object, false for array of transaction ids

Result: (for verbose = false):
```
[                     (json array of string)
  "transactionid"     (string) The transaction id
  ,...
]
```
Result: (for verbose = true):
```
{                           (json object)
  "transactionid" : {       (json object)
    "size" : n,             (numeric) transaction size in bytes
    "fee" : n,              (numeric) transaction fee in VRSC
    "time" : n,             (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n,           (numeric) block height when transaction entered pool
    "startingpriority" : n, (numeric) priority when transaction entered pool
    "currentpriority" : n,  (numeric) transaction priority now
    "depends" : [           (array) unconfirmed transactions used as inputs for this transaction
        "transactionid",    (string) parent transaction id
       ... ]
  }, ...
}
```

### `getspentinfo`
Returns the txid and index where an output is spent.

Arguments:
```
{
  "txid" (string) The hex string of the txid
  "index" (number) The start block height
}
```
Result:
```
{
  "txid"  (string) The transaction id
  "index"  (number) The spending input index
  ,...
}
```

### `gettxout "txid" n ( includemempool )`
Returns details about an unspent transaction output.

Arguments:
1. "txid"       (string, required) The transaction id
2. n              (numeric, required) vout value
3. includemempool  (boolean, optional) Whether to include the mempool

Result:
```
{
  "bestblock" : "hash",    (string) the block hash
  "confirmations" : n,       (numeric) The number of confirmations
  "value" : x.xxx,           (numeric) The transaction value in VRSC
  "scriptPubKey" : {         (json object)
     "asm" : "code",       (string)
     "hex" : "hex",        (string)
     "reqSigs" : n,          (numeric) Number of required signatures
     "type" : "pubkeyhash", (string) The type, eg pubkeyhash
     "addresses" : [          (array of string) array of Komodo addresses
        "komodoaddress"        (string) Komodo address
        ,...
     ]
  },
  "version" : n,              (numeric) The version
  "coinbase" : true|false     (boolean) Coinbase or not
}
```

## `gettxoutproof ["txid",...] ( blockhash )`
Returns a hex-encoded proof that "txid" was included in a block.

NOTE: By default this function only works sometimes. This is when there is an
unspent output in the utxo for this transaction. To make it always work,
you need to maintain a transaction index, using the -txindex command line option or
specify the block in which the transaction is included in manually (by blockhash).

Return the raw transaction data.

Arguments:
1. "txids"       (string) A json array of txids to filter
```
    [
      "txid"     (string) A transaction hash
      ,...
    ]
```
2. "block hash"  (string, optional) If specified, looks for txid in the block with this hash

Result:
"data"           (string) A string that is a serialized, hex-encoded data for the proof.

### `gettxoutsetinfo`
Returns statistics about the unspent transaction output set.
Note this call may take some time.

Result:
```
{
  "height":n,     (numeric) The current block height (index)
  "bestblock": "hex",   (string) the best block hash hex
  "transactions": n,      (numeric) The number of transactions
  "txouts": n,            (numeric) The number of output transactions
  "bytes_serialized": n,  (numeric) The serialized size
  "hash_serialized": "hash",   (string) The serialized hash
  "total_amount": x.xxx          (numeric) The total amount
}
```

### `kvsearch key`
Search for a key stored via the kvupdate command. This feature is only available for asset chains.

Arguments:
1. key                      (string, required) search the chain for this key

Result:
```
{
  "coin": "xxxxx",          (string) chain the key is stored on
  "currentheight": xxxxx,     (numeric) current height of the chain
  "key": "xxxxx",           (string) key
  "keylen": xxxxx,            (string) length of the key
  "owner": "xxxxx"          (string) hex string representing the owner of the key
  "height": xxxxx,            (numeric) height the key was stored at
  "expiration": xxxxx,        (numeric) height the key will expire
  "flags": x                  (numeric) 1 if the key was created with a password; 0 otherwise.
  "value": "xxxxx",         (string) stored value
  "valuesize": xxxxx          (string) amount of characters stored
}
```

### `kvupdate key "value" days passphrase`
Store a key value. This feature is only available for asset chains.

Arguments:
1. key                      (string, required) key
2. "value"                (string, required) value
3. days                     (numeric, required) amount of days(1440 blocks/day) before the key expires. Minimum 1 day
4. passphrase               (string, optional) passphrase required to update this key

Result:
```
{
  "coin": "xxxxx",          (string) chain the key is stored on
  "height": xxxxx,            (numeric) height the key was stored at
  "expiration": xxxxx,        (numeric) height the key will expire
  "flags": x,                 (string) amount of days the key will be stored
  "key": "xxxxx",           (numeric) stored key
  "keylen": xxxxx,            (numeric) length of the key
  "value": "xxxxx"          (numeric) stored value
  "valuesize": xxxxx,         (string) length of the stored value
  "fee": xxxxx                (string) transaction fee paid to store the key
  "txid": "xxxxx"           (string) transaction id
}
```

### `minerids needs height`

### `notaries height timestamp`

### `verifychain ( checklevel numblocks )`
Verifies blockchain database.

Arguments:
1. checklevel   (numeric, optional, 0-4, default=3) How thorough the block verification is.
2. numblocks    (numeric, optional, default=288, 0=all) The number of blocks to check.

Result:
true|false       (boolean) Verified or not

### `verifytxoutproof "proof"`
Verifies that a proof points to a transaction in a block, returning the transaction it commits to
and throwing an RPC error if the block is not in our best chain

Arguments:
1. "proof"    (string, required) The hex-encoded proof generated by gettxoutproof

Result:
["txid"]      (array, strings) The txid(s) which the proof commits to, or empty array if the proof is invalid

## Control

### `getinfo`
Returns an object containing various state info.

Result:
```
{
  "version": xxxxx,           (numeric) the server version
  "protocolversion": xxxxx,   (numeric) the protocol version
  "walletversion": xxxxx,     (numeric) the wallet version
  "balance": xxxxxxx,         (numeric) the total Komodo balance of the wallet
  "blocks": xxxxxx,           (numeric) the current number of blocks processed in the server
  "timeoffset": xxxxx,        (numeric) the time offset
  "connections": xxxxx,       (numeric) the number of connections
  "proxy": "host:port",     (string, optional) the proxy used by the server
  "difficulty": xxxxxx,       (numeric) the current difficulty
  "testnet": true|false,      (boolean) if the server is using testnet or not
  "keypoololdest": xxxxxx,    (numeric) the timestamp (seconds since GMT epoch) of the oldest pre-generated key in the key pool
  "keypoolsize": xxxx,        (numeric) how many new keys are pre-generated
  "unlocked_until": ttt,      (numeric) the timestamp in seconds since epoch (midnight Jan 1 1970 GMT) that the wallet is unlocked for transfers, or 0 if the wallet is locked
  "paytxfee": x.xxxx,         (numeric) the transaction fee set in VRSC/kB
  "relayfee": x.xxxx,         (numeric) minimum relay fee for non-free transactions in VRSC/kB
  "errors": "..."           (string) any error messages
}
```

### `help ( "command" )`
List all commands, or get help for a specified command.

Arguments:
1. "command"     (string, optional) The command to get help on

Result:
"text"     (string) The help text

### `stop`
Stop Verus server.

## Crosschain

### `MoMoMdata symbol kmdheight ccid`
### `assetchainproof needs a txid`
### `calc_MoM height MoMdepth`
### `getNotarisationsForBlock blockHash`
Takes a block hash and returns notarisation transactions within the block

### `height_MoM height`
### `migrate_completeimporttransaction importTx`
### `migrate_converttoexport rawTx dest_symbol export_amount`
Convert a raw transaction to a cross-chain export.
If neccesary, the transaction should be funded using `fundrawtransaction`.
Finally, the transaction should be signed using `signrawtransaction`.
The finished export transaction, plus the payouts, should be passed to the "`migrate_createimporttransactio`n" method on a KMD node to get the corresponding import transaction.

### `migrate_createimporttransaction burnTx payouts`
Create an importTx given a burnTx and the corresponding payouts, hex encoded

### `scanNotarisationsDB blockHeight symbol [blocksLimit=1440]`
Scans notarisationsdb backwards from height for a notarisation of given symbol

## Disclosure

### `z_getpaymentdisclosure "txid" "js_index" "output_index" ("message")`
Generate a payment disclosure for a given joinsplit output.

EXPERIMENTAL FEATURE

WARNING: z_getpaymentdisclosure is disabled.
To enable it, restart verusd with the `-experimentalfeatures` and `-paymentdisclosure` commandline options, or add these two lines to the zcash.conf file:
```
experimentalfeatures=1
paymentdisclosure=1
```
Arguments:
1. "txid"            (string, required)
2. "js_index"        (string, required)
3. "output_index"    (string, required)
4. "message"         (string, optional)

Result:
"paymentdisclosure"  (string) Hex data string, with "zpd:" prefix.

### `z_validatepaymentdisclosure "paymentdisclosure"`
Validates a payment disclosure.

EXPERIMENTAL FEATURE

WARNING: z_validatepaymentdisclosure is disabled.
To enable it, restart zcashd with the `-experimentalfeatures` and
`-paymentdisclosure` commandline options, or add these two lines
to the zcash.conf file:
```
experimentalfeatures=1
paymentdisclosure=1
```
Arguments:
1. "paymentdisclosure"     (string, required) Hex data string, with "zpd:" prefix.

## Generating

### `generate numblocks`
Mine blocks immediately (before the RPC call returns)

Note: this function can only be used on the regtest network

Arguments:
1. numblocks    (numeric) How many blocks are generated immediately.

Result
[ blockhashes ]     (array) hashes of blocks generated

### `getgenerate`
Return if the server is set to mine and/or mint coins or not. The default is false.
It is set with the command line argument `-gen` (or `VRSC.conf` setting `gen`) and `-mint`
It can also be set with the setgenerate call.

Result
```
{
  "staking": true|false      (boolean) If staking is on or off (see setgenerate)
  "generate": true|false     (boolean) If mining is on or off (see setgenerate)
  "numthreads": n            (numeric) The processor limit for mining. (see setgenerate)
}
```

### `setgenerate generate ( genproclimit )`
Set 'generate' true to turn either mining/generation or minting/staking on and false to turn both off.
Mining is limited to 'genproclimit' processors, -1 is unlimited, setgenerate true with 0 genproclimit turns on staking
See the getgenerate call for the current setting.

Arguments:
1. generate         (boolean, required) Set to true to turn on generation, off to turn off.
2. genproclimit     (numeric, optional) Set processor limit when generation is on. Can be -1 for unlimited, 0 to turn on staking.

## Identity

### `getidentity "name"`
Arguments

Result:

### `listidentities (includecansign) (includewatchonly)`
Arguments
    "includecanspend"    (bool, optional, default=true)    Include identities for which we can spend/authorize
    "includecansign"     (bool, optional, default=true)    Include identities that we can only sign for but not spend
    "includewatchonly"   (bool, optional, default=false)   Include identities that we can neither sign nor spend, but are either watched or are co-signers with us

Result:

### `recoveridentity "jsonidentity" (returntx)`
Arguments
       "returntx"                        (bool,   optional) defaults to false and transaction is sent, if true, transaction is signed by this wallet and returned

Result:

### `registeridentity "jsonidregistration" feeoffer`
Arguments
{
    "txid" : "hexid",          (hex)    the transaction ID of the name committment for this ID name
    "namereservation" :
    {
        "name": "namestr",     (string) the unique name in this commitment
        "salt": "hexstr",      (hex)    salt used to hide the commitment
        "referrer": "identityID", (name@ or address) must be a valid ID to use as a referrer to receive a discount
    },
    "identity" :
    {
        "name": "namestr",     (string) the unique name for this identity
        ...
    }
}
feeoffer                           (amount, optional) amount to offer miner/staker for the registration fee, if missing, uses standard price


Result:
   transactionid                   (hexstr)

### `registernamecommitment "name" "controladdress" ("referralidentity")`
Registers a name commitment, which is required as a source for the name to be used when registering an identity. The name commitment hides the name itself while ensuring that the miner who mines in the registration cannot front-run the name unless they have also registered a name commitment for the same name or are willing to forfeit the offer of payment for the chance that a commitment made now will allow them to register the name in the future.

Arguments
"name"                           (string, required)  the unique name to commit to. creating a name commitment is not a registration, and if one is
                                                       created for a name that exists, it may succeed, but will never be able to be used.
"controladdress"                 (address, required) address that will control this commitment
"referralidentity"               (identity, optional)friendly name or identity address that is provided as a referral mechanism and to lower network cost of the ID

Result: obj
```
{
    "txid" : "hexid"
    "namereservation" :
    {
        "name"    : "namestr",     (string) the unique name in this commitment
        "salt"    : "hexstr",      (hex)    salt used to hide the commitment
        "referral": "identityaddress", (base58) address of the referring identity if there is one
        "parent"  : "namestr",   (string) name of the parent if not Verus or Verus test
        "nameid"  : "address",   (base58) identity address for this identity if it is created
    }
}
```

### `revokeidentity "nameorID" (returntx)`
Arguments
       "returntx"                        (bool,   optional) defaults to false and transaction is sent, if true, transaction is signed by this wallet and returned

Result:

### `updateidentity "jsonidentity" (returntx)`
Arguments
       "returntx"                        (bool,   optional) defaults to false and transaction is sent, if true, transaction is signed by this wallet and returned

Result:

## Mining

### `getblocksubsidy height`
Returns block subsidy reward, taking into account the mining slow start and the founders reward, of block at index provided.

Arguments:
1. height         (numeric, optional) The block height.  If not provided, defaults to the current height of the chain.

Result:
```
{
  "miner" : x.xxx           (numeric) The mining reward amount in KMD.
}
```

### `getblocktemplate ( "jsonrequestobject" )`
If the request parameters include a 'mode' key, that is used to explicitly select between the default 'template' request or a 'proposal'.
It returns data needed to construct a block to work on.
See https://en.bitcoin.it/wiki/BIP_0022 for full specification.

Arguments:
1. "jsonrequestobject"       (string, optional) A json object in the following spec
```
     {
       "mode":"template"    (string, optional) This must be set to "template" or omitted
       "capabilities":[       (array, optional) A list of strings
           "support"           (string) client side supported feature, 'longpoll', 'coinbasetxn', 'coinbasevalue', 'proposal', 'serverlist', 'workid'
           ,...
         ]
     }
```

Result:
```
{
  "version" : n,                     (numeric) The block version
  "previousblockhash" : "xxxx",    (string) The hash of current highest block
  "finalsaplingroothash" : "xxxx", (string) The hash of the final sapling root
  "transactions" : [                 (array) contents of non-coinbase transactions that should be included in the next block
      {
         "data" : "xxxx",          (string) transaction data encoded in hexadecimal (byte-for-byte)
         "hash" : "xxxx",          (string) hash/id encoded in little-endian hexadecimal
         "depends" : [              (array) array of numbers
             n                        (numeric) transactions before this one (by 1-based index in 'transactions' list) that must be present in the final block if this one is
             ,...
         ],
         "fee": n,                   (numeric) difference in value between transaction inputs and outputs (in Satoshis); for coinbase transactions, this is a negative Number of the total collected block fees (ie, not including the block subsidy); if key is not present, fee is unknown and clients MUST NOT assume there isn't one
         "sigops" : n,               (numeric) total number of SigOps, as counted for purposes of block limits; if key is not present, sigop count is unknown and clients MUST NOT assume there aren't any
         "required" : true|false     (boolean) if provided and true, this transaction must be in the final block
      }
      ,...
  ],
  "coinbasetxn" : { ... },           (json object) information for coinbase transaction
  "target" : "xxxx",               (string) The hash target
  "mintime" : xxx,                   (numeric) The minimum timestamp appropriate for next block time in seconds since epoch (Jan 1 1970 GMT)
  "mutable" : [                      (array of string) list of ways the block template may be changed
     "value"                         (string) A way the block template may be changed, e.g. 'time', 'transactions', 'prevblock'
     ,...
  ],
  "noncerange" : "00000000ffffffff",   (string) A range of valid nonces
  "sigoplimit" : n,                 (numeric) limit of sigops in blocks
  "sizelimit" : n,                  (numeric) limit of block size
  "curtime" : ttt,                  (numeric) current timestamp in seconds since epoch (Jan 1 1970 GMT)
  "bits" : "xxx",                 (string) compressed target of next block
  "height" : n                      (numeric) The height of the next block
}
```

### `getlocalsolps`
Returns the average local solutions per second since this node was started.
This is the same information shown on the metrics screen (if enabled).

Result:
xxx.xxxxx     (numeric) Solutions per second average

### `getmininginfo`
Returns a json object containing mining-related information.
Result:
```
{
  "blocks": nnn,             (numeric) The current block
  "currentblocksize": nnn,   (numeric) The last block size
  "currentblocktx": nnn,     (numeric) The last block transaction
  "averageblockfees": xxx.xxxxx (numeric) The average block fees, in addition to block reward, over the past 100 blocks
  "difficulty": xxx.xxxxx    (numeric) The current difficulty
  "stakingsupply": xxx.xxxxx (numeric) The current estimated total staking supply
  "errors": "..."          (string) Current errors
  "generate": true|false     (boolean) If the generation is on or off (see getgenerate or setgenerate calls)
  "genproclimit": n          (numeric) The processor limit for generation. -1 if no generation. (see getgenerate or setgenerate calls)
  "localsolps": xxx.xxxxx    (numeric) The average local solution rate in Sol/s since this node was started
  "networksolps": x          (numeric) The estimated network solution rate in Sol/s
  "pooledtx": n              (numeric) The size of the mem pool
  "testnet": true|false      (boolean) If using testnet or not
  "chain": "xxxx",         (string) current network name as defined in BIP70 (main, test, regtest)
  "generate": true|false     (boolean) If this instance is mining or staking
  "staking": true|false      (boolean) If staking
  "numthreads": n            (numeric) Number of CPU threads mining
  "mergemining": n           (numeric) Number of blockchains we are merge mining with
  "mergeminedchains": []     (optional, list of names) Blockchain names that are being merge mined with this blockchain
}
```

### `getnetworkhashps ( blocks height )`
DEPRECATED - left for backwards-compatibility. Use getnetworksolps instead.

Returns the estimated network solutions per second based on the last n blocks.
Pass in [blocks] to override # of blocks, -1 specifies over difficulty averaging window.
Pass in [height] to estimate the network speed at the time when a certain block was found.

Arguments:
1. blocks     (numeric, optional, default=120) The number of blocks, or -1 for blocks over difficulty averaging window.
2. height     (numeric, optional, default=-1) To estimate at the time of the given height.

Result:
x             (numeric) Solutions per second estimated

### `getnetworksolps ( blocks height )`
Returns the estimated network solutions per second based on the last n blocks.
Pass in [blocks] to override # of blocks, -1 specifies over difficulty averaging window.
Pass in [height] to estimate the network speed at the time when a certain block was found.

Arguments:
1. blocks     (numeric, optional, default=120) The number of blocks, or -1 for blocks over difficulty averaging window.
2. height     (numeric, optional, default=-1) To estimate at the time of the given height.

Result:
x             (numeric) Solutions per second estimated

### `prioritisetransaction <txid> <priority delta> <fee delta>`
Accepts the transaction into mined blocks at a higher (or lower) priority

Arguments:
1. "txid"       (string, required) The transaction id.
2. priority delta (numeric, required) The priority to add or subtract.
                  The transaction selection algorithm considers the tx as it would have a higher priority.
                  (priority of a transaction is calculated: coinage * value_in_satoshis / txsize)
3. fee delta      (numeric, required) The fee value (in satoshis) to add (or subtract, if negative).
                  The fee is not actually paid, only the algorithm for selecting transactions into a block
                  considers the transaction as it would have paid a higher (or lower) fee.

Result
true              (boolean) Returns true

### `submitblock "hexdata" ( "jsonparametersobject" )`
Attempts to submit new block to network.
The 'jsonparametersobject' parameter is currently ignored.
See https://en.bitcoin.it/wiki/BIP_0022 for full specification.

Arguments
1. "hexdata"    (string, required) the hex-encoded block data to submit
2. "jsonparametersobject"     (string, optional) object of optional parameters
    {
      "workid" : "id"    (string, optional) if the server provided a workid, it MUST be included with submissions
    }

Result:
"duplicate" - node already has valid copy of block
"duplicate-invalid" - node already has block, but it is invalid
"duplicate-inconclusive" - node already has block but has not validated it
"inconclusive" - node has not validated the block, it may not be on the node's current best chain
"rejected" - block was rejected as invalid
For more information on submitblock parameters and results, see: https://github.com/bitcoin/bips/blob/master/bip-0022.mediawiki#block-submission

## Multichain

### `addmergedblock "hexdata" ( "jsonparametersobject" )`
Adds a fully prepared block and its header to the current merge mining queue of this daemon.
Parameters determine the action to take if adding this block would exceed the available merge mining slots.
Default action to take if adding would exceed available space is to replace the choice with the least ROI if this block provides more.

Arguments
1. "hexdata"                     (string, required) the hex-encoded, complete, unsolved block data to add. nTime, and nSolution are replaced.
2. "name"                        (string, required) chain name symbol
3. "rpchost"                     (string, required) host address for RPC connection
4. "rpcport"                     (int,    required) port address for RPC connection
5. "userpass"                    (string, required) credentials for login to RPC

Result:
"deserialize-invalid" - block could not be deserialized and was rejected as invalid
"blocksfull"          - block did not exceed others in estimated ROI, and there was no room for an additional merge mined block

### `definechain '{"name": "BAAS", ... }'`
This defines a PBaaS chain, provides it with initial notarization fees to support its launch, and prepares it to begin running.

Arguments
```
      {
         "name"       : "xxxx",    (string, required) unique Verus ecosystem-wide name/symbol of this PBaaS chain
         "paymentaddress" : "Rxxx", (string, optional) premine and launch fee recipient
         "premine"    : "n",       (int,    optional) amount of coins that will be premined and distributed to premine address
         "initialcontribution" : "n", (int, optional) amount of coins as initial contribution
         "conversion" : "n",       (int,    optional) amount of coins that may be converted from Verus, price determined by total contribution
         "launchfee"  : "n",       (int,    optional) VRSC fee for conversion at startup, multiplied by amount, divided by 100000000
         "startblock" : "n",       (int,    optional) VRSC block must be notarized into block 1 of PBaaS chain, default curheight + 100
         "eras"       : "objarray", (array, optional) data specific to each era, maximum 3
         {
            "reward"      : "n",   (int64,  optional) native initial block rewards in each period
            "decay" : "n",         (int64,  optional) reward decay for each era
            "halving"      : "n",  (int,    optional) halving period for each era
            "eraend"       : "n",  (int,    optional) ending block of each era
            "eraoptions"   : "n",  (int,    optional) options for each era
         }
         "notarizationreward" : "n", (int,  required) default VRSC notarization reward total for first billing period
         "billingperiod" : "n",    (int,    optional) number of blocks in each billing period
         "nodes"      : "[obj, ..]", (objectarray, optional) up to 2 nodes that can be used to connect to the blockchain         [{
            "networkaddress" : "txid", (string,  optional) internet, TOR, or other supported address for node
            "paymentaddress" : "n", (int,     optional) rewards payment address
          }, .. ]
      }
```
Result:
```
{
  "txid" : "transactionid", (string) The transaction id
  "tx"   : "json",          (json)   The transaction decoded as a transaction
  "hex"  : "data"           (string) Raw data for signed transaction
}
```

### `getchaindefinition "chainname"`
Returns a complete definition for any given chain if it is registered on the blockchain. If the chain requested is NULL, chain definition of the current chain is returned.

Arguments
1. "chainname"                     (string, optional) name of the chain to look for. no parameter returns current chain in daemon.

Result:
```
  {
     "chaindefinition" : {
        "version" : "n",             (int) version of this chain definition
        "name" : "string",           (string) name or symbol of the chain, same as passed
        "address" : "string",        (string) cryptocurrency address to send fee and non-converted premine
        "chainid" : "hex-string",    (string) 40 char string that represents the chain ID, calculated from the name
        "premine" : "n",             (int) amount of currency paid out to the premine address in block #1, may be smart distribution
        "convertible" : "xxxx"       (bool) if this currency is a fractional reserve currency of Verus
        "launchfee" : "n",           (int) (launchfee * total converted) / 100000000 sent directly to premine address
        "startblock" : "n",          (int) block # on this chain, which must be notarized into block one of the chain
        "endblock" : "n",            (int) block # after which, this chain's useful life is considered to be over
        "eras" : "[obj, ...]",       (objarray) different chain phases of rewards and convertibility
        {
          "reward" : "[n, ...]",     (int) reward start for each era in native coin
          "decay" : "[n, ...]",      (int) exponential or linear decay of rewards during each era
          "halving" : "[n, ...]",    (int) blocks between halvings during each era
          "eraend" : "[n, ...]",     (int) block marking the end of each era
          "eraoptions" : "[n, ...]", (int) options for each era (reserved)
        }
        "nodes"      : "[obj, ..]",  (objectarray, optional) up to 2 nodes that can be used to connect to the blockchain          [{
             "nodeaddress" : "txid", (string,  optional) internet, TOR, or other supported address for node
             "paymentaddress" : "n", (int,     optional) rewards payment address
           }, .. ]
      }
    "bestnotarization" : {
     }
    "besttxid" : "txid"
     }
    "confirmednotarization" : {
     }
    "confirmedtxid" : "txid"
  }
```

### `getchainexports "chainname"`
Returns all pending export transfers that are not yet provable with confirmed notarizations.

Arguments
1. "chainname"                     (string, optional) name of the chain to look for. no parameter returns current chain in daemon.

Result:
```
  {
  }
```

### `getchainimports "chainname"`
Returns all imports from a specific chain.

Arguments
1. "chainname"                     (string, optional) name of the chain to look for. no parameter returns current chain in daemon.

Result:
```
  {
  }
```

### `getcrossnotarization "chainid" '["notarizationtxid1", "notarizationtxid2", ...]'`
Returns the latest PBaaS notarization transaction found in the list of transaction IDs or nothing if not found

Arguments
1. "chainid"                     (string, required) the hex-encoded chainid to search for notarizations on
2. "txidlist"                    (stringarray, optional) list of transaction ids to check in preferred order, first found is returned
3. "accepted"                    (bool, optional) accepted, not earned notarizations, default: true if on
                                                    VRSC or VRSCTEST, false otherwise

Result:
```
{
  "crosstxid" : "xxxx",        (hexstring) cross-transaction id of the notarization that matches, which is one in the arguments
  "txid" : "xxxx",             (hexstring) transaction id of the notarization that was found
  "rawtx" : "hexdata",         (hexstring) entire matching transaction data, serialized
  "newtx" : "hexdata"          (hexstring) the proposed notarization transaction with an opret and opretproof
}
```

### getcurrencystate "n"
Returns the total amount of preconversions that have been confirmed on the blockchain for the specified chain.

Arguments
   "n" or "m,n" or "m,n,o"         (int or string, optional) height or inclusive range with optional step at which to get the currency state
                                                                   If not specified, the latest currency state and height is returned

Result:
```
   [
       {
           "height": n,
           "blocktime": n,
           "currencystate": {
               "flags" : n,
               "initialratio" : n,
               "initialsupply" : n,
               "emitted" : n,
               "supply" : n,
               "reserve" : n,
               "currentratio" : n,
           "}
       },
   ]
```

### `getdefinedchains (includeexpired)`
Returns a complete definition for any given chain if it is registered on the blockchain. If the chain requested

is NULL, chain definition of the current chain is returned.

Arguments
1. "includeexpired"                (bool, optional) if true, include chains that are no longer active

Result:
```
[
  {
    "version" : "n",             (int) version of this chain definition
    "name" : "string",           (string) name or symbol of the chain, same as passed
    "address" : "string",        (string) cryptocurrency address to send fee and non-converted premine
    "chainid" : "hex-string",    (string) 40 char string that represents the chain ID, calculated from the name
    "premine" : "n",             (int) amount of currency paid out to the premine address in block #1, may be smart distribution
    "convertible" : "xxxx"       (bool) if this currency is a fractional reserve currency of Verus
    "launchfee" : "n",           (int) (launchfee * total converted) / 100000000 sent directly to premine address
    "startblock" : "n",          (int) block # on this chain, which must be notarized into block one of the chain
    "endblock" : "n",            (int) block # after which, this chain's useful life is considered to be over
    "eras" : "[obj, ...]",       (objarray) different chain phases of rewards and convertibility
    {
      "reward" : "[n, ...]",     (int) reward start for each era in native coin
      "decay" : "[n, ...]",      (int) exponential or linear decay of rewards during each era
      "halving" : "[n, ...]",    (int) blocks between halvings during each era
      "eraend" : "[n, ...]",     (int) block marking the end of each era
      "eraoptions" : "[n, ...]", (int) options for each era (reserved)
    }
    "nodes"      : "[obj, ..]",  (objectarray, optional) up to 2 nodes that can be used to connect to the blockchain      [{
         "nodeaddress" : "txid", (string,  optional) internet, TOR, or other supported address for node
         "paymentaddress" : "n", (int,     optional) rewards payment address
       }, .. ]
  }, ...
]
```

### `getinitialcurrencystate "name"`
Returns the total amount of preconversions that have been confirmed on the blockchain for the specified chain.

Arguments
   "name"                    (string, required) name or chain ID of the chain to get the export transactions for

Result:
```
   [
       {
           "flags" : n,
           "initialratio" : n,
           "initialsupply" : n,
           "emitted" : n,
           "supply" : n,
           "reserve" : n,
           "currentratio" : n,
       },
   ]
```

### `getlastimportin "fromname"`
This returns the last import transaction from the chain specified and a blank transaction template to use when making new import transactions. Since the typical use for this call is to make new import transactions from the other chain that will be then broadcast to this chain, we include the template by default.

Arguments
   "fromname"                (string, required) name of the chain to get the last import transaction in from

Result:
```
   {
       "lastimporttransaction": "hex"      Hex encoded serialized import transaction
       "lastconfirmednotarization" : "hex" Hex encoded last confirmed notarization transaction
       "importtxtemplate": "hex"           Hex encoded import template for new import transactions
       "totalimportavailable": "amount"    Total amount if native import currency available to import as native
   }
```

### `getlatestimportsout "name" "lastimporttransaction" "importtxtemplate"`
This creates and returns all new imports for the specified chain that are exported from this blockchain.

Arguments
```
{
   "name" : "string"                   (string, required) name of the chain to get the export transactions for
   "lastimporttransaction" : "hex"     (hex,    required) the last import transaction to follow, return list starts with import that spends this
   "lastconfirmednotarization" : "hex" (hex,    required) the last confirmed notarization transaction
   "importtxtemplate" : "hex"          (hex,    required) template transaction to use, so we create a transaction compatible with the other chain
   "totalimportavailable": "amount"    (number,  required) Total amount if native import currency available to import as native
}
```
Result:
UniValue array of hex transactions

### `getblocktemplate ( "jsonrequestobject" )`
If the request parameters include a 'mode' key, that is used to explicitly select between the default 'template' request or a 'proposal'.
It returns data needed to construct a block to work on.
See https://en.bitcoin.it/wiki/BIP_0022 for full specification.

Arguments:
1. "jsonrequestobject"       (string, optional) A json object in the following spec
```
     {
       "mode":"template"    (string, optional) This must be set to "template" or omitted
       "capabilities":[       (array, optional) A list of strings
           "support"           (string) client side supported feature, 'longpoll', 'coinbasetxn', 'coinbasevalue', 'proposal', 'serverlist', 'workid'
           ,...
         ]
     }
```
Result:
```
{
  "version" : n,                     (numeric) The block version
  "previousblockhash" : "xxxx",    (string) The hash of current highest block
  "finalsaplingroothash" : "xxxx", (string) The hash of the final sapling root
  "transactions" : [                 (array) contents of non-coinbase transactions that should be included in the next block
      {
         "data" : "xxxx",          (string) transaction data encoded in hexadecimal (byte-for-byte)
         "hash" : "xxxx",          (string) hash/id encoded in little-endian hexadecimal
         "depends" : [              (array) array of numbers
             n                        (numeric) transactions before this one (by 1-based index in 'transactions' list) that must be present in the final block if this one is
             ,...
         ],
         "fee": n,                   (numeric) difference in value between transaction inputs and outputs (in Satoshis); for coinbase transactions, this is a negative Number of the total collected block fees (ie, not including the block subsidy); if key is not present, fee is unknown and clients MUST NOT assume there isn't one
         "sigops" : n,               (numeric) total number of SigOps, as counted for purposes of block limits; if key is not present, sigop count is unknown and clients MUST NOT assume there aren't any
         "required" : true|false     (boolean) if provided and true, this transaction must be in the final block
      }
      ,...
  ],
  "coinbasetxn" : { ... },           (json object) information for coinbase transaction
  "target" : "xxxx",               (string) The hash target
  "mintime" : xxx,                   (numeric) The minimum timestamp appropriate for next block time in seconds since epoch (Jan 1 1970 GMT)
  "mutable" : [                      (array of string) list of ways the block template may be changed
     "value"                         (string) A way the block template may be changed, e.g. 'time', 'transactions', 'prevblock'
     ,...
  ],
  "noncerange" : "00000000ffffffff",   (string) A range of valid nonces
  "sigoplimit" : n,                 (numeric) limit of sigops in blocks
  "sizelimit" : n,                  (numeric) limit of block size
  "curtime" : ttt,                  (numeric) current timestamp in seconds since epoch (Jan 1 1970 GMT)
  "bits" : "xxx",                 (string) compressed target of next block
  "height" : n                      (numeric) The height of the next block
}
```

### `getnotarizationdata "chainid" accepted`
Returns the latest PBaaS notarization data for the specifed chainid.

Arguments
1. "chainid"                     (string, required) the hex-encoded ID or string name  search for notarizations on
2. "accepted"                    (bool, optional) accepted, not earned notarizations, default: true if on
                                                    VRSC or VRSCTEST, false otherwise

Result:
```
{
  "version" : n,                 (numeric) The notarization protocol version
}
```

### `getpendingchaintransfers "chainname"`
Returns all pending transfers for a particular chain that have not yet been aggregated into an export

Arguments
1. "chainname"                     (string, optional) name of the chain to look for. no parameter returns current chain in daemon.

Result:
```
  {
  }
```

### `paynotarizationrewards "chainid" "amount" "billingperiod"`
Adds some amount of funds to a specific billing period of a PBaaS chain, which will be released

in the form of payments to notaries whose notarizations are confirmed.

Arguments

Result:

### `refundfailedlaunch "chainid"`
Refunds any funds sent to the chain if they are eligible for refund.
This attempts to refund all transactions for all contributors.

Arguments
"chainid"            (hex or chain name, required)   the chain to refund contributions to

Result:

### `reserveexchange '[{"toreserve": 1, "recipient": "RRehdmUV7oEAqoZnzEGBH34XysnWaBatct", "amount": 5.0}]'`
This sends a Verus output as a JSON object or lists of Verus outputs as a list of objects to an address on the same or another chain.

Funds are sourced automatically from the current wallet, which must be present, as in sendtoaddress.

Arguments
```
       {
           "toreserve"      : "bool",  (bool,   optional) if present, conversion is to the underlying reserve (Verus), if false, from Verus
           "recipient"      : "Rxxx",  (string, required) recipient of converted funds or funds that failed to convert
           "amount"         : "n",     (int64,  required) amount of source coins that will be converted, depending on the toreserve flag, the rest is change
           "limit"          : "n",     (int64,  optional) price in reserve limit, below which for buys and above which for sells, execution will occur
           "validbefore"    : "n",     (int,    optional) block before which this can execute as a conversion, otherwise, it executes as a send with normal network fee
           "subtractfee"    : "bool",  (bool,   optional) if true, reduce amount to destination by the fee amount, otherwise, add from inputs to cover fee
       }
```
Result:
       "txid" : "transactionid" (string) The transaction id.

### `sendreserve '{"name": "PBAASCHAIN", "paymentaddress": "RRehdmUV7oEAqoZnzEGBH34XysnWaBatct", "amount": 5.0, "convert": 1}' (returntx)`
This sends a Verus output as a JSON object or lists of Verus outputs as a list of objects to an address on the same or another chain.

Funds are sourced automatically from the current wallet, which must be present, as in sendtoaddress.

Arguments
```
       {
           "name"           : "xxxx",  (string, optional) Verus ecosystem-wide name/symbol of chain to send to, if absent, current chain is assumed
           "paymentaddress" : "Rxxx",  (string, required) transaction recipient address
           "refundaddress"  : "Rxxx",  (string, optional) if a pre-convert is not mined in time, funds can be spent by the owner of this address
           "amount"         : "n.n",   (value,  required) coins that will be moved and sent to address on PBaaS chain, network and conversion fees additional
           "tonative"       : "false", (bool,   optional) auto-convert from Verus to PBaaS currency at market price
           "toreserve"      : "false", (bool,   optional) auto-convert from PBaaS to Verus reserve currency at market price
           "preconvert"     : "false", (bool,   optional) auto-convert to PBaaS currency at market price, this only works if the order is mined before block start of the chain
           "subtractfee"    : "bool",  (bool,   optional) if true, reduce amount to destination by the transfer and conversion fee amount. normal network fees are never subtracted
       }
       "returntx"                        (bool,   optional) defaults to false and transaction is sent, if true, transaction is signed by this wallet and returned
```
Result:
       "txid" : "transactionid" (string) The transaction id.

### `submitacceptednotarization "hextx"`
Finishes an almost complete notarization transaction based on the notary chain and the current wallet or pubkey.

If successful in submitting the transaction based on all rules, a transaction ID is returned, otherwise, NULL.

Arguments
1. "hextx"                       (hexstring, required) partial hex-encoded notarization transaction to submit transaction should have only one notarization and one opret output

Result:
txid                               (hexstring) transaction ID of submitted transaction

## Network

### `addnode "node" "add|remove|onetry"`
Attempts add or remove a node from the addnode list.
Or try a connection to a node once.

Arguments:
1. "node"     (string, required) The node (see getpeerinfo for nodes)
2. "command"  (string, required) 'add' to add a node to the list, 'remove' to remove a node from the list, 'onetry' to try a connection to the node once

### `clearbanned`
Clear all banned IPs.

### `disconnectnode "node"`
Immediately disconnects from the specified node.

Arguments:
1. "node"     (string, required) The node (see getpeerinfo for nodes)

### `getaddednodeinfo dns ( "node" )`
Returns information about the given added node, or all added nodes (note that onetry addnodes are not listed here)
If dns is false, only a list of added nodes will be provided, otherwise connected information will also be available.

Arguments:
1. dns        (boolean, required) If false, only a list of added nodes will be provided, otherwise connected information will also be available.
2. "node"   (string, optional) If provided, return information about this specific node, otherwise all nodes are returned.

Result:
```
[
  {
    "addednode" : "192.168.0.201",   (string) The node ip address
    "connected" : true|false,          (boolean) If connected
    "addresses" : [
       {
         "address" : "192.168.0.201:27485",  (string) The Verus server host and port
         "connected" : "outbound"           (string) connection, inbound or outbound
       }
       ,...
     ]
  }
  ,...
]
```

### `getconnectioncount`
Returns the number of connections to other nodes.

Result:
n          (numeric) The connection count

### `getdeprecationinfo`
Returns an object containing current version and deprecation block height. Applicable only on mainnet.

Result:
```
{
  "version": xxxxx,                      (numeric) the server version
  "subversion": "/MagicBean:x.y.z[-v]/",     (string) the server subversion string
  "deprecationheight": xxxxx,            (numeric) the block height at which this version will deprecate and shut down
}
```

### `getnettotals`
Returns information about network traffic, including bytes in, bytes out,
and current time.

Result:
```
{
  "totalbytesrecv": n,   (numeric) Total bytes received
  "totalbytessent": n,   (numeric) Total bytes sent
  "timemillis": t        (numeric) Total cpu time
}
```

### `getnetworkinfo`
Returns an object containing various state info regarding P2P networking.

Result:
```
{
  "version": xxxxx,                      (numeric) the server version
  "subversion": "/MagicBean:x.y.z[-v]/",     (string) the server subversion string
  "protocolversion": xxxxx,              (numeric) the protocol version
  "localservices": "xxxxxxxxxxxxxxxx", (string) the services we offer to the network
  "timeoffset": xxxxx,                   (numeric) the time offset
  "connections": xxxxx,                  (numeric) the number of connections
  "networks": [                          (array) information per network
  {
    "name": "xxx",                     (string) network (ipv4, ipv6 or onion)
    "limited": true|false,               (boolean) is the network limited using -onlynet?
    "reachable": true|false,             (boolean) is the network reachable?
    "proxy": "host:port"               (string) the proxy that is used for this network, or empty if none
  }
  ,...
  ],
  "relayfee": x.xxxxxxxx,                (numeric) minimum relay fee for non-free transactions in VRSC/kB
  "localaddresses": [                    (array) list of local addresses
  {
    "address": "xxxx",                 (string) network address
    "port": xxx,                         (numeric) network port
    "score": xxx                         (numeric) relative score
  }
  ,...
  ]
  "warnings": "..."                    (string) any network warnings (such as alert messages)
}
```

### `getpeerinfo`
Returns data about each connected network node as a json array of objects.

Result:
```
[
  {
    "id": n,                   (numeric) Peer index
    "addr":"host:port",      (string) The ip address and port of the peer
    "addrlocal":"ip:port",   (string) local address
    "services":"xxxxxxxxxxxxxxxx",   (string) The services offered
    "lastsend": ttt,           (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last send
    "lastrecv": ttt,           (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last receive
    "bytessent": n,            (numeric) The total bytes sent
    "bytesrecv": n,            (numeric) The total bytes received
    "conntime": ttt,           (numeric) The connection time in seconds since epoch (Jan 1 1970 GMT)
    "timeoffset": ttt,         (numeric) The time offset in seconds
    "pingtime": n,             (numeric) ping time
    "pingwait": n,             (numeric) ping wait
    "version": v,              (numeric) The peer version, such as 170002
    "subver": "/MagicBean:x.y.z[-v]/",  (string) The string version
    "inbound": true|false,     (boolean) Inbound (true) or Outbound (false)
    "startingheight": n,       (numeric) The starting height (block) of the peer
    "banscore": n,             (numeric) The ban score
    "synced_headers": n,       (numeric) The last header we have in common with this peer
    "synced_blocks": n,        (numeric) The last block we have in common with this peer
    "inflight": [
       n,                        (numeric) The heights of blocks we're currently asking from this peer
       ...
    ]
  }
  ,...
]
```

### `listbanned`
List all banned IPs/Subnets.

### `ping`
Requests that a ping be sent to all other nodes, to measure ping time.
Results provided in getpeerinfo, pingtime and pingwait fields are decimal seconds.
Ping command is handled in queue with all other commands, so it measures processing backlog, not just network ping.

### `setban "ip(/netmask)" "add|remove" (bantime) (absolute)`
Attempts add or remove a IP/Subnet from the banned list.

Arguments:
1. "ip(/netmask)" (string, required) The IP/Subnet (see getpeerinfo for nodes ip) with a optional netmask (default is /32 = single ip)
2. "command"      (string, required) 'add' to add a IP/Subnet to the list, 'remove' to remove a IP/Subnet from the list
3. "bantime"      (numeric, optional) time in seconds how long (or until when if [absolute] is set) the ip is banned (0 or empty means using the default time of 24h which can also be overwritten by the -bantime startup argument)
4. "absolute"     (boolean, optional) If set, the bantime must be a absolute timestamp in seconds since epoch (Jan 1 1970 GMT)

## Rawtransactions

### `createrawtransaction [{"txid":"id","vout":n},...] {"address":amount,...} ( locktime ) ( expiryheight )`
Create a transaction spending the given inputs and sending to the given addresses.
Returns hex-encoded raw transaction.
Note that the transaction's inputs are not signed, and
it is not stored in the wallet or transmitted to the network.

Arguments:
1. "transactions"        (string, required) A json array of json objects
```
     [
       {
         "txid":"id",    (string, required) The transaction id
         "vout":n        (numeric, required) The output number
         "sequence":n    (numeric, optional) The sequence number
       }
       ,...
     ]
```
2. "addresses"           (string, required) a json object with addresses as keys and amounts as values
```
    {
      "address": x.xxx   (numeric, required) The key is the Komodo address, the value is the VRSC amount
      ,...
    }
```
3. locktime              (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs
4. expiryheight          (numeric, optional, default=nextblockheight+20 (pre-Blossom) or nextblockheight+40 (post-Blossom)) Expiry height of transaction (if Overwinter is active)

Result:
"transaction"            (string) hex string of the transaction

### `decoderawtransaction "hexstring"`
Return a JSON object representing the serialized, hex-encoded transaction.

Arguments:
1. "hex"      (string, required) The transaction hex string

Result:
```
{
  "txid" : "id",        (string) The transaction id
  "overwintered" : bool   (boolean) The Overwintered flag
  "version" : n,          (numeric) The version
  "versiongroupid": "hex"   (string, optional) The version group id (Overwintered txs)
  "locktime" : ttt,       (numeric) The lock time
  "expiryheight" : n,     (numeric, optional) Last valid block height for mining transaction (Overwintered txs)
  "vin" : [               (array of json objects)
     {
       "txid": "id",    (string) The transaction id
       "vout": n,         (numeric) The output number
       "scriptSig": {     (json object) The script
         "asm": "asm",  (string) asm
         "hex": "hex"   (string) hex
       },
       "sequence": n     (numeric) The script sequence number
     }
     ,...
  ],
  "vout" : [             (array of json objects)
     {
       "value" : x.xxx,            (numeric) The value in VRSC
       "n" : n,                    (numeric) index
       "scriptPubKey" : {          (json object)
         "asm" : "asm",          (string) the asm
         "hex" : "hex",          (string) the hex
         "reqSigs" : n,            (numeric) The required sigs
         "type" : "pubkeyhash",  (string) The type, eg 'pubkeyhash'
         "addresses" : [           (json array of string)
           "RTZMZHDFSTFQst8XmX2dR4DaH87cEUs3gC"   (string) komodo address
           ,...
         ]
       }
     }
     ,...
  ],
  "vjoinsplit" : [        (array of json objects, only for version >= 2)
     {
       "vpub_old" : x.xxx,         (numeric) public input value in KMD
       "vpub_new" : x.xxx,         (numeric) public output value in KMD
       "anchor" : "hex",         (string) the anchor
       "nullifiers" : [            (json array of string)
         "hex"                     (string) input note nullifier
         ,...
       ],
       "commitments" : [           (json array of string)
         "hex"                     (string) output note commitment
         ,...
       ],
       "onetimePubKey" : "hex",  (string) the onetime public key used to encrypt the ciphertexts
       "randomSeed" : "hex",     (string) the random seed
       "macs" : [                  (json array of string)
         "hex"                     (string) input note MAC
         ,...
       ],
       "proof" : "hex",          (string) the zero-knowledge proof
       "ciphertexts" : [           (json array of string)
         "hex"                     (string) output note ciphertext
         ,...
       ]
     }
     ,...
  ],
}
```

### `decodescript "hex"`
Decode a hex-encoded script.

Arguments:
1. "hex"     (string) the hex encoded script

Result:
```
{
  "asm":"asm",   (string) Script public key
  "hex":"hex",   (string) hex encoded public key
  "type":"type", (string) The output type
  "reqSigs": n,    (numeric) The required signatures
  "addresses": [   (json array of string)
     "address"     (string) Komodo address
     ,...
  ],
  "p2sh","address" (string) script address
}
```

### `fundrawtransaction "hexstring"`
Add inputs to a transaction until it has enough in value to meet its out value.
This will not modify existing inputs, and will add one change output to the outputs.
Note that inputs which were signed may need to be resigned after completion since in/outputs have been added.
The inputs added will not be signed, use signrawtransaction for that.

Arguments:
1. "hexstring"    (string, required) The hex string of the raw transaction

Result:
```
{
  "hex":       "value", (string)  The resulting raw transaction (hex-encoded string)
  "fee":       n,         (numeric) The fee added to the transaction
  "changepos": n          (numeric) The position of the added change output, or -1
}
"hex"
```

### `getrawtransaction "txid" ( verbose )`
NOTE: By default this function only works sometimes. This is when the tx is in the mempool
or there is an unspent output in the utxo for this transaction. To make it always work,
you need to maintain a transaction index, using the -txindex command line option.

Return the raw transaction data.

If verbose=0, returns a string that is serialized, hex-encoded data for 'txid'.
If verbose is non-zero, returns an Object with information about 'txid'.

Arguments:
1. "txid"      (string, required) The transaction id
2. verbose       (numeric, optional, default=0) If 0, return a string, other return a json object

Result (if verbose is not set or set to 0):
"data"      (string) The serialized, hex-encoded data for 'txid'

Result (if verbose > 0):
```
{
  "hex" : "data",       (string) The serialized, hex-encoded data for 'txid'
  "txid" : "id",        (string) The transaction id (same as provided)
  "version" : n,          (numeric) The version
  "locktime" : ttt,       (numeric) The lock time
  "expiryheight" : ttt,   (numeric, optional) The block height after which the transaction expires
  "vin" : [               (array of json objects)
     {
       "txid": "id",    (string) The transaction id
       "vout": n,         (numeric)
       "scriptSig": {     (json object) The script
         "asm": "asm",  (string) asm
         "hex": "hex"   (string) hex
       },
       "sequence": n      (numeric) The script sequence number
     }
     ,...
  ],
  "vout" : [              (array of json objects)
     {
       "value" : x.xxx,            (numeric) The value in VRSC
       "n" : n,                    (numeric) index
       "scriptPubKey" : {          (json object)
         "asm" : "asm",          (string) the asm
         "hex" : "hex",          (string) the hex
         "reqSigs" : n,            (numeric) The required sigs
         "type" : "pubkeyhash",  (string) The type, eg 'pubkeyhash'
         "addresses" : [           (json array of string)
           "komodoaddress"          (string) Komodo address
           ,...
         ]
       }
     }
     ,...
  ],
  "vjoinsplit" : [        (array of json objects, only for version >= 2)
     {
       "vpub_old" : x.xxx,         (numeric) public input value in KMD
       "vpub_new" : x.xxx,         (numeric) public output value in KMD
       "anchor" : "hex",         (string) the anchor
       "nullifiers" : [            (json array of string)
         "hex"                     (string) input note nullifier
         ,...
       ],
       "commitments" : [           (json array of string)
         "hex"                     (string) output note commitment
         ,...
       ],
       "onetimePubKey" : "hex",  (string) the onetime public key used to encrypt the ciphertexts
       "randomSeed" : "hex",     (string) the random seed
       "macs" : [                  (json array of string)
         "hex"                     (string) input note MAC
         ,...
       ],
       "proof" : "hex",          (string) the zero-knowledge proof
       "ciphertexts" : [           (json array of string)
         "hex"                     (string) output note ciphertext
         ,...
       ]
     }
     ,...
  ],
  "blockhash" : "hash",   (string) the block hash
  "confirmations" : n,      (numeric) The confirmations
  "time" : ttt,             (numeric) The transaction time in seconds since epoch (Jan 1 1970 GMT)
  "blocktime" : ttt         (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
}
```

### `sendrawtransaction "hexstring" ( allowhighfees )`
Submits raw transaction (serialized, hex-encoded) to local node and network.

Also see createrawtransaction and signrawtransaction calls.

Arguments:
1. "hexstring"    (string, required) The hex string of the raw transaction)
2. allowhighfees    (boolean, optional, default=false) Allow high fees

Result:
"hex"             (string) The transaction hash in hex

### `signrawtransaction "hexstring" ( [{"txid":"id","vout":n,"scriptPubKey":"hex","redeemScript":"hex"},...] ["privatekey1",...] sighashtype )`
Sign inputs for raw transaction (serialized, hex-encoded).
The second optional argument (may be null) is an array of previous transaction outputs that this transaction depends on but may not yet be in the block chain.
The third optional argument (may be null) is an array of base58-encoded private keys that, if given, will be the only keys used to sign the transaction.

Arguments:
1. "hexstring"     (string, required) The transaction hex string
2. "prevtxs"       (string, optional) An json array of previous dependent transaction outputs
```
     [               (json array of json objects, or 'null' if none provided)
       {
         "txid":"id",             (string, required) The transaction id
         "vout":n,                  (numeric, required) The output number
         "scriptPubKey": "hex",   (string, required) script key
         "redeemScript": "hex",   (string, required for P2SH) redeem script
         "amount": value            (numeric, required) The amount spent
       }
       ,...
    ]
```
3. "privatekeys"     (string, optional) A json array of base58-encoded private keys for signing
```
    [                  (json array of strings, or 'null' if none provided)
      "privatekey"   (string) private key in base58-encoding
      ,...
    ]
```
4. "sighashtype"     (string, optional, default=ALL) The signature hash type. Must be one of
       "ALL"
       "NONE"
       "SINGLE"
       "ALL|ANYONECANPAY"
       "NONE|ANYONECANPAY"
       "SINGLE|ANYONECANPAY"
5.  "branchid"       (string, optional) The hex representation of the consensus branch id to sign with. This can be used to force signing with consensus rules that are ahead of the node's current height.

Result:
```
{
  "hex" : "value",           (string) The hex-encoded raw transaction with signature(s)
  "complete" : true|false,   (boolean) If the transaction has a complete set of signatures
  "errors" : [                 (json array of objects) Script verification errors (if there are any)
    {
      "txid" : "hash",           (string) The hash of the referenced, previous transaction
      "vout" : n,                (numeric) The index of the output to spent and used as input
      "scriptSig" : "hex",       (string) The hex-encoded signature script
      "sequence" : n,            (numeric) Script sequence number
      "error" : "text"           (string) Verification or signing error related to the input
    }
    ,...
  ]
}
```

## Util

### `createmultisig nrequired ["key",...]`
Creates a multi-signature address with n signature of m keys required.
It returns a json object with the address and redeemScript.

Arguments:
1. nrequired      (numeric, required) The number of required signatures out of the n keys or addresses.
2. "keys"       (string, required) A json array of keys which are Komodo addresses or hex-encoded public keys
```
     [
       "key"    (string) Komodo address or hex-encoded public key
       ,...
     ]
```
Result:
```
{
  "address":"multisigaddress",  (string) The value of the new multisig address.
  "redeemScript":"script"       (string) The string value of the hex-encoded redemption script.
}
```

### `estimatefee nblocks`
Estimates the approximate fee per kilobyte needed for a transaction to begin confirmation within nblocks blocks.

Arguments:
1. nblocks     (numeric)

Result:
n :    (numeric) estimated fee-per-kilobyte

-1.0 is returned if not enough transactions and blocks have been observed to make an estimate.

### `estimatepriority nblocks`
Estimates the approximate priority a zero-fee transaction needs to begin confirmation within nblocks blocks.

Arguments:
1. nblocks     (numeric)

Result:
n :    (numeric) estimated priority

-1.0 is returned if not enough transactions and
blocks have been observed to make an estimate.

### `invalidateblock "hash"`
Permanently marks a block as invalid, as if it violated a consensus rule.

Arguments:
1. hash   (string, required) the hash of the block to mark as invalid

Result:

#### jumblr_deposit "depositaddress"
#### jumblr_pause
#### jumblr_resume
#### jumblr_secret "secretaddress"
### `reconsiderblock "hash"`
Removes invalidity status of a block and its descendants, reconsider them for activation.
This can be used to undo the effects of invalidateblock.

Arguments:
1. hash   (string, required) the hash of the block to reconsider

Result:

### `validateaddress "verusaddress"`
Return information about the given Komodo address.

Arguments:
1. "verusaddress"     (string, required) The Komodo address to validate

Result:
```
{
  "isvalid" : true|false,         (boolean) If the address is valid or not. If not, this is the only property returned.
  "address" : "verusaddress",   (string) The Verus address validated
  "scriptPubKey" : "hex",       (string) The hex encoded scriptPubKey generated by the address
  "ismine" : true|false,          (boolean) If the address is yours or not
  "isscript" : true|false,        (boolean) If the key is a script
  "pubkey" : "publickeyhex",    (string) The hex value of the raw public key
  "iscompressed" : true|false,    (boolean) If the address is compressed
  "account" : "account"         (string) DEPRECATED. The account associated with the address, "" is the default account
}
```

### `verifyfile "address or identity" "signature" "filepath/filename" "checklatest"`
Verify a signed file

Arguments:
1. "t-addr or identity" (string, required) The transparent address or identity that signed the file.
2. "signature"       (string, required) The signature provided by the signer in base 64 encoding (see signfile).
3. "filename"        (string, required) The file, which must be available locally to the daemon and that was signed.
3. "checklatest"     (bool, optional)   If true, checks signature validity based on latest identity. defaults to false, which determines validity of signing height stored in signature.

Result:
true|false   (boolean) If the signature is verified or not.

### `verifyhash "address or identity" "signature" "hexhash" "checklatest"`
Verify a signed message

Arguments:
1. "t-addr or identity" (string, required) The transparent address or identity that signed the data.
2. "signature"       (string, required) The signature provided by the signer in base 64 encoding (see signmessage/signfile).
3. "hexhash"         (string, required) Hash of the message or file that was signed.
3. "checklatest"     (bool, optional)   If true, checks signature validity based on latest identity. defaults to false, which determines validity of signing height stored in signature.

Result:
true|false   (boolean) If the signature is verified or not.

### `verifymessage "address or identity" "signature" "message" "checklatest"`
Verify a signed message

Arguments:
1. "t-addr or identity" (string, required) The transparent address or identity that signed the message.
2. "signature"       (string, required) The signature provided by the signer in base 64 encoding (see signmessage).
3. "message"         (string, required) The message that was signed.
3. "checklatest"     (bool, optional)   If true, checks signature validity based on latest identity. defaults to false, which determines validity of signing height stored in signature.

Result:
true|false   (boolean) If the signature is verified or not.

### `z_validateaddress "zaddr"`
Return information about the given z address.

Arguments:
1. "zaddr"     (string, required) The z address to validate

Result:
```
{
  "isvalid" : true|false,      (boolean) If the address is valid or not. If not, this is the only property returned.
  "address" : "zaddr",         (string) The z address validated
  "type" : "xxxx",             (string) "sprout" or "sapling"
  "ismine" : true|false,       (boolean) If the address is yours or not
  "payingkey" : "hex",         (string) [sprout] The hex value of the paying key, a_pk
  "transmissionkey" : "hex",   (string) [sprout] The hex value of the transmission key, pk_enc
  "diversifier" : "hex",       (string) [sapling] The hex value of the diversifier, d
  "diversifiedtransmissionkey" : "hex", (string) [sapling] The hex value of pk_d
}
```

## Wallet

### `addmultisigaddress nrequired ["key",...] ( "account" )`
Add a nrequired-to-sign multisignature address to the wallet.
Each key is a VRSC address or hex-encoded public key.
If 'account' is specified (DEPRECATED), assign address to that account.

Arguments:
1. nrequired        (numeric, required) The number of required signatures out of the n keys or addresses.
2. "keysobject"   (string, required) A json array of VRSC addresses or hex-encoded public keys
```
     [
       "address"  (string) VRSC address or hex-encoded public key
       ...,
     ]
```
3. "account"      (string, optional) DEPRECATED. If provided, MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

Result:
"VRSC_address"  (string) A VRSC address associated with the keys.

### `backupwallet "destination"`
Safely copies `wallet.dat` to destination filename

Arguments:
1. "destination"   (string, required) The destination filename, saved in the directory set by `-exportdir` option.

### `convertpassphrase "walletpassphrase"`
Converts Verus Desktop, Agama, Verus Agama, or Verus Mobile passphrase to a private key and WIF (for import with importprivkey).

Arguments:
1. "walletpassphrase"   (string, required) Wallet passphrase

Result:
"walletpassphrase": "walletpassphrase",   (string) Wallet passphrase you entered
"address": "verus address",             (string) Address corresponding to your passphrase
"pubkey": "publickeyhex",               (string) The hex value of the raw public key
"privkey": "privatekeyhex",             (string) The hex value of the raw private key
"wif": "wif"                            (string) The private key in WIF format to use with 'importprivkey'

### `dumpprivkey "t-addr"`
Reveals the private key corresponding to 't-addr'.
Then the importprivkey can be used with this output

Arguments:
1. "t-addr"   (string, required) The transparent address for the private key

### `dumpwallet "filename" (omitemptytaddresses)`
Dumps taddr wallet keys in a human-readable format.  Overwriting an existing file is not permitted.

Arguments:
1. "filename"    (string, required) The filename, saved in folder set by verusd -exportdir option
2. "omitemptytaddresses" (boolean, optional) Defaults to false. If true, export only addresses with indexed UTXOs or that control IDs in the wallet (do not use this option without being sure that all addresses of interest are included)

Result:
"path"           (string) The full path of the destination file

### `encryptwallet "passphrase"`
WARNING: encryptwallet is disabled.
To enable it, restart zcashd with the `-experimentalfeatures` and `-developerencryptwallet` commandline options, or add these two lines to the zcash.conf file:
```
experimentalfeatures=1
developerencryptwallet=1
```
Encrypts the wallet with 'passphrase'. This is for first time encryption.
After this, any calls that interact with private keys such as sending or signing will require the passphrase to be set prior the making these calls.
Use the walletpassphrase call for this, and then walletlock call.
If the wallet is already encrypted, use the walletpassphrasechange call.
Note that this will shutdown the server.

Arguments:
1. "passphrase"    (string) The pass phrase to encrypt the wallet with. It must be at least 1 character, but should be long.

### `getaccount "VRSC_address"`
DEPRECATED. Returns the account associated with the given address.

Arguments:
1. "VRSC_address"  (string, required) The VRSC address for account lookup.

Result:
"accountname"        (string) the account address

### `getaccountaddress "account"`
DEPRECATED. Returns the current VRSC address for receiving payments to this account.

Arguments:
1. "account"       (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

Result:
"VRSC_address"   (string) The account VRSC address

### `getaddressesbyaccount "account"`
DEPRECATED. Returns the list of addresses for the given account.

Arguments:
1. "account"  (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

Result:
```
[                     (json array of string)
  "VRSC_address"  (string) a VRSC address associated with the given account
  ,...
]
```

### `getbalance ( "account" minconf includeWatchonly )`
Returns the server's total available balance.

Arguments:
1. "account"      (string, optional) DEPRECATED. If provided, it MUST be set to the empty string "" or to the string "`*`", either of which will give the total available balance. Passing any other string will result in an error.
2. minconf          (numeric, optional, default=1) Only include transactions confirmed at least this many times.
3. includeWatchonly (bool, optional, default=false) Also include balance in watchonly addresses (see 'importaddress')

Result:
amount              (numeric) The total amount in VRSC received for this account.

### `getnewaddress ( "account" )`
Returns a new VRSC address for receiving payments.

Arguments:
1. "account"        (string, optional) DEPRECATED. If provided, it MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

Result:
"VRSC_address"    (string) The new VRSC address

### `getrawchangeaddress`
Returns a new VRSC address, for receiving change.
This is for use with raw transactions, NOT normal use.

Result:
"address"    (string) The address

### `getreceivedbyaccount "account" ( minconf )`
Returns a new VRSC address, for receiving change.
This is for use with raw transactions, NOT normal use.

Result:
"address"    (string) The address

### `getreceivedbyaddress "VRSC_address" ( minconf )`
Returns the total amount received by the given VRSC address in transactions with at least minconf confirmations.

Arguments:
1. "VRSC_address"  (string, required) The VRSC address for transactions.
2. minconf             (numeric, optional, default=1) Only include transactions confirmed at least this many times.

Result:
amount   (numeric) The total amount in VRSC received at this address.

### `gettransaction "txid" ( includeWatchonly )`
Get detailed information about in-wallet transaction <txid>

Arguments:
1. "txid"    (string, required) The transaction id
2. "includeWatchonly"    (bool, optional, default=false) Whether to include watchonly addresses in balance calculation and details[]

Result:
```  
{
  "amount" : x.xxx,        (numeric) The transaction amount in VRSC
  "confirmations" : n,     (numeric) The number of confirmations
  "blockhash" : "hash",  (string) The block hash
  "blockindex" : xx,       (numeric) The block index
  "blocktime" : ttt,       (numeric) The time in seconds since epoch (1 Jan 1970 GMT)
  "txid" : "transactionid",   (string) The transaction id.
  "time" : ttt,            (numeric) The transaction time in seconds since epoch (1 Jan 1970 GMT)
  "timereceived" : ttt,    (numeric) The time received in seconds since epoch (1 Jan 1970 GMT)
  "details" : [
    {
      "account" : "accountname",  (string) DEPRECATED. The account name involved in the transaction, can be "" for the default account.
      "address" : "VRSC_address",   (string) The VRSC address involved in the transaction
      "category" : "send|receive",    (string) The category, either 'send' or 'receive'
      "amount" : x.xxx                  (numeric) The amount in VRSC
      "vout" : n,                       (numeric) the vout value
    }
    ,...
  ],
  "vjoinsplit" : [
    {
      "anchor" : "treestateref",          (string) Merkle root of note commitment tree
      "nullifiers" : [ string, ... ]      (string) Nullifiers of input notes
      "commitments" : [ string, ... ]     (string) Note commitments for note outputs
      "macs" : [ string, ... ]            (string) Message authentication tags
      "vpub_old" : x.xxx                  (numeric) The amount removed from the transparent value pool
      "vpub_new" : x.xxx,                 (numeric) The amount added to the transparent value pool
    }
    ,...
  ],
  "hex" : "data"         (string) Raw data for transaction
}
```

### `getunconfirmedbalance`
Returns the server's total unconfirmed balance

### `getwalletinfo`
Returns an object containing various wallet state info.

Result:
```
{
  "walletversion": xxxxx,     (numeric) the wallet version
  "balance": xxxxxxx,         (numeric) the total confirmed balance of the wallet in VRSC
  "reserve_balance": xxxxxxx, (numeric) for PBaaS reserve chains, the total confirmed reserve balance of the wallet in VRSC
  "unconfirmed_balance": xxx, (numeric) the total unconfirmed balance of the wallet in VRSC
  "unconfirmed_reserve_balance": xxx, (numeric) total unconfirmed reserve balance of the wallet in VRSC
  "immature_balance": xxxxxx, (numeric) the total immature balance of the wallet in VRSC
  "immature_reserve_balance": xxxxxx, (numeric) total immature reserve balance of the wallet in VRSC
  "eligible_staking_balance": xxxxxx, (numeric) eligible staking balance in VRSC
  "txcount": xxxxxxx,         (numeric) the total number of transactions in the wallet
  "keypoololdest": xxxxxx,    (numeric) the timestamp (seconds since GMT epoch) of the oldest pre-generated key in the key pool
  "keypoolsize": xxxx,        (numeric) how many new keys are pre-generated
  "unlocked_until": ttt,      (numeric) the timestamp in seconds since epoch (midnight Jan 1 1970 GMT) that the wallet is unlocked for transfers, or 0 if the wallet is locked
  "paytxfee": x.xxxx,         (numeric) the transaction fee configuration, set in VRSC/kB
  "seedfp": "uint256",        (string) the BLAKE2b-256 hash of the HD seed
}
```

### `importaddress "address" ( "label" rescan )`
Adds an address or script (in hex) that can be watched as if it were in your wallet but cannot be used to spend.

Arguments:
1. "address"          (string, required) The address
2. "label"            (string, optional, default="") An optional label
3. rescan               (boolean, optional, default=true) Rescan the wallet for transactions

Note: This call can take minutes to complete if rescan is true.

### `importprivkey "komodoprivkey" ( "label" rescan )`
Adds a private key (as returned by dumpprivkey) to your wallet.

Arguments:
1. "verusprivkey"   (string, required) The private key (see dumpprivkey)
2. "label"            (string, optional, default="") An optional label
3. rescan               (boolean, optional, default=true) Rescan the wallet for transactions

Note: This call can take minutes to complete if rescan is true.

### `importwallet "filename"`
Imports taddr keys from a wallet dump file (see dumpwallet).

Arguments:
1. "filename"    (string, required) The wallet file

### `keypoolrefill ( newsize )`
Fills the keypool.

Arguments
1. newsize     (numeric, optional, default=100) The new keypool size

### `listaccounts ( minconf includeWatchonly)`
DEPRECATED. Returns Object that has account names as keys, account balances as values.

Arguments:
1. minconf          (numeric, optional, default=1) Only include transactions with at least this many confirmations
2. includeWatchonly (bool, optional, default=false) Include balances in watchonly addresses (see 'importaddress')

Result:
```
{                      (json object where keys are account names, and values are numeric balances
  "account": x.xxx,  (numeric) The property name is the account name, and the value is the total balance for the account.
  ...
}
```

### `listaddressgroupings`
Lists groups of addresses which have had their common ownership
made public by common use as inputs or as the resulting change
in past transactions

Result:
```
[
  [
    [
      "VRSC address",     (string) The VRSC address
      amount,                 (numeric) The amount in VRSC
      "account"             (string, optional) The account (DEPRECATED)
    ]
    ,...
  ]
  ,...
]
```

### `listlockunspent`
List the locked transactions

### `listreceivedbyaccount ( minconf includeempty includeWatchonly)`
DEPRECATED. List balances by account.

Arguments:
1. minconf      (numeric, optional, default=1) The minimum number of confirmations before payments are included.
2. includeempty (boolean, optional, default=false) Whether to include accounts that haven't received any payments.
3. includeWatchonly (bool, optional, default=false) Whether to include watchonly addresses (see 'importaddress').

Result:
```
[
  {
    "involvesWatchonly" : true,   (bool) Only returned if imported addresses were involved in transaction
    "account" : "accountname",  (string) The account name of the receiving account
    "amount" : x.xxx,             (numeric) The total amount received by addresses with this account
    "confirmations" : n           (numeric) The number of confirmations of the most recent transaction included
  }
  ,...
]
```

### `listreceivedbyaddress ( minconf includeempty includeWatchonly)`
List balances by receiving address.

Arguments:
1. minconf       (numeric, optional, default=1) The minimum number of confirmations before payments are included.
2. includeempty  (numeric, optional, default=false) Whether to include addresses that haven't received any payments.
3. includeWatchonly (bool, optional, default=false) Whether to include watchonly addresses (see 'importaddress').

Result:
```
[
  {
    "involvesWatchonly" : true,        (bool) Only returned if imported addresses were involved in transaction
    "address" : "receivingaddress",  (string) The receiving address
    "account" : "accountname",       (string) DEPRECATED. The account of the receiving address. The default account is "".
    "amount" : x.xxx,                  (numeric) The total amount in VRSC received by the address
    "confirmations" : n                (numeric) The number of confirmations of the most recent transaction included
  }
  ,...
]
```

### `listsinceblock ( "blockhash" target-confirmations includeWatchonly)`
Get all transactions in blocks since block [blockhash], or all transactions if omitted

Arguments:
1. "blockhash"   (string, optional) The block hash to list transactions since
2. target-confirmations:    (numeric, optional) The confirmations required, must be 1 or more
3. includeWatchonly:        (bool, optional, default=false) Include transactions to watchonly addresses (see 'importaddress')
Result:
```
{
  "transactions": [
    "account":"accountname",       (string) DEPRECATED. The account name associated with the transaction. Will be "" for the default account.
    "address":"VRSC_address",    (string) The VRSC address of the transaction. Not present for move transactions (category = move).
    "category":"send|receive",     (string) The transaction category. 'send' has negative amounts, 'receive' has positive amounts.
    "amount": x.xxx,          (numeric) The amount in VRSC. This is negative for the 'send' category, and for the 'move' category for moves
                                          outbound. It is positive for the 'receive' category, and for the 'move' category for inbound funds.
    "vout" : n,               (numeric) the vout value
    "fee": x.xxx,             (numeric) The amount of the fee in VRSC. This is negative and only available for the 'send' category of transactions.
    "confirmations": n,       (numeric) The number of confirmations for the transaction. Available for 'send' and 'receive' category of transactions.
    "blockhash": "hashvalue",     (string) The block hash containing the transaction. Available for 'send' and 'receive' category of transactions.
    "blockindex": n,          (numeric) The block index containing the transaction. Available for 'send' and 'receive' category of transactions.
    "blocktime": xxx,         (numeric) The block time in seconds since epoch (1 Jan 1970 GMT).
    "txid": "transactionid",  (string) The transaction id. Available for 'send' and 'receive' category of transactions.
    "time": xxx,              (numeric) The transaction time in seconds since epoch (Jan 1 1970 GMT).
    "timereceived": xxx,      (numeric) The time received in seconds since epoch (Jan 1 1970 GMT). Available for 'send' and 'receive' category of transactions.
    "comment": "...",       (string) If a comment is associated with the transaction.
    "to": "...",            (string) If a comment to is associated with the transaction.
  ],
  "lastblock": "lastblockhash"     (string) The hash of the last block
}
```

### `listtransactions ( "account" count from includeWatchonly)`
Returns up to 'count' most recent transactions skipping the first 'from' transactions for account 'account'.

Arguments:
1. "account"    (string, optional) DEPRECATED. The account name. Should be "`*`".
2. count          (numeric, optional, default=10) The number of transactions to return
3. from           (numeric, optional, default=0) The number of transactions to skip
4. includeWatchonly (bool, optional, default=false) Include transactions to watchonly addresses (see 'importaddress')

Result:
```
[
  {
    "account":"accountname",       (string) DEPRECATED. The account name associated with the transaction.
                                                It will be "" for the default account.
    "address":"VRSC_address",    (string) The VRSC address of the transaction. Not present for
                                                move transactions (category = move).
    "category":"send|receive|move", (string) The transaction category. 'move' is a local (off blockchain)
                                                transaction between accounts, and not associated with an address,
                                                transaction id or block. 'send' and 'receive' transactions are
                                                associated with an address, transaction id and block details
    "amount": x.xxx,          (numeric) The amount in VRSC. This is negative for the 'send' category, and for the
                                         'move' category for moves outbound. It is positive for the 'receive' category,
                                         and for the 'move' category for inbound funds.
    "vout" : n,               (numeric) the vout value
    "fee": x.xxx,             (numeric) The amount of the fee in VRSC. This is negative and only available for the
                                         'send' category of transactions.
    "confirmations": n,       (numeric) The number of confirmations for the transaction. Available for 'send' and
                                         'receive' category of transactions.
    "blockhash": "hashvalue", (string) The block hash containing the transaction. Available for 'send' and 'receive'
                                          category of transactions.
    "blockindex": n,          (numeric) The block index containing the transaction. Available for 'send' and 'receive'
                                          category of transactions.
    "txid": "transactionid", (string) The transaction id. Available for 'send' and 'receive' category of transactions.
    "time": xxx,              (numeric) The transaction time in seconds since epoch (midnight Jan 1 1970 GMT).
    "timereceived": xxx,      (numeric) The time received in seconds since epoch (midnight Jan 1 1970 GMT). Available
                                          for 'send' and 'receive' category of transactions.
    "comment": "...",       (string) If a comment is associated with the transaction.
    "otheraccount": "accountname",  (string) For the 'move' category of transactions, the account the funds came
                                          from (for receiving funds, positive amounts), or went to (for sending funds,
                                          negative amounts).
    "size": n,                (numeric) Transaction size in bytes
  }
]
```

### `listunspent ( minconf maxconf  ["address",...] )`
Returns array of unspent transaction outputs with between minconf and maxconf (inclusive) confirmations.
Optionally filter to only include txouts paid to specified addresses.
Results are an array of Objects, each of which has:
{txid, vout, scriptPubKey, amount, confirmations}

Arguments:
1. minconf          (numeric, optional, default=1) The minimum confirmations to filter
2. maxconf          (numeric, optional, default=9999999) The maximum confirmations to filter
3. "addresses"    (string) A json array of VRSC addresses to filter
```
    [
      "address"   (string) VRSC address
      ,...
    ]
```
Result
```
[                   (array of json object)
  {
    "txid" : "txid",          (string) the transaction id
    "vout" : n,               (numeric) the vout value
    "generated" : true|false  (boolean) true if txout is a coinbase transaction output
    "address" : "address",    (string) the Zcash address
    "account" : "account",    (string) DEPRECATED. The associated account, or "" for the default account
    "scriptPubKey" : "key",   (string) the script key
    "amount" : x.xxx,         (numeric) the transaction amount in VRSC
    "confirmations" : n,      (numeric) The number of confirmations
    "redeemScript" : n        (string) The redeemScript if scriptPubKey is P2SH
    "spendable" : xxx         (bool) Whether we have the private keys to spend this output
  }
  ,...
]
```

### `lockunspent unlock [{"txid":"txid","vout":n},...]`
Updates list of temporarily unspendable outputs.
Temporarily lock (unlock=false) or unlock (unlock=true) specified transaction outputs.
A locked transaction output will not be chosen by automatic coin selection, when spending VRSC.
Locks are stored in memory only. Nodes start with zero locked outputs, and the locked output list is always cleared (by virtue of process exit) when a node stops or fails.
Also see the listunspent call

Arguments:
1. unlock            (boolean, required) Whether to unlock (true) or lock (false) the specified transactions
2. "transactions"  (string, required) A json array of objects. Each object the txid (string) vout (numeric)
```
     [           (json array of json objects)
       {
         "txid":"id",    (string) The transaction id
         "vout": n         (numeric) The output number
       }
       ,...
     ]
```
Result:
true|false    (boolean) Whether the command was successful or not

### `move "fromaccount" "toaccount" amount ( minconf "comment" )`
DEPRECATED. Move a specified amount from one account in your wallet to another.

Arguments:
1. "fromaccount"   (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.
2. "toaccount"     (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.
3. amount            (numeric) Quantity of VRSC to move between accounts.
4. minconf           (numeric, optional, default=1) Only use funds with at least this many confirmations.
5. "comment"       (string, optional) An optional comment, stored in the wallet only.

Result:
true|false           (boolean) true if successful.

### `resendwallettransactions`
Immediately re-broadcast unconfirmed wallet transactions to all peers.
Intended only for testing; the wallet code periodically re-broadcasts automatically.
Returns array of transaction ids that were re-broadcast.

### `sendfrom "fromaccount" "toVRSCaddress" amount ( minconf "comment" "comment-to" )`
DEPRECATED (use sendtoaddress). Sent an amount from an account to a VRSC address.
The amount is a real and is rounded to the nearest 0.00000001.

Arguments:
1. "fromaccount"       (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.
2. "toVRSCaddress"  (string, required) The VRSC address to send funds to.
3. amount                (numeric, required) The amount in VRSC (transaction fee is added on top).
4. minconf               (numeric, optional, default=1) Only use funds with at least this many confirmations.
5. "comment"           (string, optional) A comment used to store what the transaction is for.
                                     This is not part of the transaction, just kept in your wallet.
6. "comment-to"        (string, optional) An optional comment to store the name of the person or organization
                                     to which you're sending the transaction. This is not part of the transaction,
                                     it is just kept in your wallet.

Result:
"transactionid"        (string) The transaction id.

### `sendmany "fromaccount" {"address":amount,...} ( minconf "comment" ["address",...] )`
Send multiple times. Amounts are decimal numbers with at most 8 digits of precision.

Arguments:
1. "fromaccount"         (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.
2. "amounts"             (string, required) A json object with addresses and amounts
```
    {
      "address":amount   (numeric) The VRSC address is the key, the numeric amount in VRSC is the value
      ,...
    }
```
3. minconf                 (numeric, optional, default=1) Only use the balance confirmed at least this many times.
4. "comment"             (string, optional) A comment
5. subtractfeefromamount   (string, optional) A json array with addresses. The fee will be equally deducted from the amount of each selected address. Those recipients will receive less VRSC than you enter in their corresponding amount field. If no addresses are specified here, the sender pays the fee.
```
    [
      "address"            (string) Subtract fee from this address
      ,...
    ]
```
Result:
"transactionid"          (string) The transaction id for the send. Only 1 transaction is created regardless of the number of addresses.

### `sendtoaddress "VRSC_address" amount ( "comment" "comment-to" subtractfeefromamount )`
Send an amount to a given address. The amount is a real and is rounded to the nearest 0.00000001

Arguments:
1. "VRSC_address"  (string, required) The VRSC address to send to.
2. "amount"      (numeric, required) The amount in VRSC to send. eg 0.1
3. "comment"     (string, optional) A comment used to store what the transaction is for. This is not part of the transaction, just kept in your wallet.
4. "comment-to"  (string, optional) A comment to store the name of the person or organization to which you're sending the transaction. This is not part of the transaction, just kept in your wallet.
5. subtractfeefromamount  (boolean, optional, default=false) The fee will be deducted from the amount being sent. The recipient will receive less VRSC than you enter in the amount field.

Result:
"transactionid"  (string) The transaction id.

### `setaccount "VRSC_address" "account"`
DEPRECATED. Sets the account associated with the given address.

Arguments:
1. "VRSC_address"  (string, required) The VRSC address to be associated with an account.
2. "account"         (string, required) MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

### `settxfee amount`
Set the transaction fee per kB.

Arguments:
1. amount         (numeric, required) The transaction fee in VRSC/kB rounded to the nearest 0.00000001

Result
true|false        (boolean) Returns true if successful

### `signfile "address or identity" "filepath/filename" "curentsig"`
Generates a SHA256D hash of the file, returns the hash, and signs the hash with the private key specified

Arguments:
1. "t-addr or identity" (string, required) The transparent address or identity to use for signing.
2. "filename"        (string, required) Local file to sign
2. "cursig"          (string) The current signature of the message encoded in base 64 if multisig ID

Result:
```
{
  "hash":"hexhash"         (string) The hash of the message (SHA256, NOT SHA256D)
  "signature":"base64sig"  (string) The aggregate signature of the message encoded in base 64 if all or partial signing successful
}
```

### `signmessage "address or identity" "message" "curentsig"`
Sign a message with the private key of a t-addr or the authorities present in this wallet for an identity

Arguments:
1. "t-addr or identity" (string, required) The transparent address or identity to use for signing.
2. "message"                   (string, required) The message to create a signature of.
2. "cursig"                    (string) The current signature of the message encoded in base 64 if multisig ID

Result:
```
{
  "hash":"hexhash"         (string) The hash of the message (SHA256, NOT SHA256D)
  "signature":"base64sig"  (string) The aggregate signature of the message encoded in base 64 if all or partial signing successful
}
```

### `z_exportkey "zaddr"`
Reveals the zkey corresponding to 'zaddr'.
Then the `z_importkey` can be used with this output

Arguments:
1. "zaddr"   (string, required) The zaddr for the private key

### `z_exportviewingkey "zaddr"`
Reveals the viewing key corresponding to 'zaddr'.
Then the `z_importviewingkey` can be used with this output

Arguments:
1. "zaddr"   (string, required) The zaddr for the viewing key

### `z_exportwallet "filename" (omitemptytaddresses)`
Exports all wallet keys, for taddr and zaddr, in a human-readable format.  Overwriting an existing file is not permitted.

Arguments:
1. "filename"            (string, required) The filename, saved in folder set by verusd -exportdir option
2. "omitemptytaddresses" (boolean, optional) Defaults to false. If true, export only addresses with indexed UTXOs or that control IDs in the wallet
                                               (do not use this option without being sure that all addresses of interest are included)


### `z_getbalance "address" ( minconf )`
Returns the balance of a taddr or zaddr belonging to the node's wallet.

CAUTION: If the wallet has only an incoming viewing key for this address, then spends cannot be
detected, and so the returned balance may be larger than the actual balance.

Arguments:
1. "address"      (string) The selected address. It may be a transparent or private address.
2. minconf          (numeric, optional, default=1) Only include transactions confirmed at least this many times.

Result:
amount              (numeric) The total amount in KMD received for this address.

### `z_getmigrationstatus`
Returns information about the status of the Sprout to Sapling migration.
Note: A transaction is defined as finalized if it has at least ten confirmations.
Also, it is possible that manually created transactions involving this wallet
will be included in the result.

Result:
```
{
  "enabled": true|false,                    (boolean) Whether or not migration is enabled
  "destination_address": "zaddr",           (string) The Sapling address that will receive Sprout funds
  "unmigrated_amount": nnn.n,               (numeric) The total amount of unmigrated VRSC
  "unfinalized_migrated_amount": nnn.n,     (numeric) The total amount of unfinalized VRSC
  "finalized_migrated_amount": nnn.n,       (numeric) The total amount of finalized VRSC
  "finalized_migration_transactions": nnn,  (numeric) The number of migration transactions involving this wallet
  "time_started": ttt,                      (numeric, optional) The block time of the first migration transaction as a Unix timestamp
  "migration_txids": [txids]                (json array of strings) An array of all migration txids involving this wallet
}
```

### `z_getnewaddress ( type )`
Returns a new shielded address for receiving payments.

With no arguments, returns a Sapling address.

Arguments:
1. "type"         (string, optional, default="sapling") The type of address. One of ["sprout", "sapling"].

Result:
"VRSC_address"    (string) The new shielded address.

### `z_getoperationresult (["operationid", ... ])`
Retrieve the result and status of an operation which has finished, and then remove the operation from memory.

Arguments:
1. "operationid"         (array, optional) A list of operation ids we are interested in.  If not provided, examine all operations known to the node.

### `z_getoperationstatus (["operationid", ... ])`
Get operation status and any associated result or error data.  The operation will remain in memory.

Arguments:
1. "operationid"         (array, optional) A list of operation ids we are interested in.  If not provided, examine all operations known to the node.

### `z_gettotalbalance ( minconf includeWatchonly )`
Return the total value of funds stored in the node's wallet.

CAUTION: If the wallet contains any addresses for which it only has incoming viewing keys,
the returned private balance may be larger than the actual balance, because spends cannot
be detected with incoming viewing keys.

Arguments:
1. minconf          (numeric, optional, default=1) Only include private and transparent transactions confirmed at least this many times.
2. includeWatchonly (bool, optional, default=false) Also include balance in watchonly addresses (see 'importaddress' and 'z_importviewingkey')

Result:
```
{
  "transparent": xxxxx,     (numeric) the total balance of transparent funds
  "private": xxxxx,         (numeric) the total balance of shielded funds (in both Sprout and Sapling addresses)
  "total": xxxxx,           (numeric) the total balance of both transparent and shielded funds
}
```

### `z_importkey "zkey" ( rescan startHeight )`
Adds a zkey (as returned by z_exportkey) to your wallet.

Arguments:
1. "zkey"             (string, required) The zkey (see z_exportkey)
2. rescan             (string, optional, default="whenkeyisnew") Rescan the wallet for transactions - can be "yes", "no" or "whenkeyisnew"
3. startHeight        (numeric, optional, default=0) Block height to start rescan from

Note: This call can take minutes to complete if rescan is true.

### `z_importviewingkey "vkey" ( rescan startHeight )`
Adds a viewing key (as returned by z_exportviewingkey) to your wallet.

Arguments:
1. "vkey"             (string, required) The viewing key (see z_exportviewingkey)
2. rescan             (string, optional, default="whenkeyisnew") Rescan the wallet for transactions - can be "yes", "no" or "whenkeyisnew"
3. startHeight        (numeric, optional, default=0) Block height to start rescan from
4. zaddr               (string, optional, default="") zaddr in case of importing viewing key for Sapling

Note: This call can take minutes to complete if rescan is true.

### `z_importwallet "filename"`
Imports taddr and zaddr keys from a wallet export file (see z_exportwallet).

Arguments:
1. "filename"    (string, required) The wallet file

### `z_listaddresses ( includeWatchonly )`
Returns the list of Sprout and Sapling shielded addresses belonging to the wallet.

Arguments:
1. includeWatchonly (bool, optional, default=false) Also include watchonly addresses (see 'z_importviewingkey')

### `z_listoperationids`
Returns the list of operation ids currently known to the wallet.

Arguments:
1. "status"         (string, optional) Filter result by the operation's state e.g. "success".

### `z_listreceivedbyaddress "address" ( minconf )`
Return a list of amounts received by a zaddr belonging to the node's wallet.

Arguments:
1. "address"      (string) The private address.
2. minconf          (numeric, optional, default=1) Only include transactions confirmed at least this many times.

Result:
```
{
  "txid": "txid",           (string) the transaction id
  "amount": xxxxx,         (numeric) the amount of value in the note
  "memo": xxxxx,           (string) hexadecimal string representation of memo field
  "jsindex" (sprout) : n,     (numeric) the joinsplit index
  "jsoutindex" (sprout) : n,     (numeric) the output index of the joinsplit
  "outindex" (sapling) : n,     (numeric) the output index
  "change": true|false,    (boolean) true if the address that received the note is also one of the sending addresses
}
```

### `z_listunspent ( minconf maxconf includeWatchonly ["zaddr",...] )`
Returns array of unspent shielded notes with between minconf and maxconf (inclusive) confirmations.
Optionally filter to only include notes sent to specified addresses.
When minconf is 0, unspent notes with zero confirmations are returned, even though they are not immediately spendable.
Results are an array of Objects, each of which has:
{txid, jsindex, jsoutindex, confirmations, address, amount, memo} (Sprout)
{txid, outindex, confirmations, address, amount, memo} (Sapling)

Arguments:
1. minconf          (numeric, optional, default=1) The minimum confirmations to filter
2. maxconf          (numeric, optional, default=9999999) The maximum confirmations to filter
3. includeWatchonly (bool, optional, default=false) Also include watchonly addresses (see 'z_importviewingkey')
4. "addresses"      (string) A json array of zaddrs (both Sprout and Sapling) to filter on.  Duplicate addresses not allowed.
```
    [
      "address"     (string) zaddr
      ,...
    ]
```
Result
```
[                             (array of json object)
  {
    "txid" : "txid",          (string) the transaction id
    "jsindex" (sprout) : n,       (numeric) the joinsplit index
    "jsoutindex" (sprout) : n,       (numeric) the output index of the joinsplit
    "outindex" (sapling) : n,       (numeric) the output index
    "confirmations" : n,       (numeric) the number of confirmations
    "spendable" : true|false,  (boolean) true if note can be spent by wallet, false if address is watchonly
    "address" : "address",    (string) the shielded address
    "amount": xxxxx,          (numeric) the amount of value in the note
    "memo": xxxxx,            (string) hexademical string representation of memo field
    "change": true|false,     (boolean) true if the address that received the note is also one of the sending addresses
  }
  ,...
]
```

### `z_mergetoaddress ["fromaddress", ... ] "toaddress" ( fee ) ( transparent_limit ) ( shielded_limit ) ( memo )`
WARNING: z_mergetoaddress is disabled.
To enable it, restart zcashd with the `-experimentalfeatures` and
`-zmergetoaddress` commandline options, or add these two lines
to the `VRSC.conf` file:
```
experimentalfeatures=1
zmergetoaddress=1
```
Merge multiple UTXOs and notes into a single UTXO or note. Protected coinbase UTXOs are ignored, use `z_shieldcoinbase` to combine those into a single note.

This is an asynchronous operation, and UTXOs selected for merging will be locked.  If there is an error, they are unlocked.  The RPC call `listlockunspent` can be used to return a list of locked UTXOs.

The number of UTXOs and notes selected for merging can be limited by the caller.  If the transparent limit parameter is set to zero, and Overwinter is not yet active, the -mempooltxinputlimit option will determine the number of UTXOs. After Overwinter has activated -mempooltxinputlimit is ignored and having a transparent input limit of zero will mean limit the number of UTXOs based on the size of the transaction.  Any limit is constrained by the consensus rule defining a maximum transaction size of 100000 bytes before Sapling, and 2000000 bytes once Sapling activates.

Arguments:
1. fromaddresses         (array, required) A JSON array with addresses.
                         The following special strings are accepted inside the array:
                             - "ANY_TADDR":   Merge UTXOs from any t-addrs belonging to the wallet.
                             - "ANY_SPROUT":  Merge notes from any Sprout zaddrs belonging to the wallet.
                             - "ANY_SAPLING": Merge notes from any Sapling zaddrs belonging to the wallet.
                         While it is possible to use a variety of different combinations of addresses and the above values,
                         it is not possible to send funds from both sprout and sapling addresses simultaneously. If a special
                         string is given, any given addresses of that type will be counted as duplicates and cause an error.
```
    [
      "address"          (string) Can be a t-addr or a zaddr
      ,...
    ]
```
2. "toaddress"           (string, required) The t-addr or zaddr to send the funds to.
3. fee                   (numeric, optional, default=0.0001) The fee amount to attach to this transaction.
4. transparent_limit     (numeric, optional, default=50) Limit on the maximum number of UTXOs to merge.  Set to 0 to use node option -mempooltxinputlimit (before Overwinter), or as many as will fit in the transaction (after Overwinter).
5. shielded_limit        (numeric, optional, default=20 Sprout or 200 Sapling Notes) Limit on the maximum number of notes to merge.  Set to 0 to merge as many as will fit in the transaction.
6. "memo"                (string, optional) Encoded as hex. When toaddress is a zaddr, this will be stored in the memo field of the new note.

Result:
```
{
  "remainingUTXOs": xxx               (numeric) Number of UTXOs still available for merging.
  "remainingTransparentValue": xxx    (numeric) Value of UTXOs still available for merging.
  "remainingNotes": xxx               (numeric) Number of notes still available for merging.
  "remainingShieldedValue": xxx       (numeric) Value of notes still available for merging.
  "mergingUTXOs": xxx                 (numeric) Number of UTXOs being merged.
  "mergingTransparentValue": xxx      (numeric) Value of UTXOs being merged.
  "mergingNotes": xxx                 (numeric) Number of notes being merged.
  "mergingShieldedValue": xxx         (numeric) Value of notes being merged.
  "opid": xxx                         (string) An operationid to pass to z_getoperationstatus to get the result of the operation.
}
```

### `z_sendmany "fromaddress" [{"address":... ,"amount":...},...] ( minconf ) ( fee )`
Send multiple times. Amounts are decimal numbers with at most 8 digits of precision.
Change generated from a taddr flows to a new taddr address, while change generated from a zaddr returns to itself.
When sending coinbase UTXOs to a zaddr, change is not allowed. The entire value of the UTXO(s) must be consumed.
Before Sapling activates, the maximum number of zaddr outputs is 54 due to transaction size limits.

Arguments:
1. "fromaddress"         (string, required) The taddr or zaddr to send the funds from.
2. "amounts"             (array, required) An array of json objects representing the amounts to send.
```
    [{
      "address":address  (string, required) The address is a taddr or zaddr
      "amount":amount    (numeric, required) The numeric amount in KMD is the value
      "memo":memo        (string, optional) If the address is a zaddr, raw data represented in hexadecimal string format
    }, ... ]
```
3. minconf               (numeric, optional, default=1) Only use funds confirmed at least this many times.
4. fee                   (numeric, optional, default=0.0001) The fee amount to attach to this transaction.

Result:
"operationid"          (string) An operationid to pass to z_getoperationstatus to get the result of the operation.

### `z_setmigration enabled`
When enabled the Sprout to Sapling migration will attempt to migrate all funds from this wallet’s Sprout addresses to either the address for Sapling account 0 or the address specified by the parameter `-migrationdestaddress`.

This migration is designed to minimize information leakage. As a result for wallets with a significant Sprout balance, this process may take several weeks. The migration works by sending, up to 5, as many transactions as possible whenever the blockchain reaches a height equal to 499 modulo 500. The transaction amounts are picked according to the random distribution specified in ZIP 308. The migration will end once the wallet’s Sprout balance is below 0.01 VRSC.

### `z_shieldcoinbase "fromaddress" "tozaddress" ( fee ) ( limit )`
Shield transparent coinbase funds by sending to a shielded zaddr.  This is an asynchronous operation and utxos selected for shielding will be locked.  If there is an error, they are unlocked.  The RPC call `listlockunspent` can be used to return a list of locked utxos.  The number of coinbase utxos selected for shielding can be limited by the caller.  If the limit parameter is set to zero, and Overwinter is not yet active, the `-mempooltxinputlimit` option will determine the number of uxtos.  Any limit is constrained by the consensus rule defining a maximum transaction size of 100000 bytes before Sapling, and 2000000 bytes once Sapling activates.

Arguments:
1. "fromaddress"         (string, required) The address is a taddr or "`*`" for all taddrs belonging to the wallet.
2. "toaddress"           (string, required) The address is a zaddr.
3. fee                   (numeric, optional, default=0.0001) The fee amount to attach to this transaction.
4. limit                 (numeric, optional, default=50) Limit on the maximum number of utxos to shield.  Set to 0 to use node option -mempooltxinputlimit (before Overwinter), or as many as will fit in the transaction (after Overwinter).

Result:
```
{
  "remainingUTXOs": xxx       (numeric) Number of coinbase utxos still available for shielding.
  "remainingValue": xxx       (numeric) Value of coinbase utxos still available for shielding.
  "shieldingUTXOs": xxx        (numeric) Number of coinbase utxos being shielded.
  "shieldingValue": xxx        (numeric) Value of coinbase utxos being shielded.
  "opid": xxx          (string) An operationid to pass to z_getoperationstatus to get the result of the operation.
}
```

### `z_viewtransaction "txid"`
Get detailed shielded information about in-wallet transaction <txid>

Arguments:
1. "txid" (string, required) The transaction id

Result:
```
{
  "txid" : "transactionid",   (string) The transaction id
  "spends" : [
    {
      "type" : "sprout|sapling",      (string) The type of address
      "js" : n,                       (numeric, sprout) the index of the JSDescription within vJoinSplit
      "jsSpend" : n,                  (numeric, sprout) the index of the spend within the JSDescription
      "spend" : n,                    (numeric, sapling) the index of the spend within vShieldedSpend
      "txidPrev" : "transactionid",   (string) The id for the transaction this note was created in
      "jsPrev" : n,                   (numeric, sprout) the index of the JSDescription within vJoinSplit
      "jsOutputPrev" : n,             (numeric, sprout) the index of the output within the JSDescription
      "outputPrev" : n,               (numeric, sapling) the index of the output within the vShieldedOutput
      "address" : "zcashaddress",     (string) The Zcash address involved in the transaction
      "value" : x.xxx                 (numeric) The amount in VRSC
      "valueZat" : xxxx               (numeric) The amount in zatoshis
    }
    ,...
  ],
  "outputs" : [
    {
      "type" : "sprout|sapling",      (string) The type of address
      "js" : n,                       (numeric, sprout) the index of the JSDescription within vJoinSplit
      "jsOutput" : n,                 (numeric, sprout) the index of the output within the JSDescription
      "output" : n,                   (numeric, sapling) the index of the output within the vShieldedOutput
      "address" : "zcashaddress",     (string) The Zcash address involved in the transaction
      "recovered" : true|false        (boolean, sapling) True if the output is not for an address in the wallet
      "value" : x.xxx                 (numeric) The amount in VRSC
      "valueZat" : xxxx               (numeric) The amount in zatoshis
      "memo" : "hexmemo",             (string) Hexademical string representation of the memo field
      "memoStr" : "memo",             (string) Only returned if memo contains valid UTF-8 text.
    }
    ,...
  ],
}
```

### `zcbenchmark benchmarktype samplecount`
Runs a benchmark of the selected type samplecount times,
returning the running times of each sample.

Output:
```
[
  {
    "runningtime": runningtime
  },
  {
    "runningtime": runningtime
  }
  ...
]
```

### `zcrawjoinsplit rawtx inputs outputs vpub_old vpub_new`
inputs: a JSON object mapping {note: zcsecretkey, ...}
outputs: a JSON object mapping {zcaddr: value, ...}

DEPRECATED. Splices a joinsplit into rawtx. Inputs are unilaterally confidential.
Outputs are confidential between sender/receiver. The vpub_old and vpub_new values are globally public and move transparent value into or out of the confidential value store, respectively.

Note: The caller is responsible for delivering the output enc1 and enc2 to the appropriate recipients, as well as signing rawtxout and ensuring it is mined. (A future RPC call will deliver the confidential payments in-band on the blockchain.)

Output:
```
{
"encryptednote1": enc1,
"encryptednote2": enc2,
"rawtxn": rawtxout
}
```

### `zcrawkeygen`
DEPRECATED. Generate a zcaddr which can send and receive confidential values.

Output:
```
{
  "zcaddress": zcaddr,
  "zcsecretkey": zcsecretkey,
  "zcviewingkey": zcviewingkey,
}
```

### `zcrawreceive zcsecretkey encryptednote`
DEPRECATED. Decrypts encryptednote and checks if the coin commitments
are in the blockchain as indicated by the "exists" result.

Output:
```
{
  "amount": value,
  "note": noteplaintext,
  "exists": exists
}
```

### `zcsamplejoinsplit`
Perform a joinsplit and return the JSDescription.


compiled by Oink.vrsc@

Note: last revision date 2020-05-16.
