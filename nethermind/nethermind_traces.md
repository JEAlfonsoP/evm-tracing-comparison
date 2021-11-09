# Nethermind Tracing

A very direct way to get access to Nethermind JSON RPC archive node is using: 

ArchiveNode.io

Once you have granted access all you need to to is, i.e:

curl -k --data '{"method":"trace_block","params":["latest"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind


Please note: https://docs.nethermind.io/nethermind/ethereum-client/json-rpc/trace#trace_block

These examples could, depending of terminal used, generate a 32700 error, if this shows up:

https://jsonlint.com/
could be of a great help. (thanks to @ralexstokes, @holiman)

Now the access to blocks have been granted, evaluation for any block and any transaction will be proposed. 



Please find all the necessary scripts in order to access and visualize:

Block and transaction info (and traces) online:
Note: MacBook Pro terminal

trace_all_traces_transaction_hash.sh

#!/bin/bash
#display all traces from transaction hash, hash below is an example. 
transaction_hash="$(source trace_transaction_hash_by_block_index.sh)"
#display transaction_hash
echo "$transaction_hash"

curl -s -k --data '{"method":"trace_transaction","params":[["0x203abf19610ce15bc509d4b341e907ff8c5a8287ae61186fd4da82146408c28c"]],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" \
-X POST  https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind


trace_block_by_number.sh

#!/bin/bash
#display block by block number. displaying latest block number as reference 
echo $'\n\n'
latest_block_number=$(source trace_latest_block_number.sh)
#display latest_block_number
echo "latest block number:"
echo $latest_block_number
echo $'\n'

read -p "enter block number: " block_number
curl -k -s --data '{"method":"trace_block","params":['$block_number'],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" -X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind

echo $'\n\n\n'


trace_latest_block.sh

#!/bin/bash
#display latest block.

curl -k -s --data '{"method":"trace_block","params":["latest"],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" -X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind

echo $'\n\n\n'


trace_latest_block_number.sh

#!/bin/bash
#display latest block number.
echo $((`curl -k -s --data '{"method":"eth_blockNumber","params":[],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" -X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind | grep -oh "\w*0x\w*"`))


trace_transaction_by_block_index.sh
#!/bin/bash
#display a transaction by its block index or count.
#block_number and index

echo $'\n\n\n'

read -p "enter block number: " block_number
read -p "enter transaction position: " transaction_index

curl -k --data '{"method":"eth_getTransactionByBlockNumberAndIndex","params":['$block_number','$transaction_index'],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" \
-X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind

echo $'\n\n\n'


trace_transaction_count_by_block.sh
#!/bin/bash
#display transaction count by block. 
echo $'\n\n'

read -p "enter block number: " block_number
#latest_block_number=$(source trace_latest_block_number.sh)
#display latest_block_number echo $latest_block_number

echo "transactions per block:"

echo $((`curl -s -k --data '{"method":"eth_getBlockTransactionCountByNumber","params":['$block_number'],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" -X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind | grep -oh "\w*0x\w*"`))

echo $'\n\n\n'


trace_transaction_hash_by_block_index.sh
#!/bin/bash
#Display the hash 

#latest_block_number=$(source trace_latest_block_number.sh)
#display latest_block_number echo $latest_block_number

echo $'\n\n'

read -p "enter block number: " block_number
read -p "enter transaction position: " transaction_index

echo "transaction hash:: "

curl -s -k --data '{"method":"eth_getTransactionByBlockNumberAndIndex","params":['$block_number','$transaction_index'],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" \
-X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind | grep -oh "\w*0x\w*" | head -n1

echo $'\n\n'


trace_transaction_receipt_hash.sh
#!/bin/bash
#display all traces from transaction. On Progress.  trace_transaction returns error when using a variable, true if use a "hash_string".
#using ./transaction_by_block_index.sh for a comparison between clients, by the time being, should be enough.

echo $'\n\n'

transaction_hash="$(source trace_transaction_hash_by_block_index.sh)"
#| grep -oh "\w*0x\w*"
#display transaction_hash
echo $transaction_hash

tx_hash_bytes=$(echo $transaction_hash | grep -oh "\w*0x\w*")

echo $'\n'

test=$(echo $tx_hash_bytes)
echo $test

curl -s -k --data '{"method":"trace_transaction","params":[["$test"]],"id":1,"jsonrpc":"2.0"}' \
-H "Content-Type: application/json" \
-X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind

echo $'\n\n'

Block Example: (cutted - due to the fact that file is to big)


