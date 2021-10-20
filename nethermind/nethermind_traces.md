# Nethermind Tracing

A very direct way to get access to Nethermind JSON RPC archive node is using: 

ArchiveNode.io

Once you have granted access all you need to to is, i.e:

curl -k --data '{"method":"trace_block","params":["latest"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://api.archivenode.io/n21le9m5ogypuubc2hdh8n21le9vhv6n/nethermind


Please note: https://docs.nethermind.io/nethermind/ethereum-client/json-rpc/trace#trace_block

These examples could, depending of terminal used, generate a 32700 error, if this shows up:

https://jsonlint.com/
could be of a great help. (thanks to @ralexstokes, @holiman)

Now the access to blocks have been granted, evaluation for latest block will be proposed. (In Progress)



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



