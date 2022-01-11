

# **Compare Tracing API Outputs Across Clients**

#### [**ethereum-cdap](https://github.com/ethereum-cdap)**/[cohort-one**](https://github.com/ethereum-cdap/cohort-one)**
#### [**https://github.com/ethereum-cdap/cohort-one/issues/43**](https://github.com/ethereum-cdap/cohort-one/issues/43)
##### Version draft

###### Prepared by Jose Alfonso Computer Engineer
##### DATE November 2021
#### Table of Contents
.Abstract \
.Motivation \
.Introduction \
.Scope \
.Previous Work \
.Work on Progress \
**.JSON-RPC**    
**.API**   
.Specifications \  
.Observations: Comparison Table \  
.Reading Material \  
**Thanks!**

### - **Abstract**

A comparison table for Geth and Nethermind API traces is presented. There is a constant need for API standardization between clients. For this work, Blocks and Transactions APIs are compared and analyzed back and forward into the blockchain, observing data structure and value for returned objects.  
### - **Motivation**

**I was so lucky to find Ethereum - CDAP program !** 

Being a 48 year old Computer Engineer with more than 25 years graduated, but never worked in Computer Sciences (I was developed as an Oilfield and Oil Refinery expert), finding this program, has allowed me in a serious, professional, advanced and friendly environment to catch up with the latest, best and tip of the spear blockchain technology as Ethereum it is. 

Getting involved in this project, ***Compare Tracing API Outputs Across Clients**,* understanding the need and the effort to create a standardized and consistent multiclient API method,  allowed me to get familiar with the different APIs, to access and query Ethereum, its data structures and to present in a simple but precise table some Geth and Nethermind APIs compatibilities. 

` `During this quest, I studied different EIPs, the Yellow Paper, Ethereum from inside and the most important, having met part of the team that makes all of this possible. 

### - **Introduction**

With the inminent worldwide mass adoption of Blockchain thecnology and Cryptocurrencies, the fact that Ethereum is the biggest real-world day to day blockchain network, and the beginning of Consencus era happening in weeks from now, it is understandable that development and functionallity client's UI (APIs) evolved in some level of an unstandarized configuration.

The following work is a condensated approximation to show that cannonical states between all different clients is TRUE, and that it is just a matter, of a classic, standarization phase, normally required during the mega expansion states, that in our partiulary case, Ethereum is facing today.

**Scope:** This work is a response to Ethereum-cdap / cohort-one Issue #43. Compare Tracing API outputs across clients. @TimBeiko, @Piper

### - **Previous Work (not limited to:)**

EIP 1474 Remote procedure call soecification. Paul Bochon, Erik Marks

EIP 1898 Add 'blockHash' to JSON-RPC methods which accept a default block parameter. Charles Cooper

EIP 234 Add 'blockHash' to JSON-RPC filter options. Micah Zoltu

EIP-3155 EVM trace specification. Martin Holst Swende, Marius van der Wijden


### **Work On progress (not limited to:)**

Edge cases of Ethereum JSON-RPC endpoints 2.0. Tomasz Kajetan Stan'czak, Jared Doro 

Ethereum Trace Testing. Thomas Jay Rush 

There are many other projects specifically dealing with this issue, but due to the generic reference that this work pretends to present the above mentioned should be more than enough reading material to catch up and understand the need for a final (if possible) EIP that once and for all standarizes this subject. Additional reading, if considered, would be listed at the end.

**Note:** MUST See: Nethermind, Tomasz Kajetan Stan'czak, Jared Doro. Work On Progress.

### - **JSON-RPC** 

The Ethereum JSON-RPC is a collection of methods that all clients implement. This interface allows downstream tooling and infrastructure to treat different Ethereum clients as modules that can be swapped at will. (ethereum/execution-apis , Danny Ryan). From another perpesctive is a stateless, light-weight remote procedure call (RPC) protocol.  

### **API**

Application Programming Interface. In other words, an API is the messenger that delivers a request from point a to point b and then delivers a response from b to a. 

Where the JSON-RPC APIs are for every Ethereum client a set of uniformed methods (data structures and the rules around their processing) that allow software applications, once connected to a node, interact with the Ethereum blockchain (reading blockchain data and/or sending transactions to the network). 


### - **Specifications**

1. Scripts
2. Data Example
3. Test Cases

**For the above data please consult:** <https://github.com/JEAlfonsoP/evm-tracing-comparison>