{"jsonrpc":"2.0","result":[{"action":{"callType":"call","from":"0x8e2400a8822fe2da5a8c52b7f7b412acb49813c8","gas":"0x55470","input":"0x1cff79cd000000000000000000000000f424018c3d4473e014c1def44171772059f2d720000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000001042fdc7315000000000000000000000000c3d03e4f041fd4cd388c549ee2a29a9e5075882f0000000000000000000000006b175474e89094c44da98b954eedeac495271d0f00000000000000000000000056178a0d5f301baf6cf3e1cd53d9863437345bf90000000000000000000000000000000000000000000975a0119174419c2800000000000000000000000000000000000000000000002db5e49f1f57e5bf469a000000000000000000000000000000000000000000000000000000ee97668a67c500000000000000000000000000000000000000000000000000000000616e394c330000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","to":"0xa57bd00134b2850b2a1c55860c9e9ea100fdd6cf","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x110e8","output":"0x"},"subtraces":1,"traceAddress":[],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"delegatecall","from":"0xa57bd00134b2850b2a1c55860c9e9ea100fdd6cf","gas":"0x533cd","input":"0x2fdc7315000000000000000000000000c3d03e4f041fd4cd388c549ee2a29a9e5075882f0000000000000000000000006b175474e89094c44da98b954eedeac495271d0f00000000000000000000000056178a0d5f301baf6cf3e1cd53d9863437345bf90000000000000000000000000000000000000000000975a0119174419c2800000000000000000000000000000000000000000000002db5e49f1f57e5bf469a000000000000000000000000000000000000000000000000000000ee97668a67c500000000000000000000000000000000000000000000000000000000616e394c3300000000000000000000000000000000000000000000000000000000000000","to":"0xf424018c3d4473e014c1def44171772059f2d720","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x1052f","output":"0x000000000000000000000000000000000000000000000001483b365be730df5a0000000000000000000000000000000000000000000000000045a3f202bb083e"},"subtraces":6,"traceAddress":[0],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"call","from":"0xa57bd00134b2850b2a1c55860c9e9ea100fdd6cf","gas":"0x51deb","input":"0x0902f1ac","to":"0xc3d03e4f041fd4cd388c549ee2a29a9e5075882f","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x205","output":"0x000000000000000000000000000000000000000000597da934b53ac2ec91084e000000000000000000000000000000000000000000000609bf5f587ed55f83be00000000000000000000000000000000000000000000000000000000616e38ce"},"subtraces":0,"traceAddress":[0,0],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"call","from":"0xa57bd00134b2850b2a1c55860c9e9ea100fdd6cf","gas":"0x50250","input":"0x70a0823100000000000000000000000056178a0d5f301baf6cf3e1cd53d9863437345bf9","to":"0x6b175474e89094c44da98b954eedeac495271d0f","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x25a","output":"0x000000000000000000000000000000000000000000095b95555cbac9d169124d"},"subtraces":0,"traceAddress":[0,1],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"call","from":"0xa57bd00134b2850b2a1c55860c9e9ea100fdd6cf","gas":"0x4fe01","input":"0x23b872dd00000000000000000000000056178a0d5f301baf6cf3e1cd53d9863437345bf9000000000000000000000000c3d03e4f041fd4cd388c549ee2a29a9e5075882f0000000000000000000000000000000000000000000013136f12fdab54e451a7","to":"0x6b175474e89094c44da98b954eedeac495271d0f","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x24ca","output":"0x0000000000000000000000000000000000000000000000000000000000000001"},"subtraces":0,"traceAddress":[0,2],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"call","from":"0xa57bd00134b2850b2a1c55860c9e9ea100fdd6cf","gas":"0x4d750","input":"0x022c0d9f0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001483b365be730df5a00000000000000000000000056178a0d5f301baf6cf3e1cd53d9863437345bf900000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000000","to":"0xc3d03e4f041fd4cd388c549ee2a29a9e5075882f","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x8b6e","output":"0x"},"subtraces":3,"traceAddress":[0,3],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"call","from":"0xc3d03e4f041fd4cd388c549ee2a29a9e5075882f","gas":"0x4b0ab","input":"0xa9059cbb00000000000000000000000056178a0d5f301baf6cf3e1cd53d9863437345bf9000000000000000000000000000000000000000000000001483b365be730df5a","to":"0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x229e","output":"0x0000000000000000000000000000000000000000000000000000000000000001"},"subtraces":0,"traceAddress":[0,3,0],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},{"action":{"callType":"staticcall","from":"0xc3d03e4f041fd4cd388c549ee2a29a9e5075882f","gas":"0x48c2c","input":"0x70a08231000000000000000000000000c3d03e4f041fd4cd388c549ee2a29a9e5075882f","to":"0x6b175474e89094c44da98b954eedeac495271d0f","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x25a","output":"0x0000000000000000000000000000000000000000005990bca3c8386e417559f5"},"subtraces":0,"traceAddress":[0,3,1],"transactionHash":"0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836","transactionPosition":0,"type":"call"},.....

