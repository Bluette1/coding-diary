---
title: Dynamic Programming
date: "2024-06-27T22:40:32.169Z"
description: Understanding Dynammic Programming.
---

Dynamic Programming (DP) is a method used in algorithm design and computer science to solve problems by breaking them down into simpler subproblems. It is particularly useful in scenarios where the same subproblems are solved multiple times. Instead of solving the same problem repeatedly, dynamic programming solves each subproblem just once and stores the results for future use. This approach reduces the computational overhead and improves efficiency.

### Key Concepts in Dynamic Programming

1. **Overlapping Subproblems**:
   - Problems can be broken down into smaller, simpler subproblems, which are solved independently.
   - These subproblems often overlap, meaning the same subproblem is solved multiple times.

2. **Optimal Substructure**:
   - An optimal solution to the problem can be constructed efficiently from optimal solutions of its subproblems.
   - This property ensures that the solution of a problem consists of the solutions to its subproblems.

3. **Memoization**:
   - This technique involves storing the results of expensive function calls and reusing them when the same inputs occur again.
   - Typically implemented using a table (array or hash map).

4. **Tabulation**:
   - This technique involves solving subproblems in a bottom-up manner, storing results in a table, and building up to the final solution.

### Example: Fibonacci Sequence

The Fibonacci sequence is a classic example where dynamic programming can be applied. The naive recursive solution has a lot of redundant calculations:

```javascript
function fib(n) {
    if (n <= 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```

This recursive approach has an exponential time complexity because it recalculates the same values many times. We can improve it using dynamic programming:

#### Memoization Approach

```javascript
function fib(n, memo = {}) {
    if (n in memo) {
        return memo[n];
    }
    if (n <= 1) {
        return n;
    }
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
    return memo[n];
}
```

#### Tabulation Approach

```javascript
function fib(n) {
    if (n <= 1) {
        return n;
    }
    let fibTable = [0, 1];
    for (let i = 2; i <= n; i++) {
        fibTable[i] = fibTable[i - 1] + fibTable[i - 2];
    }
    return fibTable[n];
}
```

### Example: Maximum Number of Events
A team organizing a university career fair has a list of companies along with their respective arrival time and duration to stay.
Only one company can present at any time. Given each company arrival time and duration they will stay, determine the maximum number of presentation that can be hosted during the career fair.

Constraint
n = number of companies 1 <= n <= 50
1 < arrival[i] < 1000
1 < duration[i] < 1000
Both arrival and duration must have an equal number of elements

Example Input & Output

n = 5
arrival = [1, 3, 3, 5, 7]
duration = [2, 2, 1, 2, 1]

There is an alternative approach to solving the problem of maximizing the number of non-overlapping presentations. While the greedy algorithm is commonly used and efficient, another method involves using dynamic programming (DP). This approach can also be effective, particularly in scenarios where a more exhaustive exploration of combinations is feasible.

### **Dynamic Programming Approach**

**Overview:**
Dynamic programming can be used to solve this problem by building up a solution based on smaller subproblems. This approach is more exhaustive compared to the greedy algorithm but provides a good alternative.

**Steps to Solve the Problem Using Dynamic Programming:**

1. **Convert to Intervals:**
   Convert the arrival and duration lists into a list of intervals (start time, end time) as done previously.

2. **Sort the Intervals:**
   Sort the intervals by their end times. This is useful for the DP approach to handle overlapping efficiently.

3. **DP Array Initialization:**
   Use a DP array where `dp[i]` represents the maximum number of presentations that can be scheduled up to the `i-th` interval.

4. **Transition:**
   For each interval, determine the maximum number of presentations that can be scheduled by including or excluding the current interval. 

5. **Backtracking for Non-Overlapping Intervals:**
   Use binary search or a loop to find the last non-overlapping interval before the current one.

Here's how you can implement this using Python:

```python
def max_presentations_dp(arrival, duration):
    import bisect
    
    n = len(arrival)
    
    # Create a list of (start_time, end_time) tuples
    intervals = [(arrival[i], arrival[i] + duration[i]) for i in range(n)]
    
    # Sort intervals by end time
    intervals.sort(key=lambda x: x[1])
    
    # Extract start times and end times for binary search convenience
    starts = [intervals[i][0] for i in range(n)]
    ends = [intervals[i][1] for i in range(n)]
    
    # Initialize DP array
    dp = [0] * n
    
    # Base case: the first interval alone
    dp[0] = 1
    
    # Fill DP array
    for i in range(1, n):
        # Option 1: Do not include the current interval
        dp[i] = dp[i - 1]
        
        # Option 2: Include the current interval
        # Find the last interval that doesn't overlap with the current one
        # Use binary search to find the rightmost interval that ends before the current starts
        last_non_overlap_index = bisect.bisect_right(ends, starts[i] - 1) - 1
        
        if last_non_overlap_index >= 0:
            dp[i] = max(dp[i], dp[last_non_overlap_index] + 1)
        else:
            dp[i] = max(dp[i], 1)
    
    # The maximum number of presentations is the value in the last cell of the DP array
    return dp[-1]

# Example usage
arrival = [1, 3, 3, 5, 7]
duration = [2, 2, 1, 2, 1]
print(max_presentations_dp(arrival, duration))  # Output: 4
```

### **Explanation:**

1. **Convert to Intervals:** Convert the arrival and duration into start and end times for each interval.
2. **Sort Intervals:** Sort the intervals by their end times to facilitate efficient DP processing.
3. **Binary Search:** Use binary search to find the latest interval that ends before the current interval starts. This helps in efficiently finding non-overlapping intervals.
4. **DP Array Update:** Update the DP array to reflect the maximum number of non-overlapping intervals considering whether to include the current interval or not.

### **Complexity:**

- **Time Complexity:** Sorting the intervals takes \(O(n \log n)\). The DP processing for each interval involves a binary search which also takes \(O(\log n)\). Therefore, the overall time complexity is \(O(n \log n)\).
- **Space Complexity:** Storing the intervals and DP array takes \(O(n)\) space.

The dynamic programming approach provides an alternative to the greedy algorithm and can be particularly useful when exploring all possible combinations is necessary. Both methods are efficient, with \(O(n \log n)\) time complexity, but the DP approach offers a more comprehensive way of solving the problem through a different methodology.


### When to Use Dynamic Programming

- Use DP when the problem can be divided into overlapping subproblems that can be solved independently.
- Look for optimal substructure in the problem, where the global optimal solution can be derived from local optimal solutions.
- DP is particularly useful for problems involving sequences, such as longest common subsequence, shortest paths in graphs, and knapsack problems.

### Conclusion

Dynamic Programming is a powerful technique for solving complex problems by breaking them down into simpler, reusable subproblems. It involves two main approaches: memoization and tabulation, both of which help in reducing the overall time complexity of algorithms by avoiding redundant calculations.
