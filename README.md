# BTC to ERC20 atomicswap
Crossblockchain exchange BTC to ERC20 token withour third party

# motivation
At present moment, we need a third party to change bitcoin to ethereum based tokens. With this example we will eliminate third party and make transactions absolutely fair, with no possibility to cheat. Connection of bitcoin payment service into ethereum based projects is a necessary step to involve such start-ups into real world. Because bitcoin exchange in fiat currencies is almost done question.

# Previous work
we studied these articles
1. https://github.com/AltCoinExchange/ethatomicswap
2. https://github.com/decred/atomicswap


# Working process
1. To test the concept, you simply need to perform a one-way exchange procedure from bitcoin (testernet) in erc20 (ropsten).
2. I used the contract https://github.com/AltCoinExchange/ethatomicswap/blob/master/contracts/AtomicSwap.sol but made it easier.
3. Change Ether "transfers" to ERC20
4. Contract was deployed to the ropsten network https://ropsten.etherscan.io/address/0x594ef832f837c2a7de85cf222f544ad723f85de4#code ( using remix.ethereum.org)
5. Compiled decred tool https://github.com/decred/atomicswap/blob/master/cmd/btcatomicswap/main.go 
6. Download and synronised bitcoin core (testnet)
7. btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=*** initiate mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq 1.0

где mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq адрес продавца токенов (participant, только он сможет разморозить средства в блокчейне биткоина, перед этим заморозив токен erc20 в смарт контракте эфириуме)

Пользователь А выполняет команду:

```
D:\blockchains\bitcoin\daemon>btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=neadmin initiate mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq 0.1
Secret:      d06aef2ea98a6a4907fb29679bc646f0e699ffff2fc78dbe683c94720b805015
Secret hash: 44682a741560103c3ba162da3f6932051ce7b0c8

Contract fee: 0.00000806 BTC (0.00002167 BTC/kB)
Refund fee:   0.00000609 BTC (0.00002215 BTC/kB)

Contract (2N7ij1vUH6oRrU3fTR7bjh2ViUx5yJmPg2v):
63a61444682a741560103c3ba162da3f6932051ce7b0c88876a914ad4912052c56cb3e82076de07116c7fb619e36bf67042f98155ab17576a914c7b2a2fd4c5954e493a1e71d98ec3566ab25b0426888ac

Contract transaction (aa1c13e5ffe937c124325693e3fae4a2ed84ee67cf07a5ee2d9f95c37fc65def):
02000000025e01c95c87c371f4fa4ec6d2119c26602b3d2fc425505d6fe850f8f0c8050222010000006b483045022100a943a0a38d3883fe4c24b4a3e7c442d5cd227b552e8543afe8e4b837f0ed091602203cc00e536b0f53e6ba5537ee80818dc9b69607df06493b7c577e14980c77367e0121034e4ce7f926027704b5f3e1519d4230f4936cbeefba94761008161e2d03e434a2feffffff72b46dbb64ec31018ebac48434e1aad6f2a4ee7986300ff060f43b57b2dcaea6010000006b483045022100f99bca2624fd9aad86f41636634116d271337288d36fe6316c11a0d3236ef6ab02203e5ea2fb6ecacc08eb457e4472b7e9218533454ee0e67f127fb13980295eded701210355642e96b8bf20cd6ea05f8bbad48cc525baa2813f3a27d7e33cecd19123ea22feffffff02809698000000000017a9149ec53937cf36ec1a95a182356ccbc96309bc182d8714a97100000000001976a914d0564bbd4c1ec9aaea924d7d0d5a3ddb3c8028c288ac00000000

Refund transaction (a485de3e50f0d01afe9e256e31ae977bf7b2afc97f62d12c6cd4738b6c1c6b5e):
0200000001ef5dc67fc3959f2deea507cf67ee84eda2e4fae393563224c137e9ffe5131caa00000000be4730440220041eb1763b85941c88bd0fd0a9d2348561489df28c05bfdf9525060fda31020302206e2a18e2adbabf200dddf32a568c7b81d61b0a58385cdc67f759f328424e08e90121033c512da0689ec49cf47e3049e87628dd9c6a8bb601d294cf1bee7a168d57fc3c004c5163a61444682a741560103c3ba162da3f6932051ce7b0c88876a914ad4912052c56cb3e82076de07116c7fb619e36bf67042f98155ab17576a914c7b2a2fd4c5954e493a1e71d98ec3566ab25b0426888ac00000000011f949800000000001976a91491c9193c3584b6d19e16e5767376bdfab4ca028788ac2f98155a

Publish contract transaction? [y/N] y
```