..... "transactionPosition":124,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0xe6173c14efabac5dc97b3f3bc30c8305a9a8660e","value":"0xabef70bb4350e0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0x5944810fafc819325de3b9f9aacd4950d3f171cf902049ee6c2309a65ba70579","transactionPosition":125,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0x006f47b80a422d8ffa576d8a4d66346059ca8e04","value":"0x2c4849a9fe9d040"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0x4c2c10044a7620358fd9c527fd88f9be9096edc002e71c2d9d3c0256f705cc0e","transactionPosition":126,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0x32c1f8de2e47ee1d31ef686df82fab3104cbfe9d","value":"0x2c0d2499e5b1e20"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0x4ee19f54d178c2139a6235800de1b2fd6169ad63e556abac8d9f705fae64c94f","transactionPosition":127,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0x3f6e588590d4121a36900d3bc1aff275bc69d81d","value":"0x2c13a12d39eaf80"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0x6008f82c1eff7e73efab88fe8444c8f4c886ac5861ebcbb82b7cb63aa3caa3a3","transactionPosition":128,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0x06830abe0223d282afeafcf3e9c4c8c457997e68","value":"0x2d2967715150f80"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0x2aa175071bc2b486b33b9159964fefec8e90f43fe223809eb6f5bab343f85d27","transactionPosition":129,"type":"call"},{"action":{"callType":"call","from":"0x8bb4857c2d59d7a96b20bf51ebc29ca64bb80ffe","gas":"0x23b1b","input":"0xd4bd8947000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000052b65efdbc8c38d4cc9078e4cab9b706f6ab36b2","to":"0xfbddadd80fe7bda00b901fbaf73803f2238ae655","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x15bbe","output":"0x"},"subtraces":1,"traceAddress":[],"transactionHash":"0x32d2d2bf151365e10d637bc768b77efe05fae511b7d9906ca78eff3545d65b53","transactionPosition":130,"type":"call"},{"action":{"callType":"delegatecall","from":"0xfbddadd80fe7bda00b901fbaf73803f2238ae655","gas":"0x2165d","input":"0xd4bd8947000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000052b65efdbc8c38d4cc9078e4cab9b706f6ab36b2","to":"0x4798bb87846c2d51953c406a430b9f3a70688ddd","value":"0x0"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x13f52","output":"0x"},"subtraces":0,"traceAddress":[0],"transactionHash":"0x32d2d2bf151365e10d637bc768b77efe05fae511b7d9906ca78eff3545d65b53","transactionPosition":130,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0x7cd4c2f968aa46969275a07cda7b92ae2970a3e5","value":"0x9ba9d44e38dbb80"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0xbae60ff5cf3cde5b4a6106ad29574ed64ad96cf4cbfa4c1ee6b184c7f8288e9b","transactionPosition":131,"type":"call"},{"action":{"callType":"call","from":"0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5","gas":"0x7148","input":"0x","to":"0x3109183cf7071cc031eee95cc4e0a19f991c280a","value":"0xabd54f35f43a40"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"result":{"gasUsed":"0x0","output":"0x"},"subtraces":0,"traceAddress":[],"transactionHash":"0x5b7a555eee7a840a2077a1d2491e733a1bf709f4f8abe3b18f9ce6b63ff30149","transactionPosition":132,"type":"call"},{"action":{"author":"0x829bd824b016326a401d083b33d092293333a830","rewardType":"block","value":"0x1bc16d674ec80000"},"blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":13445744,"subtraces":0,"traceAddress":[],"type":"reward"}],"id":1}


Transaction:

{"jsonrpc":"2.0","result":{"hash":"0x4ec5e0bba687a81cdfe6e53165bfd39a57bdbd78fb7542627a58f6671dcf492b","nonce":"0x5d20","blockHash":"0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101","blockNumber":"0xcd2a70","transactionIndex":"0x1","from":"0x15dce17509846b420b1f5c158fe3d7518204abb6","to":"0x018d5c4783f5317815f6e8168942a12adde3cd3c","value":"0x0","gasPrice":"0x111efa39b6","maxPriorityFeePerGas":"0x111efa39b6","maxFeePerGas":"0x111efa39b6","gas":"0x5d15e","data":"0x2000c20d7865d3bc931502030c190100000000000000000000001766985eae0000590306020001f40000000000001546a51fb9277d4f1e2601030c151900000000000000016e06d2c25eed800000002b0302000001f400000000000000000000001766985eae01010c001500000000000000016e06d2c25eed8000020501018d5c4783f5317815f6e8168942a12adde3cd3c00000000000000016e5254680ba9b5ae","input":"0x2000c20d7865d3bc931502030c190100000000000000000000001766985eae0000590306020001f40000000000001546a51fb9277d4f1e2601030c151900000000000000016e06d2c25eed800000002b0302000001f400000000000000000000001766985eae01010c001500000000000000016e06d2c25eed8000020501018d5c4783f5317815f6e8168942a12adde3cd3c00000000000000016e5254680ba9b5ae","chainId":"0x1","type":"0x2","v":"0x1","s":"0x67b496c78b56f0174d35ad2885e4861fc353ceff86628a0815e0e9def4f100d2","r":"0x75ceb843d1ac33d321e52e3c74f3a335b996d59d191f9c9f4db829b18d0abfc4"},"id":1}


#end nethermind

