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
5. 