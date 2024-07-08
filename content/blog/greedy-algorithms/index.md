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


## Application Question

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