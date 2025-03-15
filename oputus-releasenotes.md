# Release Notes

## Installation/upgrades instructions [here](https://github.com/lutierigb/shell-things/blob/main/oputus-readme.md#how-to-installupgrade-it)

## 2025Mar15
- Increases retransmission alerts threshold so it becomes less annoying. Also add remote control option to temporarily silence the alerts

## 2025Mar12
- Resolves bug that could cause profit sharing to crash the EA at the end of the day

## 2025Feb21
- Resolves an old issue with indicators like RSI and BB which could have them pointing to the wrong indicator. Mostly happened when turning ATF On and Off

## 2025Feb19
- improving logging. Every single log now includes timestamp(in server time) and log level
- For EA using Bra50 which don't expire, it will no longer send a notification to the user at the beginning of the day

## 2025Feb16
- tiny improvement to DTP logic ontick

## 2025Feb06
- Yet more improvements to the new MT version feature

## 2025Feb05
- Improves(supresses) logging of the new MT version feature detection

## 2025Feb03
 - New feature: Push notification when a new MT version is detected. Requires one-time setup see [docs](https://github.com/lutierigb/shell-things/blob/main/oputus-readme.md#new-mt-version-availabe)

## 2025Jan22
- force TP to be updated when EA is reloaded when changing from simple to weighted average and vice-versa
- add note to BOD message about stop buying after closing current positions 

## 2024Dec18
- Adds chart symbol to the notifications header
- add note about new buys not allowed in BOD message
- expands remote control to include help and stop buying after closing all (not yet implemented)

## 2024Dec14
- fixing bug that would use long lot size when pressing the sell button on the chart
- removing sell/buy buttons during BT

## 2024Nov30
- fixing bug that would cause short TP to not be updated if the input changed

## 2024Nov25
- Invalid stop errors were seeing because of a bug when calculating minimum TP allowed for short

## 2024Nov14
- fixing bug in TP matching algo that caused TP line to be added when it wasn't really necessary

## 2024Nov05
- improves TP matching algo on init. Preventing EA from updating orders on init if not needed. If the current TP on orders matches with the points specified in the EA then there is no need to update the orders. This fixes an edge case when existing positions would be modified upon market opening if the price is above the orders TP

## 2024Oct23
- new feature: ability to specify lot size for manual operations

## 2024Oct22
- improvments to symbol rotation. It will no longer be deprecated. Should solve issue where it says the rotation failed but actually completed
- attempt to fix bug: extending the window forward to look for recently closed positions

## 2024Sep24
- bug fix: info panel was displaying the wrong current long lot size when GB was on

## 2024Sep01
- bug fix: abruptly terminating the VM caused GVs to not be commited to disk and upon MT restart it would restore from a previous version. this is now fixed
- bug fix: closing a position manually will now force TP to be recalculated

## 2024Aug07
- New feature/major overhaul: adding shorting
- bug fix: back in version 2023101801 the graphical info panel started to slow-down BT in visual mode. This is now fixed.
- new feature: adds capability of withdrawing part of profits during BTs
- new feature: option to recalculate short lot size during BT

## 2024Jul17-01
- Low risk now enforces M5 using auto TMF.

## 2024Jul17
- New Feature: Under Advanced Trading, there is an option to close positions at the end of the day. This feature is unstable at the moment until we move to CTrade and confirm the order execution

## 2024Jul15
- New Feature: Under Advanced Trading, there is an option to skip the first minutes of the day.
Skipping the first 10 minutes for first buys only resolves the issue of medium profile busting the account in Feb 2024 with real ticks

## 2024Jul12
- bug fix: BTAllClosePositions was being called in non-BT environemnt. ie EA loaded on a chart 
- bug fix: Profit Sharing Global Variables were not zeroed out and would contain info from the last report

## 2024Jul9
- New feature: if there are multiples EA on the same machine even running in different MT instances, the one with the largest Magic Number will send a summary of all the profits from all EAs 

## 2024Jul5
- only devs care: better BT stats printed to logs

## 2024Jul4
- only devs care: saving inputs to a file for future reference

## 2024Jun25
- improve behavior of Force Avg Price on Init

## 2024Jun16-2
- bug fux: reinvesting wasn't updated effective long lot GV. Which would cause the EA to load old lot size when reloaded. Does not affect GB lot size

## 2024Jun16
- New Feature: Lines on the chart representing the start/end of the trading session
- New Feature: Buy button on the chart will place a market order. Lot size will be determined by the EA depending on the features config, ie. grading buy
- attempt to fix bug: adding debug info to understand why profit shows as zeros sometimes in the chart
- bug fix: text on chart won't appear over info panel

## 2024Jun15
- attempt to fix bug: Increasing window to look for recently closed positions(used to print the profit on the chart)

## 2024Jun14
- improvements: how lot sizes are recalculated if there are positions open. better version that yesterday's
- bug fix: EA would recalculate initial lot size if reinvesting was on and retrieve lot size from profile was off

## 2024Jun13
- improvement: lot sizes will no longer be recalculated if EA is reloaded and there are positions open. fixed GH issue #3
- improvement:  Push notifications now includes: EA loaded and removed from a chart

## 2024Jun12
- improvement: Push notifications now includes: deposits/withdraw alerts and notification when X positions are closed. Good for those moments when you are praying. fixed GH issue #8

## 2024Jun10
- bug fix: MAE was showing zero in the notifications - fixes GH issues #5

## 2024May27
- Improvements to EOD message and dump config message via remote control

## 2024May14
- Investing Profile tweak(will affect profit/DD): Medium risk profile changes average price TP from 120 points to 100.
- New feature: Dynamic Take Profit is now part of the profiles
- New feature: Maximum spread allowed for new orders
- refactoring: removing extreme_risk profile

## 2024May08
- bug fix: profits calculation now include positions that have been manually closed
- refactoring: Current DTP level and source are now global variables updated only when there is a change
- new feature: added spread level input. Would only allow buys if spread if below specified level

## 2024May02
- Breaking change: Authorization codes algorithm changed. New auth codes will have to be issued. Now the same code will work in multiple MT/accounts as long as they are all part of the same account in the broker
- bug fix: EOD message would report the wrong number of buys when there was a holiday and the broker didn't update the symbol hours

## 2024May01-01
- new feature: adding weighthed average input. Default is still simple

## 2024May01
- Remote control is now a thing! You can place special orders to control the EA. Reads the docs: https://github.com/lutierigb/shell-things/blob/main/oputus-readme.md#remote-control

## 2024Apr30
- bug fix: In some cases DTPv2 would fallback to level one upon start

## 2024Apr29
- New versioning format!
- bug fix: EOD notification had the wrong value for TP. affecting calculations
- New Feature: EA can now manage positions manually created
- New Feature: Avg Price can be forced upon start

## 2024040801
- bug fix: sometimes profits in chart will be printed as $0. should be fixed now but difficult to effetivily test this
- refactoring: moving info panel code to its own file. Removing trailing spaces in some of the files

## 2024040503
- New option: Reinvesting now has "ASAP" that will recalculate lots every time all the positions are closed. Existing .set files are compatible with this version

## 2024040502
- Refactoring: notifications

## 2024040501
- Cosmetic: improves End of the Day push notification to include estimated profit and distance from TP

## 2024040404
 - Refactoring: removing old comments from the code

## 2024040403
 - Refactoring: Global Variable names in the reinvesting code in order to make it less delicate in case there are multiple Oputus' instances in the same MT

## 2024040402
- Further improvements to detecting a failed symbol rotation

## 2024040401
- bug fix: Symbol rotation would fail to rotate if the future symbol is not in the Market Watch window. Now the future symbol is added in case it wasn't there already. Requiring no actions from the user
- bug fix: Symbol rotation would send notification saying the rotation completed when it failed. Fixed.

## 2024032501
- Added a new input to select what to do once there is only one open buy left. Default has always been to keep it but discussion was raised on the possibility of closing it. There is now an option to keep it as is, close it and close it only if profitable. 

## 2024032201
- New feature(will affect profit/DD): DTP has graduated to DTP v2. Now DTP is based of previous day price and an optional moving average. Read the docs for more info
- Cosmetic change: EA Number is now one of the first inputs

## 2024021601
- Significant change(will affect profit/DD): When using ATF, there could be two consecutive buys when switching timeframes, either at the same price or very close. For example, with the initial timeframe set to M3, it could buy at 13:06, switch to M5 after one buy and place another other at 13:06(because the previous M5 candle also closed below the inferior BB). Fix has been added to avoid this edge case. Now, a new buy will only be allowed at 13:10, in this example

## 2024021101
- Significant change(will affect profit/DD): Investing profile Low has one of its level one triggered changed from 7 to 8. This allows the low risk profile to pass during the covid 19 market in Feb 2020

## 2024021101
- removing trading hours check from buy signal. It would just create inconsistent results during BT as it would only trade during symbol trading hours which changed throughout the year. Now it should trade on every tick regardless of the trading hours specified on the symbol

## 2023121001
- new feature: Due to popular request a new input has been added. Called **Percentage of the balance to use for lot size calculations** it allows to use a given percentage of the account balance(less or more) when calculating lot size(mainly when using investing profiles and/or reinvesting). For example, in an account with $10k balance, if the new setting is set to 90, when using investing profiles, lot sizes will be calculated based on $9k(90% of 10k) instead of the actual account balance. Note: the actual balance used for lot size calculations is rounded down to the closest $100. So considering an account with balance of $9999, and the new setting set to 90, the actual balance to be used in the calculation will be as follows: 9999 * 90% = 8999.1 rounding it down it will result in 8900. 8900 will then be used when calculating lot sizes. This is done to avoid fraction of lot sizes.

## 2023102401
- bug fix: some global variables would retain their old value when parameters changed in the EA among other things would cause
the infopanel to show duplicated info. 
- improvements: moved groups of paremeters around to an order that makes more sense

## 2023102302
- bug fix: in rare cases(rapid price movement) ATF would switch timeframes quickly and multiple buys per bar would happen
this fix takes care of that by checking the current bar time with the last buy

## 2023102301
- improvement: Comments are now rotated.

## 2023102201
- improvement: SOD and EOD push notifications. ie. include equity in different formats.

## 2023101801
- improvement: info panel now updates every tick and only the parts that are variable are reloaded. Static parts aren't touched. Should save a few CPU cycles.
- improvement: current take profit is now easier to understand in the info panel
- new feature: New input **Info Panel type**, allows to select what kind of info panel will be plotted on the chart. Note: Graphical info panel slows down the ticks when running Strategy Tester in Visual Mode.

## 2023101501
- improvement: EOD now counts today's ops before sending notification instead of relying on global variable

## 2023101303
- fix: some profits and ATF messages were not being plotted in the chart

## 2023101302
improvement: new symbol detection won't check for new symbol if chart's symbol is the one being checked

## 2023101301
Retransmission threshold is now a parameter/inputs

## 2023101001
- increases the retransmission rate from 3% to 5% to avoid unnecessary notifications
- changes default value of parameter **# of days before expiration to start rotation** in Symbol Rotation from 5 to 10

## 2023100801
- New Feature: Capable of detecting and notifying when a new symbol is available

## 2023100601
- re-words Start of Day notification
- small changes to symbol rotation routine including new pre-change message and post-change message

## 2023100502
- adds equity to end of day notification
- bug fix: symbol rotation silently failed to reload chart on new symbol

## 2023100501

- Adds info panel in visual mode

## 2023100301

- Updating investing profiles.


## 20230926

- Includes de EA Name in the push notifications

## 2023092103

- Adds support to Auto Timeframe from investing profiles. Now, investing profiles can control ATF
