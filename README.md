# BTC2ERC20_atomicswap
кроссблокчейновый обмен (биткоин на токен) без третьей стороны

# мотивация
Сейчас, что бы поменять биткоин на токены в сети эфириума требуется третья сторона (обменник). В этом примере мы устраним посредника, тем самым снизим риск обмана до нуля
Подключение оплаты биткоином к проектам на основе модели токенов необходимый шаг к дальнейшей интеграции блокчейн стартапов в реальный мир, так как обмен фиатных валют на биткоин практически решенная задача

# предыдущие работы
изучен следующий опыт:
1. https://github.com/AltCoinExchange/ethatomicswap
2. https://github.com/decred/atomicswap


# ход работы
1. Решено, для теста не писать полноценные контракты, а просто проверить концепцию, для этого необходимо провести транзакцию обмена в одну сторону. 
2. За основу взят контрак https://github.com/AltCoinExchange/ethatomicswap/blob/master/contracts/AtomicSwap.sol из него удалено практически все, что не касается напрямую передачи. 
3. Работа с эфиром заменена на ERC20 
4. Контракт задеплоен в сеть https://ropsten.etherscan.io/address/0x0440c9ac98e9dce9cf1d37236481464ffc7c68a0#code
5. Скомпилен https://github.com/decred/atomicswap/blob/master/cmd/btcatomicswap/main.go 
6. Скачана, настроена и синхронизирована нода биткоина с сетью testnet
7. btcatomicswap.exe --testnet --rpcuser=admin --rpcpass=*** initiate mqxSYdDnWwfTKssxHeMrUFEu2WNdkirSjo 1.0

``сгенеренная транза``

6. необходимые данные (согласно статье https://github.com/decred/atomicswap) отправлены получателю (по обычному каналу связи). Секрет разумеется не передавался.
7. со стороны пользователя в эфириуме, в контракт на эфире залочено соотвествующее количество токенов. На входе поданы следующие данные ``participate(uint value,uint _refundTime, bytes20 _hashedSecret,address _initiator)``

где value - количество отдаваемых токенов
_refundTime - время таймаута
_hashedSecret - хеш секрета полученный от юезра А
_initiator - адрес юзера А в сети эфириум (для начисления токенов)

8. Оба юзера залочили свои обеспечения, теперь нужно провести операцию свопа. 
9. Юзер А обращается к контракту в эфириуме выполняя функцию redeem(bytes32 _secret, bytes20 _hashedSecret), где раскрывает секрет. 
10. Контракт после проверки соответсвия секрета его хешу отправляет юхеру А средства залоченые юзером Б ранее. 
11. одновременно с этим контракт выдает в эфир событие с раскрытым секретом, юзер Б ловит его.
12. выполняет соотвествущую команду в биткоине и забирает деньги
