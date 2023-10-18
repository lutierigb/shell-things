# oputus-trading-robot

## Basics

An EA based on Sistema Manche from investor Douglas Niemeyer. Only used for long positions and tested in indexes such as Bra50, SPY and NASDAQ

## Features

### Limits

Used to limit the number of open positions at any given time. Value of zero effectively disable this feature. You can limit based on the number of lots, number of open positions or both. Whatever happens first will block further buys.

Note: I wouldn't recommend the use of this feature as the strategy seems to benefit from being allowed to continue to buy during market downfalls.

### Reinvesting

Calculates lot size based on the current balance and Investment profile. An investment profile must be selected to use this option. Can be done either daily or Monthly. It also calculates the lot sizes used in Gradient Buy, in case the feature is on. 

Note: It will delay calculating lot sizes if there are positions opened until they are closed. That's to avoid reducing the lot size in case of withdrawals from the account. Example: Suppose you start the day with $10k balance in the account. Reinvesting is set to Daily. At the end of the trading session on day one there are a few opened positions. You then decide to withdraw $5k from the account. Let's imagine the new balance is now $5k. On the following day, when the session starts if it would recalculate the lot sizes based on the balance, the lot size would halve for new buys resulting in less profit or even loss when the positions close. In order to avoid this scenario the EA will wait until the positions are all closed and then recalculate the lot sizes. 

Notification note: This feature sends push notifications. See *Notifications* below.

### Symbol Rotation


EA will switch from the current symbol, assumed to be something like Bra50XXXYY to a new symbol X days before the current symbol expires. It will try to rotate to the new symbol every new day if there are no positions opened. Otherwise it will be postponed until all the positions are closed. It will only switch on a 

Note: Currently, the chart won't be reloaded to the new symbol once the rotation has been done. Meaning that while it will be operating on the new symbol, the chart won't match the positions and won't show profits. It will operate normally on the new symbol but things will look strange in the chart. It's recommended to load a new chart window with the new symbol. This can be done at any time, even with positions opened.

Notification note: This feature sends push notifications. See *Notifications* below.

### Dynamic Take Profit

Allows to set different Take Profit levels depending on the number of opened positions. This only applies when the average price is applied.

For example: Let's assume *Take Profit(in points) when average price is calculated* is set to 50, *Level One Open buys threshold* is set to 10 and *Level One Take Profit* is set to 100. In this scenario, if the number of open buys is equal to or less than 10, it will use 50 points as the Take Profit on top of the average price. If another buy order would occur, now with 11 open buys positions, take profit of 100 would be used.

If you do not wish to use this feature set *Level One Open buys threshold* to zero.

Note 1: *Level Two Open buys threshold* must be greater than *Level One Open buys threshold*. If you do not intend to use the second level you can set it to a very high, unlikely number of open positions like 10000 or simply set it to greater than *Level One Open buys threshold* and specify *Level Two Take Profit*  to be the same as *Level One Take Profit*

Note 2: Take profit is just an arbitrary integer number. It could be anything you want, including negative numbers(which usually results in little to no profit)

### Gradient Buys

Allow the EA to change the lot size based on the number of current open positions. There are two levels to be configured. You could say something like after 10 open positions the lot size will be 2. Until then the Lot Size in the Basic setting will be used.

If you do not wish to use this feature set *Level One Trigger.* to zero.

### Auto TMF

It's a feature to adjust the timeframe of the chart and lot size based on the number of open positions.

....

Upon closing of all opened positions the timeframe will change to the initial time frame set in the parameters.

### Flow Control

Blocks new orders when equity is below a certain level.

With this feature on, a new buy signal will be ignored if the previous order was placed less than X minutes specified in the parameters `Time-out(in minutes) for new buy orders when FC is on`. A mark will show in the chart where the buy signal was detected but skipped.

To enable this feature, the parameter `Time-out(in minutes) for new buy orders when FC is on` must be greater than zero.

Note: This feature seems to work best with larger lot sizes and it will protect consecutive buys that could otherwise result in high levels of drawdown. Running this feature with an appropriate lot size will simply delay closing positions.

