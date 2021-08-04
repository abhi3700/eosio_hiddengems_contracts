# Token contract
## Brief
* It is a token contract which is to 
	- [x] create tokens with issuer & max_supply
	- [x] issue tokens to issuer only
	- [x] retire tokens from supply
	- [x] transfer tokens from one account to another
	- [x] open account with even zero balance
	- [x] close account with zero balance

## About
* contract name - `hiddengemscoin`
* contract's account name - `hiddengemsss`
* action
	- `create`
	- `issue`
	- `retire`
	- `transfer`
	- `open`
	- `close`
* table
	- `stats`
	- `accounts`

## Compile
```console
$ eosio-cpp hiddengemscoin.cpp -o hiddengemscoin.wasm
Warning, action <create> does not have a ricardian contract
Warning, action <issue> does not have a ricardian contract
Warning, action <retire> does not have a ricardian contract
Warning, action <transfer> does not have a ricardian contract
Warning, action <open> does not have a ricardian contract
Warning, action <close> does not have a ricardian contract
/mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.cpp:5:22: warning: abigen warning (Action <create> does not have a ricardian contract)
void hiddengemscoin::create( const name&   issuer,
                     ^
Warning, action <create> does not have a ricardian contract
/mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.cpp:27:22: warning: abigen warning (Action <issue> does not have a ricardian contract)
void hiddengemscoin::issue( const name& to, const asset& quantity, const string& memo )
                     ^
Warning, action <issue> does not have a ricardian contract
/mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.cpp:53:22: warning: abigen warning (Action <retire> does not have a ricardian contract)
void hiddengemscoin::retire( const asset& quantity, const string& memo )
                     ^
Warning, action <retire> does not have a ricardian contract
/mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.cpp:77:22: warning: abigen warning (Action <transfer> does not have a ricardian contract)
void hiddengemscoin::transfer( const name&    from,
                     ^
Warning, action <transfer> does not have a ricardian contract
/mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.cpp:129:22: warning: abigen warning (Action <open> does not have a ricardian contract)
void hiddengemscoin::open( const name& owner, const symbol& symbol, const name& ram_payer )
                     ^
Warning, action <open> does not have a ricardian contract
/mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.cpp:149:22: warning: abigen warning (Action <close> does not have a ricardian contract)
void hiddengemscoin::close( const name& owner, const symbol& symbol )
                     ^
Warning, action <close> does not have a ricardian contract
6 warnings generated.
```

## Deploy
* deploy contract
```console
$ cleost set contract hiddengemsss ./
Reading WASM from /mnt/f/Coding/github_repos/eosio_hiddengems_contracts/hiddengemscoin/hiddengemscoin.wasm...
Publishing contract...
executed transaction: c74311663c3e0c912bdbcafcd1253e6cfb6911cf2a967ef8b3df6de12731bcca  19376 bytes  2175 us
#         eosio <= eosio::setcode               {"account":"hiddengemsss","vmtype":0,"vmversion":0,"code":"0061736d010000000180022860000060037f7f7f0...
#         eosio <= eosio::setabi                {"account":"hiddengemsss","abi":"0e656f73696f3a3a6162692f312e320008076163636f756e7400010762616c616e6...
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```
* Adding eosio.code to permissions (for inline actions)
```console
$ cleost set account permission hiddengemsss active --add-code
executed transaction: b5f18afe15fb5936e0785400db42386a2c401339e41df5d93b088ca99133e7fb  184 bytes  288 us
#         eosio <= eosio::updateauth            {"account":"hiddengemsss","permission":"active","parent":"owner","auth":{"threshold":1,"keys":[{"key...
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```

## Testing
### Testnet
#### Action - `create`
* create the token with max_supply - 10 B XGEM tokens.
```console
$ cleost push action hiddengemsss create '["hiddengemsxx", "10000000000.0000 XGEM"]' -p hiddengemsss@active
executed transaction: cfa37ac21a415b03b49144e863f855950ea55e3a6ff8b29984e4de044ba82490  120 bytes  447 us
#  hiddengemsss <= hiddengemsss::create         {"issuer":"hiddengemsxx","maximum_supply":"10000000000.0000 XGEM"}
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```

#### Action - `issue`
* issue 1 B to issuer - `hiddengemsxx`
```console
$ cleost push action hiddengemsss issue '["hiddengemsxx", "1000000000.0000 XGEM", "issue 1 B tokens"]' -p hiddengemsxx@active
executed transaction: 984530054247ed51c34cd91804116d796e208f624598acf472c121e4c78a9cea  136 bytes  276 us
#  hiddengemsss <= hiddengemsss::issue          {"to":"hiddengemsxx","quantity":"1000000000.0000 XGEM","memo":"issue 1 B tokens"}
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```

#### Action - `transfer`
* issuer - `hiddengemsxx` transfer some tokens to a user - `hgemsusr1111`
```console
$ cleost push action hiddengemsss transfer '["hiddengemsxx", "hgemsusr1111", "100.0000 XGEM", "transfer 100 XGEM tokens"]' -p hiddengemsxx@active
executed transaction: 05853b8941caba11c3067f30a6d0ef4ad0d5afe6ae43157f67c2f4840613e045  152 bytes  326 us
#  hiddengemsss <= hiddengemsss::transfer       {"from":"hiddengemsxx","to":"hgemsusr1111","quantity":"100.0000 XGEM","memo":"transfer 100 XGEM toke...
#  hiddengemsxx <= hiddengemsss::transfer       {"from":"hiddengemsxx","to":"hgemsusr1111","quantity":"100.0000 XGEM","memo":"transfer 100 XGEM toke...
#  hgemsusr1111 <= hiddengemsss::transfer       {"from":"hiddengemsxx","to":"hgemsusr1111","quantity":"100.0000 XGEM","memo":"transfer 100 XGEM toke...
warning: transaction executed locally, but may not be confirmed by the network yet         ]
```