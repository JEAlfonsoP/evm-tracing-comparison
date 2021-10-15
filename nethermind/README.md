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

