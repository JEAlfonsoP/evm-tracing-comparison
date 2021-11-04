# Besu Tracing

Since, it has not been possible to access a Besu client or archive node so far. It will include examples from:

https://besu.hyperledger.org
Besu API methogs - Hyperledger Besu

eth_getBlockByNumber

Returns information about the block matching the specified block number.

Parameters
blockNumber: string - integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter

verbose: boolean - if true, returns the full transaction objects; if false, returns only the hashes of the transactions.

Returns
result: object - block object, or null when there is no block.

Example


curl HTTP

curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x68B3", true],"id":1}' http://127.0.0.1:8545

Note: Curl command is available (same as Nethermind)

Returns:

{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : {
    "number" : "0x68b3",
    "hash" : "0xd5f1812548be429cbdc6376b29611fc49e06f1359758c4ceaaa3b393e2239f9c",
    "mixHash" : "0x24900fb3da77674a861c428429dce0762707ecb6052325bbd9b3c64e74b5af9d",
    "parentHash" : "0x1f68ac259155e2f38211ddad0f0a15394d55417b185a93923e2abe71bb7a4d6d",
    "nonce" : "0x378da40ff335b070",
    "sha3Uncles" : "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "logsBloom" : "0x00000000000000100000004080000000000500000000000000020000100000000800001000000004000001000000000000000800040010000020100000000400000010000000000000000040000000000000040000000000000000000000000000000400002400000000000000000000000000000004000004000000000000840000000800000080010004000000001000000800000000000000000000000000000000000800000000000040000000020000000000000000000800000400000000000000000000000600000400000000002000000000000000000000004000000000000000100000000000000000000000000000000000040000900010000000",
    "transactionsRoot" : "0x4d0c8e91e16bdff538c03211c5c73632ed054d00a7e210c0eb25146c20048126",
    "stateRoot" : "0x91309efa7e42c1f137f31fe9edbe88ae087e6620d0d59031324da3e2f4f93233",
    "receiptsRoot" : "0x68461ab700003503a305083630a8fb8d14927238f0bc8b6b3d246c0c64f21f4a",
    "miner" : "0xb42b6c4a95406c78ff892d270ad20b22642e102d",
    "difficulty" : "0x66e619a",
    "totalDifficulty" : "0x1e875d746ae",
    "extraData" : "0xd583010502846765746885676f312e37856c696e7578",
    "size" : "0x334",
    "gasLimit" : "0x47e7c4",
    "gasUsed" : "0x37993",
    "timestamp" : "0x5835c54d",
    "uncles" : [ ],
    "transactions" : [ ],
    "baseFeePerGas" : "0x7"
  }
}


//


eth_getTransactionByBlockNumberAndIndex

Returns transaction information for the specified block number and transaction index position.

Parameters
blockNumber: string - integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter

index: string - transaction index position

Returns
result: object - transaction object, or null when there is no transaction

Example

This request returns the third transaction in the 82990 block on the Ropsten testnet. You can also view this block and transaction on Etherscan.


curl HTTP

curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["82990", "0x2"], "id":1}' http://127.0.0.1:8545

Returns:

{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : {
    "blockHash" : "0xbf137c3a7a1ebdfac21252765e5d7f40d115c2757e4a4abee929be88c624fdb7",
    "blockNumber" : "0x1442e",
    "chainId": 2018,
    "from" : "0x70c9217d814985faef62b124420f8dfbddd96433",
    "gas" : "0x3d090",
    "gasPrice" : "0x57148a6be",
    "hash" : "0xfc766a71c406950d4a4955a340a092626c35083c64c7be907060368a5e6811d6",
    "input" : "0x51a34eb8000000000000000000000000000000000000000000000029b9e659e41b780000",
    "nonce" : "0x2cb2",
    "publicKey": "0x3a514176466fa815ed481ffad09110a2d344f6c9b78c1d14afc351c3a51be33d8072e77939dc03ba44790779b7a1025baf3003f6732430e20cd9b76d953391b3",
    "raw": "0xf86401018304cb2f946295ee1b4f6dd65047762f924ecd367c17eabf8f0a8412a7b9141ba0ed2e0f715eccaab4362c19c1cf35ad8031ab1cabe71ada3fe8b269fe9d726712a06691074f289f826d23c92808ae363959eb958fb7a91fc721875ece4958114c65",
    "to" : "0xcfdc98ec7f01dab1b67b36373524ce0208dc3953",
    "transactionIndex" : "0x2",
    "value" : "0x0",
    "v" : "0x2a",
    "r" : "0xa2d2b1021e1428740a7c67af3c05fe3160481889b25b921108ac0ac2c3d5d40a",
    "s" : "0x63186d2aaefe188748bfb4b46fb9493cbc2b53cf36169e8501a5bc0ed941b484"
  }
}



