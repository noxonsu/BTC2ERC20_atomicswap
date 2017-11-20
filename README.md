# BTC2ERC20_atomicswap
кроссблокчейновый обмен (биткоин на токен) без третьей стороны

# motivation
At present moment, we need a third party to change bitcoin to ethereum based tokens. With this example we will eliminate third party and make transactions absolutely fair, with no possibility to cheat. Connection of bitcoin payment service into ethereum based projects is a necessary step to involve such start-ups into real world. Because bitcoin exchange in fiat currencies is almost done question.

Сейчас, что бы поменять биткоин на токены в сети эфириума требуется третья сторона (обменник). В этом примере мы устраним посредника, тем самым снизим риск обмана до нуля
Подключение оплаты биткоином к проектам на основе модели токенов необходимый шаг к дальнейшей интеграции блокчейн стартапов в реальный мир, так как обмен фиатных валют на биткоин практически решенная задача

# предыдущие работы
изучен следующий опыт:
1. https://github.com/AltCoinExchange/ethatomicswap
2. https://github.com/decred/atomicswap


# ход работы
1. Решено, для теста не писать полноценные контракты, а просто проверить концепцию, для этого необходимо провести транзакцию обмена в одну сторону из биткоина (тестнет) в erc20 (ropsten). 
2. За основу взят контракт https://github.com/AltCoinExchange/ethatomicswap/blob/master/contracts/AtomicSwap.sol 
3. Работа с эфиром заменена на ERC20 
4. Контракт задеплоен в сеть https://ropsten.etherscan.io/address/0x5908afd7ccacce3d14b268312f357fad08797c96#code . Я использую remix.ethereum.org для отправки транзакций в сеть.
5. Скомпилен https://github.com/decred/atomicswap/blob/master/cmd/btcatomicswap/main.go 
6. Скачана, настроена и синхронизирована нода биткоина с сетью testnet
7. btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=*** initiate mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq 1.0

где mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq адрес продавца токенов (participant, только он сможет разморозить средства в блокчейне биткоина, перед этим заморозив токен erc20 в смарт контракте эфириуме)

Пользователь А выполняет команду:
```
D:\blockchains\bitcoin\daemon>btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=neadmin initiate mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq 1.0
Secret:      e44dfed0df2c273fb8d57c90ebe679b18a7463f0043a2566c694567999de7170
Secret hash: eef1c5498dba72d309f006426ef20a42d456d286

Contract fee: 0.00004642 BTC (0.00020723 BTC/kB)
Refund fee:   0.00005823 BTC (0.00021098 BTC/kB)

Contract (2N8MaT61ESnPLD9XpXaNEuGkzU8Uk41nw68):
63a614eef1c5498dba72d309f006426ef20a42d456d2868876a914ad4912052c56cb3e82076de07116c7fb619e36bf67049b74155ab17576a914f96c4b9523f77da899a3384de0183b57417ce6f46888ac

Contract transaction (3af0d49d0e7d966c7f9c85bce46f8e582cda0d664fce38ab6b60e3a29ff89b8c):
020000000180e17679d308214bdd30bca495a4d8fb800f2f81dfe1755f095059a0a6e1e9e6000000006b483045022100dc9249cead4831d64c434ecfdce356e9a67b969d8e8bfb9322d88553b3c28a4e0220538c9c6c4c48f9cb3b71c768e5544190507abec94abf05c6f75c6c93d61e78720121022884a21413cb8b9723fceec49319b8bb8e64aaa50070eb221753ad6e6936cdb2feffffff025e849800000000001976a914e8827d2805d08b9cf1a7b28297387a3a50eb29a488ac00e1f5050000000017a914a5bd76a48026f97774f36cf6c6d96f9626b9618b8700000000

Refund transaction (45ccf0b86334429e719a67d27fd00b0e3ce6000419aacd7cc52f85a5d5a6bc79):
02000000018c9bf89fa2e3606bab38ce4f660dda2c588e6fe4bc859c7f6c967d0e9dd4f03a01000000bf483045022100befd82f8fd22921318ec505065675e808c47f5e0e25a01656eff52c182411f560220612ad224815439a83721b7be02a28450e5c4ec6eb5abdf64eb986026b11be3b30121021e3a5dfb52c15ce039a1d775f8c135dd0d73f878079fd45a18a53d20d1f3046e004c5163a614eef1c5498dba72d309f006426ef20a42d456d2868876a914ad4912052c56cb3e82076de07116c7fb619e36bf67049b74155ab17576a914f96c4b9523f77da899a3384de0183b57417ce6f46888ac000000000141caf505000000001976a914028935296986e8ad78e9c121c963d8bdcd97391088ac9b74155a

Publish contract transaction? [y/N] y
Transaction id: 3af0d49d0e7d966c7f9c85bce46f8e582cda0d664fce38ab6b60e3a29ff89b8c
```

эта операция заморозила 1 биткоин с адреса mwKCjG4XkxgrZX2b3qkF6KJHHdHCUo17Hq (покупатель токенов он же user a). 
Транзакция создания контракта в блокченйе биткоина: https://test-insight.bitpay.com/tx/3af0d49d0e7d966c7f9c85bce46f8e582cda0d664fce38ab6b60e3a29ff89b8c
Адрес созданного контракта: https://test-insight.bitpay.com/address/2N8MaT61ESnPLD9XpXaNEuGkzU8Uk41nw68

