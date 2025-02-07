# EVM Tracing Comparison

Any transaction on an EVM compatible protocol that involves a contract address can execute arbitrarily complex instructions, including acting on previously stored data with other accounts and addresses. The receipt of a transaction contains a status code indicating its success or failure, but no other information relevant to the context of the execution. Hence it can be necessary to ask a client/node to trace a transaction by re-executing the contract instructions and parsing the transactions in a useful structure to understand what (if any) transactions, modifications, and external code were invoked. This can be an issue because the tracing formats of each client are fragmented. This project is meant to serve as a repository of information about each client and its tracing APIs to eventually standardize key trace formats.

Clients involved: go-ethereum, Open ethereum, Nethermind, Erigon, Besu.
Trace APIs per client (suggested):

go-ethereum: eth / tracers / func API: StandardTraceBadBlockToFile, StandardTraceBlocktoFile, TraceBadBlock, TraceBlock, TraceBlockByHash, TraceBlockByNumber, TraceBlockFromFile, TraceCall, TraceChain, TraceTransaction, Backend.

Open ethereum: trace module / The Ad-hoc Tracing API: trace_call (eth_call), trace_callMany, trace_rawTransaction (eth_sendRawTransaction), trace_replayBlockTransactions, trace_replayTransaction. 
Transaction-Trace Filtering: trace_block, trace_filter, trace_get, trace_transaction.

Nethermind: trace.block, trace.call, trace.filter, trace.rawTransaction, trace.replayBlockTransactions, trace.replayTransaction, trace.transaction

Erigon: OpenEthereum trace-realted routines. 

Besu: trace_block, trace_replayBlockTransactions, trace_transaction. DEBUG Tracing APIs


Tracing prerequisites
In its simplest form, tracing a transaction entails requesting the Ethereum node to reexecute the desired transaction with varying degrees of data collection and have it return the aggregated summary for post processing. Reexecuting a transaction however has a few prerequisites to be met.

In order for an Ethereum node to reexecute a transaction, it needs to have available all historical state accessed by the transaction:

Balance, nonce, bytecode and storage of both the recipient as well as all internally invoked contracts.
Block metadata referenced during execution of both the outer as well as all internally created transactions.
Intermediate state generated by all preceding transactions contained in the same block as the one being traced.
Depending on your node’s mode of synchronization and pruning, different configurations result in different capabilities:

An archive node retaining all historical data can trace arbitrary transactions at any point in time. Tracing a single transaction also entails reexecuting all preceding transactions in the same block.
A full synced node retaining all historical data after initial sync can only trace transactions from blocks following the initial sync point. Tracing a single transaction also entails reexecuting all preceding transactions in the same block.
A fast synced node retaining only periodic state data after initial sync can only trace transactions from blocks following the initial sync point. Tracing a single transaction entails reexecuting all preceding transactions both in the same block, as well as all preceding blocks until the previous stored snapshot.
A light synced node retrieving data on demand can in theory trace transactions for which all required historical state is readily available in the network. In practice, data availability is not a feasible assumption.   https://geth.ethereum.org/docs/rpc/ns-debug#debug_traceblockbynumber

We will use options 1 and 4 (installing geth), to Go back at least 100,000 blocks.

One of the posibilities would be:
Using: 

debug_standardTraceBlockToFile,
It streams output to disk during the execution, to not blow up the memory usage on the node
It uses jsonl as output format (to allow streaming)
Uses a cross-client standardized output, so called ‘standard json’
Uses op for string-representation of opcode, instead of op/opName for numeric/string, and other simlar small differences.
has refund

From there identify Trace Transactions for each block (using: https://www.4byte.directory/signatures/)
TraceToToken 0x6e66cc38
TraceToken 0xfaa679bf
TraceChain 0xf346fd74
traceMultiSigContract 0x05e87e2a

As the search in 4byte.directory is manual (239,764 signatures) a complementary method using:

https://github.com/ethereum-lists/4bytes :

This is a collection of Ethereum Method signatures. It can be used to get the text-signatures from hex-signatures (with 4-bytes - hence the name)

A real world example:

> curl https://raw.githubusercontent.com/ethereum-lists/4bytes/master/signatures/a9059cbb
will result in this method (from the ERC-20 standard):

transfer(address,uint256)    ,example

Runnig a script file and querying the 4bytes list would be possible.

Once the Trace APIs have been identified, 
debug_traceTransaction: The traceTransaction debugging method will attempt to run the transaction in the exact same manner as it was executed on the network. It will replay any transaction that may have been executed prior to this one before it will finally attempt to execute the transaction that corresponds to the given hash.
It is one of the best options for observing any given transaction in any given time (hash)

All of the above is just one posibility that may works to obtain for each Trace API exact returns and conditions when executed. 
(Rest of the clients evalatuion / diagnostic, on going) 