### Notifications

These are push notification that requires the **Meta Trader 5** app on your phone. In order to set it up in the MT5 on your desktop select Tools - Options - Notifications tab. Check **Enable Push notifications**, enter the **MetaQuotes ID** from the App in the appropriate field and hit **Test**.

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

You will also be notified of high levels of packet loss/retransmission as reported by MT5. Note it doesn't necessarily mean an issue in the connection with the broker as this metrics is retrieved from the OS and includes any connection on the system.

#### Symbol about to expire

Even with Symbol Rotation off, daily, starting 5 days before the symbol expires you will be notified the current symbol is about to expire so you can take action.

#### Symbol Rotation completed

A notification will be sent once the symbol has switched to the new symbol as per Symbol Rotation settings.


Note: Checking **Notifications from the local terminal/trade server** will send a message every time an order is placed, modified, closed. I found this "spammy".

#### New Symbol Detected

If a symbol(case-sensitive) is specified in **Notify when new symbol becomes available**, you will receieve a notification once it becomes available for trading. Useful when running the EA with symbol Bra50 and waiting for the next Bra50XXXYY to become available. Note this feature can not be backtested.



### Other Stuff

#### Stop buying after closing all positions

Offer a simple way to place further orders once the current ones are closed. It's useful if you'd like to pause placing orders as soon as the current ones are closed. Has been used in the past to allow withdraws from the account and also to rotate to a different symbol.

#### Authorization code

The EA requires an authorization code to be loaded on a chart. One code is required per metatrader installation.

Authorization code is not needed when performing backtesting.

#### Info Panel type

Selects how the info panel in the chart will be plotted. It can be turned off, in text mode or graphical mode. This setting controls the info panel used during backtests as well. Note: Graphical mode will slow down progress of ticks in visual mode. If you'd like to speed up the ticks in visual mode either choose Text or turn it off completely if not needed.

### Investing Profiles

Investing profiles can be used to set lot sizes, take profit, gradient buy  and Auto Timeframe setting.  On the current version the profiles have the following settings:

|Low Risk|  |
|--------|--|
| Base balance: | $10k |
|||
| Basics - Take Profit(in points) used when placing new order| 5 |
| Basics - Take Profit(in points) when average price is calculated: | 60 |
| Basics - Buy orders size(in contract): | 0.6 |
|||
|Gradient Buy - Level One Trigger: | 7 |
| Gradient Buy - Level One Lot Size: | 0.8 |
| Gradient Buy - Level Two Trigger: | 10 |
| Gradient Buy - Level Two Lot Size: | 1.2 |
|||
| Dynamic Take Profit - Level One Open buys threshold: | 10 |
| Dynamic Take Profit - Level One Take Profit: | 100 |
| Dynamic Take Profit - Level Two Open buys threshold: | 100 |
| Dynamic Take Profit - Level Two Take Profit: | 100 |
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
| Basics - Take Profit(in points) when average price is calculated: | 120 |
| Basics - Buy orders size(in contract): | 1 |
|||
|Gradient Buy - Level One Trigger: | 7 |
| Gradient Buy - Level One Lot Size: | 1.4 |
| Gradient Buy - Level Two Trigger: | 10 |
| Gradient Buy - Level Two Lot Size: | 1.6 |
|||
| Dynamic Take Profit - Level One Open buys threshold: | 10 |
| Dynamic Take Profit - Level One Take Profit: | 100 |
| Dynamic Take Profit - Level Two Open buys threshold: | 100 |
| Dynamic Take Profit - Level Two Take Profit: | 100 |
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
|Gradient Buy - Level One Trigger: | 7 |
| Gradient Buy - Level One Lot Size: | 2 |
| Gradient Buy - Level Two Trigger: | 10 |
| Gradient Buy - Level Two Lot Size: | 2.2 |
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

## Remarks
- A hedging account is assumed for this EA to work appropriately 
- If you change the average take profit it will be modified on all existing orders in a matter of seconds after the change. There is no need for a new order to be placed for the TP to be recalculated and modified.
- Most of the information in the info panel will be updated every 5 seconds
