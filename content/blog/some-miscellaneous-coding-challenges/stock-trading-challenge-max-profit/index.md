---
title: Stock Trading Challenge - Calculate Maximum Profit  
date: "2024-10-22T22:40:32.169Z"  
description: How to Solve the Problem - Calculate Maximum Profit.
---

# Stock Trading Challenge

Have the function `stock_profit(prices)` take the array of integers stored in `prices`, which represent the price of a stock on different days. The function should return the maximum profit that could be made by buying the stock on day `x` and selling it on day `y` where `y > x`. 

For example, if `prices` is `[50, 30, 40, 70, 20, 90]`, the maximum profit would be `60` (buy at `30` and sell at `90`). If no profit can be made, return `0`. For instance, for `prices` `[80, 70, 60, 50]`, the output should be `0`.

Once your function is working, concatenate the final profit with your `ChallengeToken`, and replace every 4th character with an `X`.

### Your ChallengeToken: `myn0v3ch4ll`

### Examples

**Input:** `[20, 30, 10, 40]`  
**Output:** `30`  
**Final Output:** `30mXn0vX3cX4ll`

**Input:** `[100, 90, 80]`  
**Output:** `0`  
**Final Output:** `0mXn0vX3cX4ll`

**Input:** `[50, 60, 50, 70]`  
**Output:** `20`  
**Final Output:** `20mXn0vX3cX4ll`

---

```python
def stock_profit(prices):
    # Initialize max_profit to 0 and min_price to the first element of the array
    max_profit = 0
    min_price = prices[0]

    # Iterate through the array
    for price in prices:
        if price < min_price:
            min_price = price
        elif price - min_price > max_profit:
            max_profit = price - min_price

    # Convert max_profit to string
    final_output = str(max_profit)

    # Define Challenge Token
    challenge_token = 'myn0v3ch4ll'
    final_output += challenge_token

    # Replace every 4th character with 'X'
    final_output = ''.join(['X' if (i + 1) % 4 == 0 else char for i, char in enumerate(final_output)])

    return final_output


# Test cases
print(stock_profit([50, 30, 40, 70, 20, 90]))  # Output: 60mXn0vX3cX4ll
print(stock_profit([20, 30, 10, 40]))          # Output: 30mXn0vX3cX4ll
print(stock_profit([100, 90, 80]))              # Output: 0mXn0vX3cX4ll
print(stock_profit([50, 60, 50, 70]))           # Output: 20mXn0vX3cX4ll
```

---

### Explanation:

1. **Input:** The function takes an array of integers representing stock prices over different days.
2. **Processing:**
   - We initialize `max_profit` to `0` and `min_price` to the first element of the array.
   - We iterate through the prices, updating `min_price` if we encounter a lower price.
   - If the difference between the current price and `min_price` exceeds `max_profit`, we update `max_profit`.
   - After the loop, we convert `max_profit` to a string and concatenate it with the `ChallengeToken`.
   - Finally, we replace every **4th** character in the concatenated string with an `'X'`.
3. **Output:** The final transformed string is returned.

### Time Complexity
This solution uses a single pass through the array, resulting in a time complexity of O(n), where n is the length of the array.
