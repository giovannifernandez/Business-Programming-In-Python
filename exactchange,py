# A program with total change amount as an integer input, and output the change using the fewest coins, one coin type per line. 
# The coin types are Dollars, Quarters, Dimes, Nickels, and Pennies. Use singular and plural coin names as appropriate, like 1 Penny vs. 2 Pennies.

total = int(input())

dollar_value = 100
quarter_value = 25
dime_value = 10
nickel_value = 5
penny_value = 1

dollar_count = total // dollar_value
total %= dollar_value

quarter_count = total // quarter_value
total %= quarter_value

dime_count = total // dime_value
total %= dime_value

nickel_count = total // nickel_value
total %= nickel_value

penny_count = total // penny_value

if dollar_count > 0:
    if dollar_count == 1:
        print('1 Dollar')
    else:
        print(f'{dollar_count} Dollars')

if quarter_count > 0:
    if quarter_count == 1:
        print('1 Quarter')
    else:
        print(f'{quarter_count} Quarters')

if dime_count > 0:
    if dime_count == 1:
        print('1 Dime')
    else:
        print(f'{dime_count} Dimes')

if nickel_count > 0:
    if nickel_count == 1:
        print('1 Nickel')
    else:
        print(f'{nickel_count} Nickels')

if penny_count > 0:
    if penny_count == 1:
        print('1 Penny')
    else:
        print(f'{penny_count} Pennies')
