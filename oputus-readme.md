# oputus-trading-robot

Current version: 2025Jul10

## Introduction

An EA based on Sistema Manche from investor Douglas Niemeyer. Only used for long positions and tested in indexes such as Bra50, SPY and NASDAQ

Settting all parameters to their defaults (right-click - Defaults) will make it act like the regular Manche EA where the lot sizes are static and no other fancy features are enabled

## Basics

- The same EA can operate on long and short positions simultaneously. Each has its own settings.

- Parameter `EA Name` allows you to specify a name for this Expert Advisor/Robot. It will be used when placing orders in the comments field and also when sending push notifications. Useful if you have more than one EA running on the same account but not required.

- Parameter `Magic Number` sets the Magic Number for orders. This EA will only "touch"/manage orders with matching EA Number. Orders placed manually will be ignored by Oputus. If you are running multiple EAs in the same account/same MT they SHOULD have different Magic Numbers to ensure they are handled accordingly.

- Parameter `Timeframe` sets the timeframe for the EA to operate. Note: if `Auto Timeframe` is enabled it will get the timeframes to operate from the `Auto Timeframe` settings and this option will only set the chart to be displayed in the timeframe of your choosing. 

- Parameter `Percentage of the balance to use for lot size calculations` It allows the use of a given percentage of the account balance(less or more) when calculating lot size(mainly when using investing profiles and/or reinvesting). For example, in an account with a $10k balance, if the new setting is set to 90, when using investing profiles, lot sizes will be calculated based on $9k(90% of $10k) instead of the actual account balance. Note: the actual balance used for lot size calculations is rounded down to the closest $100. So considering an account with balance of $9999, and the new setting set to 90, the actual balance to be used in the calculation will be as follows: 9999 * 90% = 8999.1 rounding it down it will result in 8900. 8900 will then be used when calculating lot sizes. This is done to avoid fractions of lot sizes.

## Nice Features

### Investing Profiles

Investing profiles can be used to set lot sizes, take profit, gradient buy and Auto Timeframe settings.

Lot sizes will be calculated according to the profile select, on the account balance and the value specified in the `Percentage of the balance to use for lot size calculations` parameter.

Once a Investing Profile is selected in the `Investing Profile. Calculates lots and take profits for you` parameter, you must select which feature(s) should get their settings from the profile.
Whenever you see a parameter called `Use below settings from the investment profile:` you can set it to **True** to get the setting from the Investing Profile instead of what you see in the inputs.

Note: These profiles have been backtested extensively and it's recommended to set all the  `Use below settings from the investment profile:`  parameters in the EA to **True** for better results.

On the current version the profiles have the following settings:

|Low Risk|  |
|--------|--|
| Base balance: | $10k |
|||
| Basics - Take Profit(in points) used when placing new order| 5 |
| Basics - Take Profit(in points) when average price is calculated: | 60 |
| Basics - Buy orders size(in contract): | 0.6 |
|||
|Gradient Buy - Level One Trigger: | 8 |
| Gradient Buy - Level One Lot Size: | 0.8 |
| Gradient Buy - Level Two Trigger: | 10 |
| Gradient Buy - Level Two Lot Size: | 1.2 |
|||
| Auto Timeframe - Auto Timeframe enabled: | False |
| Auto Timeframe - Timeframe to start with: | NA |
| Auto Timeframe - Open Positions Threshold: | NA |
| Auto Timeframe - New timeframe: | NA |

|Medium Risk|  |
|--------|--|
| Base balance: | $10k |
|||
| Basics - Take Profit(in points) used when placing new order| 5 |
| Basics - Take Profit(in points) when average price is calculated: | 100 |
| Basics - Buy orders size(in contract): | 1 |
|||
|Gradient Buy - Level One Trigger: | 7 |
| Gradient Buy - Level One Lot Size: | 1.4 |
| Gradient Buy - Level Two Trigger: | 10 |
| Gradient Buy - Level Two Lot Size: | 1.6 |
|||
| Auto Timeframe - Auto Timeframe enabled: | True |
| Auto Timeframe - Timeframe to start with: | M3 |
| Auto Timeframe - Open Positions Threshold: | 1 |
| Auto Timeframe - New timeframe: | M5 |

|High Risk|  |
|--------|--|
| Base balance: | $10k |
|||
| Basics - Take Profit(in points) used when placing new order| 5 |
| Basics - Take Profit(in points) when average price is calculated: | 120 |
| Basics - Buy orders size(in contract): | 1.5 |
|||
| Dynamic Take Profit - Level One Open buys threshold: | 10 |
| Dynamic Take Profit - Level One Take Profit: | 200 |
| Dynamic Take Profit - Level Two Open buys threshold: | 2100 |
| Dynamic Take Profit - Level Two Take Profit: | 200 |
|||
| Auto Timeframe - Auto Timeframe enabled: | True |
| Auto Timeframe - Timeframe to start with: | M3 |
| Auto Timeframe - Open Positions Threshold: | 1 |
| Auto Timeframe - New timeframe: | M5 |


### Dynamic Take Profit

Allows to set different Take Profit levels depending on the current conditions. This only applies when the average price is applied.

When enabled it will check current price to determine whether it should use `Level One Take Profit` or `Level Two Take Profit`. If the current price is below the previous day's low, the take profit when using average price will be `Level One Take Profit` otherwise `Level Two Take Profit`. The moment the current price falls below the previous day's low it will apply `Level One Take Profit` to all open orders.

This feature is opportunistic and will only use a higher TP (`Level Two Take Profit`) if the price is still climbing compared to the previous day's low. All backtests concluded it does not increase the Equity Drawdown Maximum(DD) further than previous seen levels. Neither it increases the occurrences of DD's greater than 15%. It should be fairly safe to use and should result in bigger profits.

A moving average can also be used to only allow `Level Two Take Profit` to be applied when the current price is above the average. Default to 9 days for the MA period.

Note: This feature is NOT currently part of any investing profiles.

### Gradient Buys

Allows the EA to change the lot size based on the number of current open positions. There are two levels to be configured. You could say something like after 10 open positions the lot size will be 2. Until then the `Buy orders size(in contract)` in the Basic setting will be used.

If you do not wish to use this feature set `Level One Trigger` to zero.

There is also a second level which can be used as follows: Continuing with the example above, you could set `Level Two Trigger` to 15 and `Level Two Lot Size` to 3. Therefore after buying 15 times it will start placing new orders using lot size 3. 

Note: You can also configure the setting to be retrieved from the Investing Profile.

### Auto Timeframe

Allows to adjust the timeframe of the chart based on the number of open positions. You select the initial timeframe in the `Timeframe to start with` and set the number of positions before in changes to the new timeframe.

Upon closing of all opened positions the timeframe will change to the initial time frame set in the parameters.

### Reinvesting

Calculates lot size based on the current balance and Investment profile. An investment profile must be selected to use this option. Can be done either daily, Monthly or ASAP. It also calculates the lot sizes used in Gradient Buy, in case the feature is on. 

Note: It will delay calculating lot sizes if there are positions opened until they are closed. That's done to avoid reducing the lot size in case of withdrawals from the account. Example: Suppose you start the day with a $10k balance in the account. Reinvesting is set to Daily. At the end of the trading session on day one there are a few opened positions. You then decide to withdraw $5k from the account. Let's imagine the new balance is now $5k. On the following day, when the session starts if it recalculates the lot sizes based on the balance, the lot size would halve for new buys resulting in less profit or even loss when the positions close. In order to avoid this scenario the EA will wait until the positions are all closed and then recalculate the lot sizes. The same applies for deposits. If you deposit money in the account and there are positions opened, it won't automatically recalculate the lot sizes. 



If you'd like to force the EA to recalculate the lots, in MT5 go to Tools - Global Variables and delete the following for the list:

```
Oputus-MagicNumber-XXXX-EffectiveLongLot
Oputus-MagicNumber-XXXX-GBEffectiveLevelOneLot
Oputus-MagicNumber-XXXX-GBEffectiveLevelTwoLot
```
Where XXXX is the magic number of your EA.

Now reload the EA.