эта операция заморозила 0.1 биткоин с адреса mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq (покупатель токенов он же user a). 
Транзакция создания контракта в блокченйе биткоина: https://test-insight.bitpay.com/tx/aa1c13e5ffe937c124325693e3fae4a2ed84ee67cf07a5ee2d9f95c37fc65def
Deployed contract: https://test-insight.bitpay.com/address/2N7ij1vUH6oRrU3fTR7bjh2ViUx5yJmPg2v

6. secret hash and ethereum address of User B sent to the seller (via offchain). Сertainly without a secret.
7. со стороны пользователя Б в эфириуме, в контракте нужно заморозить соотвествующее количество токенов (предварительно выполнив erc20 approve). На входе поданы следующие данные ``participate(uint value,uint _refundTime, bytes20 _hashedSecret,address _initiator)``

where value - amount of tokens
*_refundTime - время таймаута, должно быть меньше, например +12 часов
*_hashedSecret - хеш секрета полученный от юезра А (обратите внимание на префикс 0x)
*_initiator - адрес юзера А в сети эфириум (для начисления токенов)

```"10000000","8640000","0x44682a741560103c3ba162da3f6932051ce7b0c8","0x104D356415708B3Dd7C898f252471Aa34106002d"```

user froze tokens: https://ropsten.etherscan.io/tx/0xc6f3dc9aa1feecb58cac6c18e5872a7d9076855ad472f6f5b85505dece3d61da

8. Оба юзера залочили свои обеспечения, теперь нужно провести операцию свопа :)  
9. Юзер А обращается к контракту в эфириуме выполняя функцию redeem(bytes32 _secret, bytes20 _hashedSecret).  

где _hashedSecret - хеш секрета переданный ранее
_secret - раскрытый секрет, зная который юзер Б сможет забрать биткоин

```"0xd06aef2ea98a6a4907fb29679bc646f0e699ffff2fc78dbe683c94720b805015","0x44682a741560103c3ba162da3f6932051ce7b0c8"```

Here this transaction: https://ropsten.etherscan.io/tx/0x78a287f35ba475848e37591d2f8091edf2dbaea637ca097781c45910b489a70f

<img src='http://joxi.ru/Q2KLwB7C9odojA.jpg'>

10. Юзер Б выпонляет транзакцию в блокчейне биткоина и забирает средства

```
D:\blockchains\bitcoin\daemon>btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=neadmin redeem 63a61444682a741560103c3ba162da3f6932051ce7b0c88876a914ad4912052c56cb3e82076de07116c7fb619e36bf67042f98155ab17576a914c7b2a2fd4c5954e493a1e71d98ec3566ab25b0426888ac 02000000025e01c95c87c371f4fa4ec6d2119c26602b3d2fc425505d6fe850f8f0c8050222010000006b483045022100a943a0a38d3883fe4c24b4a3e7c442d5cd227b552e8543afe8e4b837f0ed091602203cc00e536b0f53e6ba5537ee80818dc9b69607df06493b7c577e14980c77367e0121034e4ce7f926027704b5f3e1519d4230f4936cbeefba94761008161e2d03e434a2feffffff72b46dbb64ec31018ebac48434e1aad6f2a4ee7986300ff060f43b57b2dcaea6010000006b483045022100f99bca2624fd9aad86f41636634116d271337288d36fe6316c11a0d3236ef6ab02203e5ea2fb6ecacc08eb457e4472b7e9218533454ee0e67f127fb13980295eded701210355642e96b8bf20cd6ea05f8bbad48cc525baa2813f3a27d7e33cecd19123ea22feffffff02809698000000000017a9149ec53937cf36ec1a95a182356ccbc96309bc182d8714a97100000000001976a914d0564bbd4c1ec9aaea924d7d0d5a3ddb3c8028c288ac00000000 d06aef2ea98a6a4907fb29679bc646f0e699ffff2fc78dbe683c94720b805015

Publish redeem transaction? [y/N] y
Published redeem transaction (329df2c94eb2defd9b7e203050da37b87f7d38909e26a65fd2d7074efcc813a2)
```

https://test-insight.bitpay.com/tx/329df2c94eb2defd9b7e203050da37b87f7d38909e26a65fd2d7074efcc813a2 

Юзер Б получил биткоин. 

<h2>Что не сделано</h2>
1. В контракт на эфириуме не добавлен обмен в обратную сторону. <br>
2. Не сделана логика разморозки токенов через таймаут. <br>
3. В эксперименте опущен шаг checkcontract контракта в стороне А (по сумме и получателю).<br>
 
<h2>Что дальше?</h2>
Технология atomicswap хороша для сайдчейновых операций, тоесть при переводе из одного блокчейна в другой 1:1 (например как это делает polkadot bride, только без валидаторов), однако для полноценной интеграции необходимо решить проблему откуда брать курсы обмена.
