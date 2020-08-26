### NEST3.0 Automatic order taking arbitrage program operating instructions

[toc]

#### Instructions
>The NEST3.0 automatic order taking arbitrage program is an example program. Related parameters such as price deviation percentage, the default value is 2%, which is not necessarily the optimal order taking arbitrage ratio, and users can adjust it according to the actual situation.

>The exchange trading function is an optional feature. Users can choose whether to enable the exchange trading function.

>Main functions：
   * Check ETH and ERC20 balances
   * ERC20 token approve
   * Exchange price update and trading（The example exchange is Huobi）
   * Take single arbitrage
   * Take the return price asset

#### Preparation before starting

1. Get ready: wallet private key, Ethereum node URL, ERC20 token contract address, trading pair API.
   * Wallet private key: Generated by mnemonic words, and can be registered through nestDapp.
   * Ethereum node URL: You can apply for free via https://infura.io or https://quiknode.io/.
   * If you want to trade on the Huobi Exchange after taking a quote, you need to create an Access Key and a Secret Key in the Huobi Exchange API management, and fill in the withdrawal address in the withdrawal address management.
   * ERC20 token contract address: For example, if you quote USDT, you need to fill in the USDT token contract address： 0xdac17f958d2ee523a2206206994597c13d831ec7
   * Huobi trading pair settings: For example, if USDT is quoted, then the trading pair needs to fill in ethusdt. The Huobi trading pair query address is https://api.huobi.pro/v1/common/symbols


#### Startup and shutdown

1. Run the arbitrage program:
   * Double-click start.bat under the root path of the take-order arbitrage program to run the take-order arbitrage program, and a window will pop up. Please do not close it. You can view the take-order quotation and retrieve information in the window.

2. log in:
   * The browser enters http://127.0.0.1:8099/eat/eatData, it will enter the login page, the default user name is nest, and the password is nest.
   * If you need to modify the password, you can modify the nest.user.name (user name) and nest.user.password (password) in NestEatOffer/src/main/resources/application.properties.

3. Close the take-and-take arbitrage program:
   * Before closing the order-taking arbitrage program, stop order-taking and arbitrage, and then wait 10 minutes, and then close the window after the quoted assets are retrieved.

#### Related settings

1. Ethereum node (required):
   * The node address must be set first.

2. Set ERC20 address and Huobi trading pair (required):
   * Take arbitrage for ETH/USDT, fill 0xdac17f958d2ee523a2206206994597c13d831ec7 for ERC20 address, fill ethusdt for Huobi trading pair.
   * For nToken oracle arbitrage, please fill in the corresponding ERC20 address and Huobi trading pair API.

3. Deviation percentage (not required):
   * The default value is 0.02, that is, when the price deviation exceeds 2%, it will initiate an arbitrage, pay attention to fill in the decimal.

4. Set the agent address and port (not required)：
   * The agent address, which defaults to the native address of 127.0.0.1, if a network agent is used, the agent port must be configured.

5. Set private key (required):
   * Fill in the private key, and some initialization work will be performed, such as mapping the address of each contract, obtaining the ERC20 token ID, the number of bits, etc. Please be patient.

6. Set Huobi API-KEY and API-SECRET (not required):
   * If you want to trade on the Huobi Exchange after taking the order quote, please fill in API-KEY and API-SECRET. You can leave it blank if you don’t need to trade.
   * Set whether to enable user authentication. If authentication is required, you can authenticate at Huobi Exchange Identity Authentication.
   * Start trading on Huobi Exchange.

7. Open take-order arbitrage:
   * After the above configuration is completed, click the Modify Status button to change the order arbitrage status to open. You can view order information, exchange trading information (if exchange trading is enabled) in the background (the window that pops up when you click start.bat).

#### Test take arbitrage

```java
1. Notes on the code for arbitrage trading:

LOG.info("Take orders quotation：take ETH={}  take{}={}  quoteETH={}  quote{}={}  payEthAmount={}",
                tranEthAmout, SYMBOL, tranErc20Amount, offerEthAmount, SYMBOL, offerErc20Amount, payEthAmount);
// Initiate an arbitrage transaction
String transactionHash = ethClient.sendEatOffer(typeList, payEthAmount, method, gasPrice);

2. Check the number of quoted ETH and quoted ERC20 in the printed log, and check the data.
```

#### Huobi Exchange Opens API-KEY

1. Log in to the exchange, select API management

   ![](./picture/API-KEY-1.png)

2. Create API-KEY and open permissions.

   * Accept open "read". For example: other people quote 10ETH: 3000USDT, Huobi exchange real-time price: 10ETH: 2600USDT, then the order will be put into 10ETH, get 3000USDT, then will read the exchange's USDT corresponding deposit address, and deposit 3000USDT Exchange.
   * Open "deal". For example: the 3000USDT recharged will be sold at market price, and 3000USDT will be sold to get ETH.
   * Open "Withdrawal". For example: Sell 3000USDT to get 11.5ETH, and 11.5ETH will be withdrawn to the current take-order quote wallet address.

   ![](./picture/API-KEY-2.png)

3. After filling in, click "Create", then it will display: api-key and api-secret. Fill it in the configuration page and click "Submit".


#### Contract interaction

[Contract interaction description](./NestEatOffer/README.md)

