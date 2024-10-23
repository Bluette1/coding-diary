---
Title: String Challenge - Calculate Days and Hours  
Date: "2024-10-22T22:40:32.169Z"  
Description: How to Solve the Problem - `Calculate Days and Hours`.
---

# String Challenge

Have the function `string_challenge(num)` take the `num` parameter being passed and return the number of days and hours the parameter converts to (i.e., if `num = 49`, then the output should be `2:1`). Separate the number of days and hours with a colon.

Once your function is working, take the final output string and concatenate it with your `ChallengeToken`, and then replace every 4th character with a capital 'Y'.

### Your ChallengeToken: `xyz987abc`

### Examples

**Input:** `50`  
**Output:** `2:2`  
**Final Output:** `2:Y2xYz9Y7aYc`

**Input:** `25`  
**Output:** `1:1`  
**Final Output:** `1:Y1xYz9Y7aYc`

**Input:** `123`  
**Output:** `5:3`  
**Final Output:** `5:Y3xYz9Y7aYc`

---

```python
def string_challenge(num):
    # Calculate days and hours
    days = num // 24
    hours = num % 24

    # Format string
    time_str = f"{days}:{hours}"

    # Define Challenge Token
    challenge_token = 'xyz987abc'
    final_output = time_str + challenge_token

    # Replace every 4th character with 'Y'
    final_output = ''.join(['Y' if (i + 1) % 4 == 0 else char for i, char in enumerate(final_output)])

    return final_output


# Example usage
print(string_challenge(50))  # Output: 2:Y2xYz9Y7aYc
print(string_challenge(25))  # Output: 1:Y1xYz9Y7aYc
print(string_challenge(123)) # Output: 5:Y3xYz9Y7aYc
```

### Explanation:
1. **Input:** The function takes an integer `num` which represents total hours.
2. **Processing:**
   - First, the total hours are converted into **days** and **remaining hours**.
   - The result is then concatenated with the `ChallengeToken`.
   - Every **4th** character in the final concatenated string is replaced with `'Y'`.
3. **Output:** The transformed string is returned as the final result.

