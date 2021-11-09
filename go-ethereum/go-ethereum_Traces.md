# go-ethereum Tracing
A very direct way to get access to geth via tunneling:
JavaScript console: api modules

$ ssh -v tempuser@mon02.ethdevops.io -i ~/.ssh/shared_temp_key -L 8545:localhost:8545 (thanks to @holiman)
$ geth attahc http://localhost:8545


Once you have granted access all you need to do is, i.e:
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

geth attach http://localhost:8545 --exec "eth.getTransactionFromBlock('$block_number','$transaction_index')"

echo $'\n\n'


Now we are able to compere nethermind vs geth API returned value for Block and Transaction traces.

geth Block returned:
#Not include transactions. return full transactions = false

{
  baseFeePerGas: 55240025456,
  difficulty: 9510234485156407,
  extraData: "0xe4b883e5bda9e7a59ee4bb99e9b1bc1e0c21",
  gasLimit: 30000000,
  gasUsed: 10110960,
  hash: "0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101",
  logsBloom: "0x0320881603901e21f00022248010973440203daa9c490511518520580687d2015c3ec48070164ca20884688340201112b21090018b482040860108160ba59c1009060217480a09a8c9c9e24b9863caf2f36020c01062040440c1706ba182b1a910bb01805a52c280a0981c01014228c5a6404750822a06706b6825f008186018d23901a517002120844010484b0063b5711155990390801824058162029023001318221d3118202358687c91900c08c818005600fe828058a46a86721a8d3c0e69886a6748dc1ac4a86498802ccd254799682220086410581432758693066b03505063038ca2101982003025030540e440e9a6ac600aa162981f631140b5c745",
  miner: "0x829bd824b016326a401d083b33d092293333a830",
  mixHash: "0xb2ae1bd9eb928fbb0cb1c9923eda7f01646aa54b1eeee97e5a929068e30cbb64",
  nonce: "0xa5f3706892d614d1",
  number: 13445744,
  parentHash: "0xc20d7865d3bc9315a4917f6572bdce206574cca007752d7f5ccc851ae4ad4ce5",
  receiptsRoot: "0x9ec34288a9a20761dcde562dd6a5007a6daf76d0cb100dfcbe3f22a85fb7c0d2",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  size: 44466,
  stateRoot: "0x13f375536d4b1ff3e11fdc98de40826a359946fc6a25ac3bdc8005bc08840383",
  timestamp: 1634613461,
  totalDifficulty: 3.2679417042431288052632e+22,
  transactions: ["0x4a7e9d4aedfbb706edd101a910b257294a9ae8a0a1f903ba4593bf71ac213836", "0x4ec5e0bba687a81cdfe6e53165bfd39a57bdbd78fb7542627a58f6671dcf492b", "0xcbc3916a7e6bdb00ffa855b49e99a052fb895a58d74f3385d35b19915a8c931e", "0xd7a3413daec840a4303cbf7a2fd30d7630817a62157cede6f19d1c6854bbaca6", "0x079dac40d4a66ce7eb0eceb5e2681715ba278b7cbc704c1ad7ac3f987165fed7", "0xf9b851cca1e90fbeabb8fac2888a020f7e1feda8a2c34121a411f0b85cc1a100", "0xb2d11a91756e3c3fcc4520a584828f2c26333659c0ef7b915bc9c77d6fb1c220", "0x13bbac577b2f62960464d3aca443f7feef957eaba7d6e2ea2665efaf6e5cb6c0", "0x00fa800a2bfe643ff5776b069cf32bec8e7d0a96a2b8743939f0f9f490c8dfde", "0x33eb06910fe1fd0d30dc800685f4443b97690bce15fc210f711e0abecac307b5", "0xf586d1f2f86fd36ce23ab1c69993cf94adfab266c6b10a33b2a03858af21d3cb", "0x7298f0c93d74ca6979018ac4f93a10b017fcceff940712fab3f6c9cbf2225097", "0xd7edc4345136a9f846352b3ae7e7b9c39b3361508775d9eb4e9a65ab5707fb26", "0x45f24197c24db59c34662d3d1c6deb105a5af09f552bb80c0be86fc2662aec4e", "0x7f1dd101e89494ed9f7f7c0c5644fdc854ca900ed9286cb9156a25810bc21444", "0x47d4fb7f9b78da9d27a12efa6df7af5b09ed08ef69f5e8abe7b3247a7cf0f287", "0xf39a0a698415ba081b79a4d1d86ec395e59b0cacde3cedb1a875af1e270ed741", "0x8a47a3605386500c716641fec78f49dabb1adf4829c7001d1c937838a382882f", "0x3cd8ca6c95ca302fc9890937f12c7485ec188b097ffef1fb4b3ac25dbba1ba28", "0xe703b8eccd5a77fa150145802d07b211e56f9fc3771d9c7a3cea4b57a86df8fd", "0x736807171bf1fc4d131529bc91011cec90d0aa5dbb397962bb200a5563b04f57", "0x80b391c1a010bea2b727d740e7c0d47bb312eceac65db7a5d46a4e7b6cbdc866", "0xdd9396dc005112276d7f6c5846485dc710ef99731060ad5664f0a27c6b28bbc5", "0x2e710f484cc29bd4fdd3fda20e0efea84ab66202408c2ea4a9659acd631e1aec", "0x23dc1d9cbba37332d7f929758b04e8358520bad242a749a7d67466c358ae5c2c", "0x0e0d352bdb071568715f258bb31d79b9c50d6bd63ccd66f408dda198a1e32652", "0xa662ebe5608c44ad2dc12d54cfa6ccef61f996ac55831b3011024b9629e8b62a", "0xd41b36ee8f5c2a7b7ef2f8b18b250fcce29c350e2799538faac18ae23ddb2371", "0x379d5e5e2a70ae844eb6537622435c2165991fb4a1f53ad1e6f082cc7c255b8e", "0x098ae5ab6e4209f7a780b91a45f3d4e3db824bb643c4416cd33945974397a56e", "0x2b3c4b9728f56d795842b0f393d808fa6e2c8768bf91bce01fb8a4020452df19", "0xfa978e6542026aaa485a40a7c9e141f1f97dc90ce0b6e92fc27c3e1a2764f122", "0xd5356234d8daa1f35ea1f9caed73b714a4058336e78e51d461247869b85df63e", "0x2644f83f6f441eb8e0a2b9be12da9c724a88f715553077605c177a97e678df6f", "0x7757b9beb18b4a73cf8986405798208a61285eb381540f1e876dad676f19d938", "0x09b67b15334868c6ab8d27c73ecbf3efe1c4adaee64e7236044654147bdc7b68", "0x881079f704ee04de0bc7229d9163c9379d324b8873af8fd2b8749af0151c3c67", "0x947edc4e832e676145436ea5d07a99bc119548b086bbf231943a506a8023f716", "0x0af1505338a6c653c6a3ac64b02ab8ede5afa5466d376ff98460dbdc38f34e75", "0x09d4edf5c4cc436542dd75797e7c18c6b72858bd556b49d1b1b8625a8e41053c", "0x4f843422f53b01aa28a2e8f3531449f920b11482870ee7dd7808ce8574f551d7", "0xd3395110c2ad7a6258d29f9bdeaf622f38af46b4d0b7c11993be20106a523a0e", "0x9f6795aff9c485c32ae5dc6c21bdb5eb3f8f1e26d8bac3f6327fcb26cd9343f7", "0x9c3dac3605352734d8bfc0dddc4ed3c94bdccb62c5e8b13d91dc5218f0809478", "0x9e8744355277d758c3c5b183fca5e73597b566529ec34def3a55ad9ab3c9e90a", "0xa84782938871e514f54ff8b0b63e5c9bab4242ac338b27d883001a7ca30f1de6", "0x3e447b0495e24cfe86a7909a22d83605656a00971eb8806f968f8188971ae25a", "0x25bebaf872303ea70f4c052c7e7a07c8d3d98c0eafdae030b93eba3b55f3b2ad", "0x328b3a73d56bbe60fc4eeee546fc550c621f787abab0bbe1eaa885b78fe9315a", "0x3e2a8ecd55a3e685be510e893e06169539d2805aadc7419c54c64106018a00bb", "0x69009584600694cc51a4702351b7f784301bba7bf3111de5d7382221ddef8654", "0xff629c62e8339a740e9b1c2e03b8b201c000efa0dcf0360622d3671a32b312dd", "0x4603bdefaf48b03aac73e32952acfecb370957c37449a0a0fcebda3ab71134b0", "0xd1252f7de02a332456273e7fb20bfa56bf3612ac8b7744172cc8542b064d5815", "0x6a9ce4420a5d630e93ab1c99aea5994b93fad8030623ce095b7000564ccf13dc", "0x923d8538d2b9c76b7697757d60f0e517e97bbe7630ffc0283281ad9600dc4b08", "0xf1e4762881af6a4cc2fa5ebd7ed383fedac6a9f9f55572fd3a7b39b728e70186", "0xf6395e2acf5b02c50ecc191c09fa5c08dcb0e17a2766a33a53d19afe0028c5f0", "0x4b01546b79f69adf6c8c3b32fa21c116c401b011caf7f632a118653d072a8367", "0xb2d2cbed56c35fb2c843021b9b7bbc9c0cb227ed7d330951af31092d1a1302ee", "0x48ac957623d47ededc4e50fd6eca11d0eaaabefcfe170e275810e7d0a819f6e8", "0xb2d5919b20ae8497c59e60cdca44bb742261a24d7b2df4c4e544391643eb7ee7", "0xa1022ffb966a8f1e2321a35de7f07d8b3799f37b5109df9dc793a8c42aac4546", "0xf308ad07fe42dc8262fc585aaeffd1f4decfb10f6c7ba426c061cbcf5c737388", "0x8ea94557c46d2fd509001edb16f667f8323e1df1499bc6f6fb9eeb1420b08f0d", "0x993fd993004e423a6902e79655fe3d453a92b3163c318ef6f77d956ba57db2c9", "0x830407307a8133033369ebea7d10c0207cdff45b97eba2836a17f8038f574e89", "0x72eea4593d87d9734ec0eaf653965e35047c3753b96865126783b95abf841df2", "0x0b0b44f116da007cb64244e2306f609a2552effd66a3191c058348f48dfbe541", "0x00f119213598972e776298e2d3c9947e809162adc2b1a8a389dc68f343e90ecf", "0x8aa5c653cc66c195d9de6b0a3518630d738209a208911946015f0d63bcc5165d", "0xadd5f7a55c01ce9ce1e24873eb6bf692a4415761331ac5217cdacfcb80b84a76", "0xe64851c06711b244daaea5bd95e255220bdcf3e54bfc936ad5b581362f52d4f6", "0x4c441decb64b62a19287773562cfd8566b1f4c6791cd74378ad187eb88fcb04f", "0x90274afdfe8acf344ee5aa4823d11d1c01c07f304559afe77bbd88ccc68599f9", "0x73bdff66771cd812f5639eeaef6cfc9ad3063fc8be5d05aa11eac5c7af77c4e4", "0x493d6edf84e4f711fa6cba0c2004f518f6b86b1a99f4b5dbca142e3c44eab667", "0xe9bcc05d86ea98683db9c2a8a6fbd22764308e84e0bafbe82e5ccd3b47588c51", "0x2e7ffadff64c9f80bbbb5ecf8bf3ccaee285634151c9b8e09806dec9f4523bc5", "0xa023ffde79a6e1ba8aa96d21d923580bb4ec311d8d7987a229136c917daa4428", "0x32eac2c854484b3bfd901f060860ea6093f5fffc9540a5853f54916ae5875825", "0xf00d7c58d40e410b3d68486d2808d8bf93422a75dae576720036d2ac7656b854", "0x4ff3cabf5832dd7930e5be86abeabe5f19e3d2b8dad693d4876b55f377bf2aca", "0x7bcff73ba4d99ca58153d8966c2002aa80daea97b562dd3f9c29bf6a4f7d729b", "0x8679831e764d2ccacf8b8429c9874f5b1586822844c56e7d4612cf6f99e18885", "0x8092c9b7b6a346bcb6a58386ceb73d10be798540823bc1eeeb3c85f10d98828f", "0x201b3a608952310e1998fa3bde44162e5b8b819def1c1cce74b27659fd664c4c", "0x461bcefcc859bc3773403f28dabbd5e37966699b21cb6d2c4a4101057e1adf04", "0x26d40ce1a2cdbebd45ffeadf38a0a322aeae47772e7ea91f2973d53bb1453e3f", "0x84e4d02ab6cb9fe9bd0fe2f506a44cbfd1aeb30ca888afb3acddce4d759659b0", "0xf9fcebbde049c42b35a405575cbdc14d6c40712ae368e7e760a2946201ad9ffd", "0x13e580bd03b012f605c1f7e79446098013b1aeacaec3c81d4d292965d3818a9c", "0x9123b90fc91cde947d2ade22cf98dce6bc52bfb3eb76c65afbf46200bbeeaf88", "0xa1a5fcb7fee1b7253f3cc8286b3733fb3808f7190f96f424419a559c0f64e5d7", "0x832f353db3ad62dc0ad6985f357ca14262135839001a72d57e7bc97230206949", "0x836eb81c8fba7b98e79d627289c7b5f4bdd372e8944b4863c0f3f27507f020c2", "0xd7b02c0d8844c9a95bb20293173443b0407763ec2bbb4a43c8b79a0e914bfca7", "0xe4e5666a1d5f00620548811addf490a1ce5282a15ec970f2baaee6bbe3d96d7f", "0x7fda0410bfddc105906a9ee48315d3f7af5c86b91dd072a09338a9fbe12970d4", "0x0dee64bb3c052f58e938db94b80cab92f3bda3ad499092da0aa91ea393cf7fc2", "0x47659652944ac931d08552c1b51dbbb604b709c27687e99a78407918145e1fb6", "0xdca670fd6d115e11ec3752475a849f2a82a4cd7144a3996262d8f499ce29aad9", "0x4267858a1b8e8c5f0b3e826c4c896d36e05aa49c1605d9c65f4267e8ed66789c", "0x87bd638d197b9a58d10efd802fcd3c2f8a2c2a72484accb547cddb046b728df8", "0x788aced7ff4efd162497905a77b30ec917a3694bdf089f6dd887914805f833f1", "0x578d0133557d9b676383882011acbf0dac912c63e14a9a1d3c18abd9e09161ab", "0xc86fec7d21839781b47921bbe0b19a803bb9218aab1767a1dedeadaa465c793a", "0x7824d434c4c9ea5bcb8283cc132f59f4e1d73fafa8d779beae05656044914b09", "0x3350dd83f2f3d8d8b7f85d01b9823127ef20e3504a7032313a1b36ea0dd6ca3c", "0xc62e81fc7fcc3605121e380c4b36d0142ddba0c6673600b743a216237c6611ad", "0xef055e11521456dcc33b067d9017738320db72c895f7b1a2b7199e3ad8046484", "0x2010503300eb6e9395fd8ac982a96de74dc487d6949be22e006780bdbb4fa1e3", "0xf68fde4aadd74d72c2a9525dce8f54eda7d8544a99bc1eb52971644967cd5a4a", "0xdc6e2bba0bc8ae58b632b83c4787a4bc4efa91276d45ee6df310835854382659", "0x599c1f6bb931fac31256f31300d891a42bb27ff4cf23fc28edb83dad08e662a0", "0xa2d20042606025ae8a4483bcf0c4d17700e81b0756370b874423d99307450016", "0x733a7fa647a9f97e5674375a992d8ab29d4f54663d2f88d084ad4ea3191065e7", "0x411671c7c2d040ec52d67ac157aa95f9b10a429a4bf7f3ff411193aac2a727d6", "0x39edb8d9bb64169e708c03078d9299dbb9909ae15c16b7e2214a61f8317bbe24", "0xb3a6dc8f622aa9aca0e5b8866aa72db4bb87d303f6d1fa6446aa5fb5621704b0", "0x33b6d00659a82c762a3a7ac3087a4f455e43bcd9a304d58f3699c4b4918cbd03", "0xe612c44ab18abadbfece7058fc10d1ed939015e4bc6cde8fb6f803f16789a4ac", "0xc43e261f6d7dbb24918679936ff424d6006cdb0e66b7dd9fc0c42d3255f17d4c", "0xf92c744d8cb514eca8ffc0320b34c510bb328cbb2aa75671d51a6d9cdf6be4f9", "0x4904195aa0a67bbdb7c961536ec82e0eb61809584c9dca24bfa9754d2d95b1c8", "0x5944810fafc819325de3b9f9aacd4950d3f171cf902049ee6c2309a65ba70579", "0x4c2c10044a7620358fd9c527fd88f9be9096edc002e71c2d9d3c0256f705cc0e", "0x4ee19f54d178c2139a6235800de1b2fd6169ad63e556abac8d9f705fae64c94f", "0x6008f82c1eff7e73efab88fe8444c8f4c886ac5861ebcbb82b7cb63aa3caa3a3", "0x2aa175071bc2b486b33b9159964fefec8e90f43fe223809eb6f5bab343f85d27", "0x32d2d2bf151365e10d637bc768b77efe05fae511b7d9906ca78eff3545d65b53", "0xbae60ff5cf3cde5b4a6106ad29574ed64ad96cf4cbfa4c1ee6b184c7f8288e9b", "0x5b7a555eee7a840a2077a1d2491e733a1bf709f4f8abe3b18f9ce6b63ff30149"],
  transactionsRoot: "0xc9a37bc6d35ffdc327cfe0816dc82b87588fef716d8f7a50152e240d23d1f1fd",
  uncles: []
}


