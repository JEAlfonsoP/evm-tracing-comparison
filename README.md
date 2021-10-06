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