![GV reinvesting](https://i.imgur.com/pX1Nrmz.png)

Notification note: This feature sends push notifications. See *Notifications* below.

### Limits

Used to limit the number of open positions at any given time. Value of zero effectively disables this feature. You can limit based on the number of lots, number of open positions or both. Whatever happens first will block further buys.

Note: I wouldn't personally recommend the use of this feature as the Manche strategy seems to benefit from being allowed to continue to buy during market downfalls.


### Symbol Rotation

EA will switch from the current symbol, assumed to be something like Bra50XXXYY to a new symbol X days before the current symbol expires. It will try to rotate to the new symbol every new day if there are no positions opened. Otherwise it will be postponed until all the positions are closed.

The chart will be reloaded to use the new symbol once the rotation is completed. 

Notification note: This feature sends push notifications. See *Notifications* below.

### Remote Control

Allows to use invalid orders(order wich will be rejected by the server) to control some aspects of the EA. In order to use this feature a Buy at the Market Price order must be placed, of any size, regardless of the Symbol. The Take Profit specified when placing the order will determine which command will be executed.

The commands available might change depending on the version so it's always a good idea to call the help menu to see which commands are available:

|Take Profit Value|Command|
|--------|--|
|100|Help menu|

A confirmation of the execution and the outcome will be sent to the user via push notification

You should use the actual values in the table as take profit. Since they are well below the current price they will be rejected by the trading server and won't be executed. 

Note that these commands can also be called when the market is closed.

### Notifications

These are push notification that requires the **Meta Trader 5** app on your phone. In order to set it up, in the MT5 in your VM select Tools - Options - Notifications tab. Check **Enable Push notifications**, enter the **MetaQuotes ID** from the App in the appropriate field and hit **Test**.

Note: I highly recommend you enable this feature as it will give you good insight into the EA.

#### Beginning/End of Day

You should receive a notification 30 minutes before the start of the sessions confirming the EA is running, your balance(which confirms communication with the broker) and the amount of open positions. Receiving this notification is also a sign that the EA will be able to trade in the upcoming session as it runs the following checks:

- Confirms the terminal is connected to the trade server
- Confirms Algo Trading is enabled in MT5
- Confirms Algo Trading is enabled for the EA
- Confirms there is no block on the account(server side)
- Confirms trading is allowed. IE: Not using investor password
- Confirms account is set to Hedge mode

Failure on any of the checks above will be surfaced on the notification.

You will also be notified at the end of the trading session with a summary of the day, including: MAE, profit, number of buys, positions that are left open.

#### Packet loss level

You will also be notified of high levels of packet loss/retransmission as reported by MT5. Note it doesn't necessarily mean an issue in the connection with the broker as this metrics is retrieved from the OS and includes any connections on the system.

#### Symbol about to expire

Even with Symbol Rotation off, daily, starting 5 days before the symbol expires you will be notified the current symbol is about to expire so you can take action.

#### Symbol Rotation completed

A notification will be sent once the symbol has switched to the new symbol as per Symbol Rotation settings.


Note: Checking **Notifications from the local terminal/trade server** will send a message every time an order is placed, modified, closed. I found this "spammy".

#### New Symbol Detected

You will receieve a notification once the next symbol becomes available for trading. This is always enabled and can't be disabled.

#### New MT Version availabe

This feature, which if off by default, will send a push notification when the Metatrader live update tool detects and downloads a new version. The notification will be send only once, when a new version is detected.

Note: this feature requires a one-time setup to work. You basically have to open a Command Prompt(as Administrator) and run a single command that will create a symlink(i.e. mklink /d ...). The exact command depends on your installation path so load the EA and check the **Experts** tab for the exact command. Run it and reload the EA. If everything is right now in the Experts tab you should see **MT Version detection: Setup complete!**

### Advanced Trading

**Stop buying after closing all positions** Offer a simple way to prevent further orders once the current ones are closed. It's useful if you'd like to pause placing orders as soon as the current ones are closed. Has been used in the past to allow withdrawals from the account and also to rotate to a different symbol.

Note: If you turn it on when there are no open orders, it will allow orders to be placed but will block further orders once they are closed. 


For the options below you usually don't want to change these settings. They exist for very specific use cases. Consult with Oputus Support before changing these settings. 

**Manage EA positions Only** Defaults to **True** which allows Oputus to only touch/manage its own orders. Manually created order via MT or others means won't be considered by this EA. Orders created by other EA with different Magic Numbers will also be ignored when this options is **True**. Setting it to **False** will allow the EA to manage all the buy orders, irrespective of the Magic Number. This will include orders placed manually by the user and orders from any other EAs running in the same account.

**Forces the EA to apply Avg Price as TP** defaults to **False**. By default, the EA will only apply average price + TP in points after the first partial exit(one order has hit its take profit). Setting this option to **True** will force the EA to apply Average Price to all orders. So if the order has each their own TP, when this is set to **False** new TP will be applied to all open orders.



### Other Stuff

#### Authorization code

The EA requires an authorization code to be loaded on a chart. One code is required per metatrader installation.

Authorization code is not needed when performing backtesting.

#### Info Panel type

Selects how the info panel in the chart will be plotted. It can be turned off, in text mode or graphical mode. This setting controls the info panel used during backtests as well. Note: Graphical mode will slow down progress of ticks in visual mode. If you'd like to speed up the ticks in visual mode either choose Text or turn it off completely if not needed.

#### Comments in the chart

Specifies the maximum number of comment lines to keep on the chart. The most recent X lines will be kept.



## Remarks
- A hedging account is assumed for this EA to work appropriately 
- If you change the average take profit it will be modified on all existing orders in a matter of seconds after the change. There is no need for a new order to be placed for the TP to be recalculated and modified.

## How to install/upgrade it

1. In Metatrader go to File - Open Data Folder
2. Navigate to `MQL5\Experts`
3. Paste the `.ex5` file
4. Double click the file you just pasted to open it with Metatrader
5. Drag and Drop it on a new chart window or same window where the current one is loaded
6. Confirm you want to replace the old one with the new one
7. Make sure `Allow Algo Trading` is checked
8. Under **Inputs** load your .set file

## FAQ

### I've been asked to collect logs. How do I do this?

In Metatrader go to **File** - **Open Data Folder(Ctrl+Shift+D)**.
1. Right click on the **Logs** folder, **Send To**, **Compressed(zipped) folder**
2. Navigate to **MQL5**, right click on the **Logs** folder, **Send To**, **Compressed(zipped) folder**
3. Send those two zip files to me(Lutieri)