geth Transaction returned:

{
  accessList: [],
  blockHash: "0x50bbefe272278d0acb397b0b4b9de902eebc8c4e7503cb33b17595008a09d101",
  blockNumber: 13445744,
  chainId: "0x1",
  from: "0x15dce17509846b420b1f5c158fe3d7518204abb6",
  gas: 381278,
  gasPrice: 73534159286,
  hash: "0x4ec5e0bba687a81cdfe6e53165bfd39a57bdbd78fb7542627a58f6671dcf492b",
  input: "0x2000c20d7865d3bc931502030c190100000000000000000000001766985eae0000590306020001f40000000000001546a51fb9277d4f1e2601030c151900000000000000016e06d2c25eed800000002b0302000001f400000000000000000000001766985eae01010c001500000000000000016e06d2c25eed8000020501018d5c4783f5317815f6e8168942a12adde3cd3c00000000000000016e5254680ba9b5ae",
  maxFeePerGas: 73534159286,
  maxPriorityFeePerGas: 73534159286,
  nonce: 23840,
  r: "0x75ceb843d1ac33d321e52e3c74f3a335b996d59d191f9c9f4db829b18d0abfc4",
  s: "0x67b496c78b56f0174d35ad2885e4861fc353ceff86628a0815e0e9def4f100d2",
  to: "0x018d5c4783f5317815f6e8168942a12adde3cd3c",
  transactionIndex: 1,
  type: "0x2",
  v: "0x1",
  value: 0
}


#end geth




