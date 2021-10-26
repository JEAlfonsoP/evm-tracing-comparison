# go-ethereum Tracing
A very direct way to get access to geth via tunneling:
JavaScript console: api modules

$ ssh -v tempuser@mon02.ethdevops.io -i ~/.ssh/shared_temp_key -L 8545:localhost:8545 (thanks to @holiman)
$ geth attahc http://localhost:8545


Once you have granted access all you need to to is, i.e:
#this will display the list of APIs available
#Note: MacBook Pro terminal

>eth
>{
  accounts: [],
  blockNumber: 13488742,
  coinbase: undefined,
  compile: {
    lll: function(),
    serpent: function(),
    solidity: function()
  },
  defaultAccount: undefined,
  defaultBlock: "latest",
  gasPrice: 164210526778,
  hashrate: 0,
  maxPriorityFeePerGas: 1325400000,
  mining: false,
  pendingTransactions: [],
  protocolVersion: undefined,
  syncing: false,
  call: function(),
  chainId: function(),
  contract: function(abi),
  createAccessList: function(),
  estimateGas: function(),
  feeHistory: function(),
  fillTransaction: function(),
  filter: function(options, callback, filterCreationErrorCallback),
  getAccounts: function(callback),
  getBalance: function(),
  getBlock: function(),
  getBlockByHash: function(),
  getBlockByNumber: function(),
  getBlockNumber: function(callback),
  getBlockTransactionCount: function(),
  getBlockUncleCount: function(),
  getCode: function(),
  getCoinbase: function(callback),
  getCompilers: function(),
  getGasPrice: function(callback),
  getHashrate: function(callback),
  getHeaderByHash: function(),
  getHeaderByNumber: function(),
  getMaxPriorityFeePerGas: function(callback),
  getMining: function(callback),
  getPendingTransactions: function(callback),
  getProof: function(),
  getProtocolVersion: function(callback),
  getRawTransaction: function(),
  getRawTransactionFromBlock: function(),
  getStorageAt: function(),
  getSyncing: function(callback),
  getTransaction: function(),
  getTransactionCount: function(),
  getTransactionFromBlock: function(),
  getTransactionReceipt: function(),
  getUncle: function(),
  getWork: function(),
  iban: function(iban),
  icapNamereg: function(),
  isSyncing: function(callback),
  namereg: function(),
  resend: function(),
  sendIBANTransaction: function(),
  sendRawTransaction: function(),
  sendTransaction: function(),
  sign: function(),
  signTransaction: function(),
  submitTransaction: function(),
  submitWork: function()
}

Scripts were coded in order to make it easy and handy for future use and comparisons.
Note that as a difference from nethermind client, were access is granted via curl command to the Json APIs, in Go Ethereum, access is granted using geth attach --exec command.


Please find all the necessary scripts in order to access and visualize:
Block and transaction info online.

eth_get_block_by_number.sh

#!/bin/bash
#JavaScript Console, Non-interactive Use: Script Mode
#https://geth.ethereum.org/docs/interface/javascrript-console
#tunneling ssh -v tempuser@mon02.ethdevops.io -i ~/.ssh/shared_temp_key -L 8545:localhost:8545
#geth attach http://localhost:8545
#>eth // list of go eth APIs


echo $'\n\n'

read -p "enter block number: " block_number

geth attach http://localhost:8545 --exec "eth.getBlock('$block_number')"

echo $'\n\n'


eth_get_transaction_by_block_index.sh

#!/bin/bash
#JavaScript Console, Non-interactive Use: Script Mode
#https://geth.ethereum.org/docs/interface/javascrript-console
#tunneling ssh -v tempuser@mon02.ethdevops.io -i ~/.ssh/shared_temp_key -L 8545:localhost:8545
#geth attach http://localhost:8545
#>eth // list of go eth APIs


echo $'\n\n'

read -p "enter block number: " block_number
read -p "enter Transaction index: " transaction_index

geth attach http://localhost:8545 --exec "eth.getBlock('$block_number','transaction_index')"

echo $'\n\n'


Now we are able to compara nethermind and geth API returned value for Block and Transaction traces.

First Block structure will be presented:





