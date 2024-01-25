# Руководство по тестированию смарт контрактов для поставщиков капитала в сетях Ethereum Sepolia и Arbitrum Sepolia


## Вступление
Для тестирования смарт контрактов для поставщиков капитала в сетях Ethereum Sepolia и Arbitrum Sepolia, нужно пройти несколько основных шагов:
1) Получение SepoliaETH
2) Получение stETH в сети Ethereum Sepolia
3) Депозит stETH в контракт распределения наград в сeти Ethereum Sepolia
4) Получение наград в токене MOR в сети Arbitrum Sepolia с использованием Layer Zero bridge


## Адреса смарт контрактов
Ethereum Sepolia 
- Контракт распределения наград: [0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b](https://sepolia.etherscan.io/address/0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b#code)
- Контракт токена stETH: [0x84BE06be19F956dEe06d4870CdDa76AF2e0385f5](https://sepolia.etherscan.io/address/0x84BE06be19F956dEe06d4870CdDa76AF2e0385f5#code)
  
Arbitrum Sepolia 
- Контракт токена MOR: [0xe6d01d086a844a61641c75f1bca572e7aa70e154](https://sepolia.arbiscan.io/address/0xe6d01d086a844a61641c75f1bca572e7aa70e154#code)


## Как получить Sepolia ETH?
- Для начала нам необходим установленный Metamask или другой кошелек web3. Нужно перейти к настройкам сети, которые по умолчанию обычно установлены на Ethereum, затем выбрать в качестве сети тестовую сеть Sepolia.
- Следующий шаг это депозит SepoliaETH, для этого вы можете использовать веб-сайты краны, такие как https://sepolia-faucet.pk910.de/# или https://sepoliafaucet.com/.
- Существует множество других кранов, которые можно легко найти, но обычно им требуется чтобы у вас был активный адрес в основной сети Ethereum.


## Как получить stETH?
Поскольку тестовый токен stETH официально не развернут в сети Sepolia, мы будем использовать копию этого токена с необходимыми для тестирования основными функциями.

Для этого необходимо перейти к контракту [stETH](https://sepolia.etherscan.io/address/0x84BE06be19F956dEe06d4870CdDa76AF2e0385f5#writeContract) и открыть вкладку “Contract”, затем нажать на “Write Contract”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

![stETHContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/stETH-580x648.png)

Далее необходимо выбрать функцию `mint()`, с помощью которой мы сможем отчеканить необходимое количество stETH.   
В качестве параметров необходимо указать:
- `account_`: адрес на который будут зачислены токены;
- `amount_`: количество токенов в WEI, вместо ETH. Для конвертации, вы можете использовать данный калькулятор https://eth-converter.com. Например, если вам необходиы 0,01 stETH, то эта сумма будет равняться 10000000000000000 WEI.  
На примере выше, было отчеканено 100 stETH (отображение в WEI) на адрес 0xa4DB...2259.

После выполнения всех действий нажмите на кнопку “Write” и подтвердите транзакцию в кошельке.


## Как проверить баланс stETH?
Необходимо перейти к контракту [stETH](https://sepolia.etherscan.io/address/0x84BE06be19F956dEe06d4870CdDa76AF2e0385f5#readContract), перейти во вкладку “Contract”, затем “Read Contract”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

![stETHContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/check-stETH.png)

Перейдите к функции `balanceOf()` и в качестве параметра, в поле `account_`, укажите ваш адрес и нажмите на кнопку "Query". В результате мы получим значение в WEI показывающее количество stETH на вашем адресе. На примере выше, у адреса 0xa4DB...2259 есть 1000 stETH отображенные в WEI.

Еще один способ проверки баланса, это добавление токена в ваш web3 кошелек. Узнать как это сделать в кошельке Metamask, вы можете из этого [руководства](https://support.metamask.io/hc/en-us/articles/360015489031-How-to-display-tokens-in-MetaMask#h_01FWH492CHY60HWPC28RW0872H). В качестве адреса токена stETH введите `0x84BE06be19F956dEe06d4870CdDa76AF2e0385f5`.


## Как внести stETH в контракт распределения наград?
Перейдите к контракту [stETH](https://sepolia.etherscan.io/address/0x84BE06be19F956dEe06d4870CdDa76AF2e0385f5#writeContract), откройте сначала вкладку “Contract”, затем вкладку “Write Contract”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

![stETHContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/stethapproval.png)

Перед внесением средств, нам необходимо предоставить разрешение контракту на отправку stETH с вашего кошелька. Для этого мы будем использовать функцию `approve()`.  
В качестве параметров укажите:
- `spender`: адрес контракта распределения наград - `0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b`;
- `amount`: сумму токенов на которые даете разрешение отображенную в WEI. Сумма должна равняться или быть больше той, которую вы планируете внести в контракт. Для удобства вычислений, вы можете воспользоваться калькулятором https://eth-converter.com.

После выполнения всех действий нажмите на кнопку “Write” и подтвердите транзакцию в кошельке.

Далее необходимо перейти к контракту [распределения наград](https://sepolia.etherscan.io/address/0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b#writeProxyContract), и открыть вкладку “Contract”, а затем “Write as Proxy”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

![DistributionContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/stake.png)

Необходимая для внесения stETH в контракт функция, называется `stake()`.  
В качестве параметров укажите:
- `poolId_`: идентификатор группы (пула); для тестирования создан пул с номером “0”;
- `amount_`: сумма токенов в WEI. (10 stETH в данном примере).

После выполнения всех действий нажмите на кнопку “Write” и подтвердите транзакцию в кошельке.


## Как получить информацию о сумме депозита и сумме наград?
Данная информация доступна через взаимодействие с контрактом [распределения наград](https://sepolia.etherscan.io/address/0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b#readProxyContract). Откройте вкладку “Contract”, а затем “Read as Proxy”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

Для нашей цели, мы будем использовать две функции. Первая показывает количество токенов MOR полученных в качестве награды за депозит stETH в контракт. Награды начисляются каждую секунду.

![DistributionContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/rewards.png)

Выберите функцию `getCurrentUserReward` и в качестве параметров укажите: 
- `poolId_` номер пула, в нашем случае это "0";
- `user_` адрес вашего кошелька.  
Для отображения информации, нажмите на кнопку "Query".

Как результат, вы увидите значение показывающее количество начисленных токенов в качестве награды. Отображение токенов в WEI.

Вторая функция `usersData()` покажет сумму вашего депозита. Параметры аналогичны первой функции.

![DistributionContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/stakedamount.png)


## Как снять stETH с контракта?
Необходимо перейти к контракту [распределения наград](https://sepolia.etherscan.io/address/0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b#writeProxyContract), вкладку “Contract”, а затем вкладку “Write as Proxy”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

![DistributionContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/withdraw.png)

Выберите функцию `withdraw()` и укажите в качестве параметров:
- `poolId_`: номер пула, в нашем случае это "0";
- `amount_`: сумму токенов указанную в WEI.

После выполнения всех действий нажмите на кнопку “Write” и подтвердите транзакцию в кошельке.


## Как получить награды?
Для этого нам снова необходим контракт [распределения наград](https://sepolia.etherscan.io/address/0x0Ad2fa5D8F420ff6D87192b32d89faf70466b30b#writeProxyContract). Нужная функция находится во вкладке “Contract” и “Write as Proxy”. Не забудьте подключить свой кошелек, в котором должен быть достаточный баланс нативного токена для оплаты комиссии.

![DistributionContract](https://github.com/antonbosss/fantastic-bassoon/blob/SepoliaTestnetGuide/claim.png)

Чеканка и получение токенов MOR осуществляется в сети Arbitrum Sepolia, поэтому нам необходимо воспользоваться мостом Layer Zero. 

Всё что необходимо сделать, это использовать функцию `claim()` и указать следующие параметры:
- `claim`: тут указывается сумма нативных токенов в сети-отправителе, которые будут использованы в качестве комиссии за чеканку токенов в сети Arbitrum Sepolia. Вы можете указать бОльшую сумму, разница будет вам возвращена;
- `poolId_`: номер пула, в нашем случае это "0";
- `user_`: адрес в сети Arbitrum Sepolia на который будут зачислены токены.
  
После выполнения всех действий нажмите на кнопку “Write” и подтвердите транзакцию в кошельке.

Поздравление, ваша награда в виде токенов MOR теперь в сети Arbitrum Sepolia. Вы можете импортировать адрес токена `0xe6D01D086a844a61641C75f1BCA572e7aa70e154` в ваш кошелек Мetamask используя это [руководство](https://support.metamask.io/hc/en-us/articles/360015489031-How-to-display-tokens-in-MetaMask#h_01FWH492CHY60HWPC28RW0872H) или проверить баланс через смарт контракт по примеру с токеном stETH.


## Как получить ETH в сети Arbitrum Sepolia?
- Для начала нам необходим установленный Metamask или другой кошелек web3. Затем необходимо добавить сеть Arbitrum Sepolia вручную или использовать сайт [Chainlist](https://chainlist.org/?testnets=true&search=arbitrum+sepolia) где нажать на кнопку “Add to Metamask”.
- Следующий шаг это депозит Arbitrum Sepolia ETH, для этого вы можете использовать веб-сайты краны, такие как ttps://faucet.quicknode.com/arbitrum/sepolia или https://faucet.triangleplatform.com/arbitrum/sepolia

**Если вам интересно более детальное руководство, с описанием функций контрактов, вы можете его найти по [ссылке](https://docs.google.com/document/d/1DbNx-CBpHjFUvIhbKHJSOshCKNTWlng7SeLzzn95AKY/edit)** 