6. хеш секрета, и ethereum адрес покупателя отправлены получателю (по внеблокчейновому каналу связи). Секрет разумеется не передавался.
7. со стороны пользователя Б в эфириуме, в контракте нужно заморозить соотвествующее количество токенов. На входе поданы следующие данные ``participate(uint value,uint _refundTime, bytes20 _hashedSecret,address _initiator)``

где value - количество отдаваемых токенов
*_refundTime - время таймаута, должно быть меньше, например +12 часов
*_hashedSecret - хеш секрета полученный от юезра А (обратите внимание на префикс 0x)
*_initiator - адрес юзера А в сети эфириум (для начисления токенов)

```"100000000","1511193253","0xeef1c5498dba72d309f006426ef20a42d456d286","0x81b7e08f65bdf5648606c89998a9cc8164397647"```

сторона B замораживет токен https://ropsten.etherscan.io/tx/0xca7d892d2e54e904d37c89eba06450a141949319aff6af1a051ea0f052cb784e

8. Оба юзера залочили свои обеспечения, теперь нужно провести операцию свопа :)  
9. Юзер А обращается к контракту в эфириуме выполняя функцию redeem(bytes32 _secret, bytes20 _hashedSecret).  

где _hashedSecret - хеш секрета переданный ранее
_secret - раскрытый секрет, зная который юзер Б сможет забрать биткоин

https://ropsten.etherscan.io/tx/0x9d512718d3a70bea34055339f66bd12ce05a678f104d536e3f33fe93f56243a7

```"0xe44dfed0df2c273fb8d57c90ebe679b18a7463f0043a2566c694567999de7170","0xeef1c5498dba72d309f006426ef20a42d456d286"```

Контракт после проверки соответсвия секрета его хешу отправляет юхеру А токены замороженные юзером Б, при этом сохраняя секрет, который видит юзер Б (вы можете увидеть этот секрет в езерскане). 

<img src='http://joxi.ru/V2VdwgKC0zV5xr.jpg'>

10. Юзер Б выпонляет транзакцию в блокчейне биткоина и забирает средства

```
D:\blockchains\bitcoin\daemon>btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=neadmin redeem 63a614eef1c5498dba72d309f006426ef20a42d456d2868876a914ad4912052c56cb3e82076de07116c7fb619e36bf67049b74155ab17576a914f96c4b9523f77da899a3384de0183b57417ce6f46888ac 020000000180e17679d308214bdd30bca495a4d8fb800f2f81dfe1755f095059a0a6e1e9e6000000006b483045022100dc9249cead4831d64c434ecfdce356e9a67b969d8e8bfb9322d88553b3c28a4e0220538c9c6c4c48f9cb3b71c768e5544190507abec94abf05c6f75c6c93d61e78720121022884a21413cb8b9723fceec49319b8bb8e64aaa50070eb221753ad6e6936cdb2feffffff025e849800000000001976a914e8827d2805d08b9cf1a7b28297387a3a50eb29a488ac00e1f5050000000017a914a5bd76a48026f97774f36cf6c6d96f9626b9618b8700000000 e44dfed0df2c273fb8d57c90ebe679b18a7463f0043a2566c694567999de7170
Redeem fee: 0.00000682 BTC (0.00002214 BTC/kB)

Redeem transaction (38ff8c204163ed814b0b89418985dba05d38733e71af128427b660cc62c1bcbd):
02000000018c9bf89fa2e3606bab38ce4f660dda2c588e6fe4bc859c7f6c967d0e9dd4f03a01000000df473044022070f1e874a87bc2851d08280ea17118af39b78257d5031da3e80605f7bc5ffdca02206eae9679a9318def9d19702ad17e63b54051f2ff9037f05a2f7ff17c4d53ad5f012102a2067d7722c74e9953b0e1399ecded6ddae8fd12c82eaa925801ff9b9cf00d0520e44dfed0df2c273fb8d57c90ebe679b18a7463f0043a2566c694567999de7170514c5163a614eef1c5498dba72d309f006426ef20a42d456d2868876a914ad4912052c56cb3e82076de07116c7fb619e36bf67049b74155ab17576a914f96c4b9523f77da899a3384de0183b57417ce6f46888acffffffff0156def505000000001976a91422d29a797b2234e7fe3d143bd5386dc4e0927d5c88ac9b74155a

Publish redeem transaction? [y/N] y
Published redeem transaction (38ff8c204163ed814b0b89418985dba05d38733e71af128427b660cc62c1bcbd)
```

https://test-insight.bitpay.com/tx/38ff8c204163ed814b0b89418985dba05d38733e71af128427b660cc62c1bcbd 

Юзер Б получил биткоин. 

<h2>Что не сделано</h2>
1. В контракт на эфириуме не добавлен обмен в обратную сторону.
2. не сделана логика разморозки токенов через таймаут.
3. В эксперименте опущен шаг checkcontract контракта в стороне А (по сумме и получателю).
