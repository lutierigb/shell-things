**Latest version:** rola-rola2024Jun11.ex5

# Intro to Rola-Rola

Rola-Rola is a one-shot Expert Advisor(EA) created to migrate open positions from one symbol to another. It's usually required when trading with symbols that come with an expiration date

# What does it do?
It will migrate/roll-over open buy(and buys only) positions from symbol A to symbol B. It won't touch sell positions.

Starting with the oldest position in symbol A, it will create a copy of this position in Symbol B, if successful then it will close the original position.

This process will repeat until it moves all positions to symbol B or a failure happens.

# What else does it do?

Upon start it will check how far the positions in symbol A are from their Take profit and display that information.

To and From symbol names will be validated to confirm they exist.

It will also calculate and apply the take profit for the new positions, taking into consideration the current market rates. 

At the end of the process it will compare the number of open positions in symbol A with the number of open positions in symbol B and will
let you know if all positions have been migrated successfully. 

It will display what the estimated profit was in symbol A(before the roll-over). What the estimated profit will be in symbol B and the net profit.

It will also show you the number of points to set as target(take profit) on your regular EA.

All the information is printed on the chart as well as on the Experts tab.

# How to use it

1. Remove your existing EA from the chart so it won't place or modify orders during the migration
2. In the Toolbox Panel - Trade tab, right-click any open positions, select Report, then HTML(Internet Explorer)
3. Open a new chart in MT5, the symbol does not matter
4. Load the Rola-Rola EA onto the chart
5. Make sure "Allow Algo Trading" is checked
6. In the Inputs tab specify the "From" and "To" symbol. Note: Symbol names are case-sensitive. "Bra50Jun28" is an example of a valid name
7. Hit OK to start the roll-over process

Once the process completes the EA will be removed from the chart.

Now:
1. Load a new chart with symbol B
2. Load your regular EA(Manche, Oputus, Flentza, etc) and set the take profit to be the amount of points displayed at the end of Rola-Rola execution
3. You are done!