### - **Observations (from mine limited position)**

### **Comparison Table**

Clients: Geth, Nethermind



|***API*** |***Geth***|***Nethermind***|
| :- | :- | :- |
|<p></p><p>*getBlock: function()*</p><p></p>|Supported|Not Supported|
|<p></p><p>*getBlockByHash: function()*</p><p></p><p>*getBlockByNumber: function()*</p><p></p><p></p>|<p>Supported</p><p>Base Fee Per Gas, is first value returned</p>|<p>Supported</p><p>Includes in the output: Author (= miner)</p><p>Transaction Data (= input) </p>|
|<p></p><p>*getBlockNumber: function(callback)*</p><p></p>|Supported|Supported|
|<p></p><p>*getBlockTransactionCount: function()*</p><p></p>|Supported|<p>Same as getBlock. It has two queries one by Block Number and one by Block Hash</p><p></p>|
|<p></p><p>*getRawTransaction: function()*</p><p></p>|Supported|Not Supported\*|
|<p></p><p>*getTransaction: function()*</p><p>*getTransactionFromBlock: function()*</p><p></p><p></p>|Supported|Supported. Includes in the output: Transaction Data (=input)|
|<p></p><p>*getTransactionCount: function()*</p><p></p>|Supported|Supported|
|<p></p><p>*getTransactionReceipt: function()*</p><p></p>|Supported|Not Supported |
|<p></p><p>*debug.traceTransaction: function()*</p><p></p>|Supported|Supported|
|<p></p><p>debug.traceBlockByHash: function()</p><p>debug.traceBlockByNumber: function()</p><p></p>|Supported|Supported|
|<p></p><p>debug.traceCall: function()</p><p></p>|Supported|<p></p><p>Not able to run it. See links in reading material.</p><p></p>|
|<p></p><p>trace\_transaction: function()</p><p></p>|Not|Supported|


#### **Quick Notes:** 

Geth uses for eth or debug APIs “.” followed by the ”API-name”, Nethermind uses “\_” followed by the ”API-name” 

The above APIs are “eth” if not specified something different.

It is relevant to mentioned that Clients generated different list of returned values for the queries.  

**Some examples for returned values are (listed as returned):**

**eth.getBlock / eth\_getBlock**

***Geth:*** 

baseFeePerGas, difficulty, extraData, gasLimit, gasUsed, hash, logsBloom, miner, mixHash, nonce, number, parentHash, receiptsRoot, sha3Uncles, size, stateRoot, timestamp, totalDifficulty, transactions, transactionsRoot, uncles.

***Nethermind:***

autor, difficulty, extraData, gasLimit, gasUsed, hash, logsBloom, miner, mixHash, nonce, number, parentHash, receiptsRoot, sha3Uncles, size, stateRoot, totalDifficulty, timestamp, baseFeePerGas, transactions, transactionsRoot, uncles.      

**traceTransaction**

**debug.traceTransaction / debug\_traceTransaction**  

**Geth**

` `failed:

`  `gas:

`  `returnValue:,

`  `structLogs: [ {

`      `depth,

`      `gas,

`      `gasCost,

`      `op,

`      `pc,

`      `stack: []

`  `}, {

`      `depth,

`      `gas,

`      `gasCost,

`      `op,

`      `pc,

`      `stack}


**Nethermind**

{"gas":"0x672c","failed":false,"returnValue":"0x","structLogs":[{"pc":0,"op":"PUSH1","gas":24740,"gasCost":3,"depth":1,"error":null,"stack":[],"memory":[],"storage":{}},{"pc":2,"op":"PUSH1","gas":24737,"gasCost":3,"depth":1,"error":null,"stack":["0000000000000000000000000000000000000000000000000000000000000080"],"memory":[],"storage":{}},


### - **Reading Material:** 


Ethereum Yellow Paper

EIP 1474, 1898, 234, 3155, 1559, 4345, and others

<https://geth.ethereum.org> 

<https://docs.nethermind.io> 

` `<https://ethervm.io> 

<https://www.jsonrpc.org/specification> 

<https://github.com/DockBoss/Execution-API-Spec> 

<https://docs.nethermind.io/nethermind/ethereum-client/json-rpc/trace>

<https://github.com/NethermindEth/nethermind/issues/1163>



## - **Thanks!**

Tim Beiko, Piper, lightclient, holiman, ralexstokes, Pooja Ranjan, ligi, jared 


