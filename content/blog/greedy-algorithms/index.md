---
title: Greedy Algorithms
date: "2024-06-27T22:40:32.169Z"
description: Understanding Greedy Algorithms.
---


A greedy algorithm is a problem-solving strategy that makes a sequence of choices, each of which looks the best at the moment. The idea is to build up a solution piece by piece, always choosing the next piece that offers the most immediate benefit or that seems best in terms of the given criteria. 

### Characteristics of Greedy Algorithms:
1. **Greedy Choice Property**: A global optimal solution can be arrived at by selecting a local optimal choice.
2. **Optimal Substructure**: A problem has an optimal substructure if an optimal solution to the problem contains optimal solutions to the sub-problems.

### Steps in a Greedy Algorithm:
1. **Initialization**: Initialize the solution to be empty.
2. **Selection**: Select the best option available at the current moment.
3. **Feasibility**: Check if the selected option can be included in the solution.
4. **Inclusion**: Include the selected option in the solution.
5. **Repeat**: Repeat the selection and inclusion steps until the entire solution is constructed.

### Examples of Greedy Algorithms:

#### 1. **Activity Selection Problem**:
Given a set of activities with start and finish times, select the maximum number of activities that don't overlap.

**Algorithm**:
1. Sort activities by their finish times.
2. Select the first activity.
3. For each remaining activity, select it if its start time is greater than or equal to the finish time of the last selected activity.

#### 2. **Huffman Coding**:
A method for creating a prefix-free binary tree that minimizes the total cost of encoding a set of characters with varying frequencies.

**Algorithm**:
1. Create a priority queue of all characters, with their frequencies as priorities.
2. While there is more than one node in the queue:
   - Extract the two nodes with the smallest frequencies.
   - Create a new internal node with these two nodes as children and frequency equal to the sum of their frequencies.
   - Insert the new node back into the queue.
3. The remaining node is the root of the Huffman tree.

#### 3. **Fractional Knapsack Problem**:
Given weights and values of items, determine the maximum value that can be accommodated in a knapsack of fixed capacity, where you can take fractions of items.

**Algorithm**:
1. Calculate the value-to-weight ratio for each item.
2. Sort items by this ratio in descending order.
3. Take as much as possible of the item with the highest ratio, then the next highest, and so on until the knapsack is full.

### Advantages and Disadvantages:

**Advantages**:
- Generally easy to design and implement.
- Often provide good solutions for optimization problems.
- Fast execution time due to making a single pass through the data.

**Disadvantages**:
- Doesn't always provide the optimal solution for all problems.
- May require additional steps to verify if the solution is truly optimal.
- Not suitable for problems where future choices depend on previous choices.

### When to Use:
- When the problem has the greedy choice property and optimal substructure.
- When a quick, approximate solution is acceptable.
- When exact solutions are computationally expensive or complex.

Greedy algorithms are widely used because of their simplicity and efficiency in many practical applications.


## Application Questions

### Minimum Number of Coins

Write a function that takes in an array of N integers and performs a given operation k times. Afterwards return the outcome.

Okay, let's solve this problem step-by-step:

1. We have coins of values 1, 2, and 5.
2. The amount we need to pay is 6.
3. To find the minimum number of coins, we can use a greedy approach:
   - Start with the largest coin value (5) and use as many of those as possible.
   - Then, use the next largest coin value (2) to make up the remaining amount.
   - Finally, use the smallest coin value (1) to make up any remaining amount.

Applying this greedy approach:
* Use 1 coin of value 5 (leaving 1 remaining)
* Use 0 coins of value 2 (leaving 1 remaining)
* Use 1 coin of value 1 (leaving 0 remaining)

Therefore, the minimum number of coins required to pay 6 is 2 coins (1 coin of value 5, and 1 coin of value 1).

```
function minCoins(coins, amount) {
  // Sort the coins in descending order
  coins.sort((a, b) => b - a);

  let numCoins = 0;
  for (let i = 0; i < coins.length; i++) {
    // Use as many of the current coin value as possible
    let count = Math.floor(amount / coins[i]);
    numCoins += count;
    amount -= count * coins[i];
  }

  return numCoins;
}

// Example usage
const coins = [1, 2, 5];
const amount = 6;
const minCoinsRequired = minCoins(coins, amount);
console.log(minCoinsRequired); // Output: 2
```

### Maximum Number of Events
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

To determine the maximum number of presentations that can be hosted during the career fair, given the constraints and requirements, we can use a greedy algorithm. The goal is to maximize the number of non-overlapping presentations.

Here's a step-by-step approach to solving the problem:

### 1. **Understanding the Problem:**

Each company has a specific arrival time and duration, which determines when it will start and when it will end. The key challenge is to schedule the maximum number of non-overlapping presentations.

### 2. **Transform the Input:**

You need to convert the input into intervals of time. For each company:
- **Start Time:** `arrival[i]`
- **End Time:** `arrival[i] + duration[i]`

### 3. **Greedy Strategy:**

The problem can be approached using a greedy algorithm that schedules presentations in a way that maximizes the number of presentations. Specifically:
- **Sort the Presentations:** Sort all presentations based on their end times. This ensures that you always consider the earliest finishing presentation first.
- **Select Presentations:** Iteratively select presentations that start after the last selected presentation ends.

### 4. **Algorithm:**

1. **Create a list of intervals** based on arrival times and durations.
2. **Sort the intervals** by their end times.
3. **Iterate through the sorted list** and select the presentations that do not overlap with the previously selected ones.

Here’s a Python function that implements this approach:

```python
def max_presentations(arrival, duration):
    n = len(arrival)
    
    # Create a list of (start_time, end_time) tuples
    intervals = [(arrival[i], arrival[i] + duration[i]) for i in range(n)]
    
    # Sort intervals by end time
    intervals.sort(key=lambda x: x[1])
    
    max_count = 0
    last_end_time = 0
    
    # Iterate through the sorted intervals
    for start, end in intervals:
        if start >= last_end_time:
            max_count += 1
            last_end_time = end
    
    return max_count

# Example usage
arrival = [1, 3, 3, 5, 7]
duration = [2, 2, 1, 2, 1]
print(max_presentations(arrival, duration))  # Output: 4
```

### 5. **Explanation:**

- **Step 1:** Convert the arrival times and durations into intervals with start and end times.
- **Step 2:** Sort these intervals by their end times. This helps in maximizing the number of non-overlapping intervals because we always consider the interval that finishes the earliest.
- **Step 3:** Use a greedy approach to select the maximum number of non-overlapping intervals by keeping track of the end time of the last selected interval and ensuring that each new interval starts after the last selected interval ends.

### **Complexity:**

- **Time Complexity:** Sorting the intervals takes \(O(n \log n)\), and iterating through the intervals takes \(O(n)\). Therefore, the overall time complexity is \(O(n \log n)\).
- **Space Complexity:** Storing the intervals takes \(O(n)\) space.

This approach efficiently solves the problem within the given constraints.