---
title: Sliding Maximum Problem
date: "2024-08-29T22:40:32.169Z"
description: How to Solve the Sliding Maximum Problem.
---

To solve the problem of finding the largest number in every `k` continuous numbers in an array with an optimal solution of \(O(n)\), you can use a deque data structure. This approach efficiently maintains the indices of potential maximum values for each window.

Here's a Python implementation:

```python
from collections import deque

def max_in_k_windows(arr, k):
    if not arr or k <= 0:
        return []

    result = []
    deq = deque()  # Stores indices of potential max elements

    for i in range(len(arr)):
        # Remove indices that are out of the current window
        if deq and deq[0] < i - k + 1:
            deq.popleft()

        # Remove indices whose corresponding values are less than arr[i]
        while deq and arr[deq[-1]] < arr[i]:
            deq.pop()

        # Add the current index
        deq.append(i)

        # Add the maximum for the current window to the result list
        if i >= k - 1:
            result.append(arr[deq[0]])

    return result

# Example usage
arr = [1, 3, 2, 5, 8, 7, 9, 6]
k = 3
print(max_in_k_windows(arr, k))  # Output: [3, 5, 8, 8, 9, 9]
```

### Explanation:

1. **Deque Usage**: We use a deque to keep track of the indices of the array elements. The deque helps in maintaining the order of potential maximums in the current window.

2. **Window Management**:

   - Remove indices from the front if they are out of the current window.
   - Maintain elements in the deque in decreasing order of their values. Remove elements from the back if they are smaller than the current element as they can't be the maximum for the current or future windows.

3. **Result Collection**: Once the first `k` elements are processed, add the front of the deque to the result list, which represents the maximum of the current window.

This approach ensures each element is added and removed from the deque at most once, resulting in an \(O(n)\) time complexity.


Here's how you can implement the solution in JavaScript using a deque:

```javascript
function maxInKWindows(arr, k) {
    if (!arr.length || k <= 0) return [];

    const result = [];
    const deque = [];

    for (let i = 0; i < arr.length; i++) {
        // Remove indices that are out of the current window
        if (deque.length && deque[0] < i - k + 1) {
            deque.shift();
        }

        // Remove indices whose corresponding values are less than arr[i]
        while (deque.length && arr[deque[deque.length - 1]] < arr[i]) {
            deque.pop();
        }

        // Add the current index
        deque.push(i);

        // Add the maximum for the current window to the result array
        if (i >= k - 1) {
            result.push(arr[deque[0]]);
        }
    }

    return result;
}

// Example usage
const arr = [1, 3, 2, 5, 8, 7, 9, 6];
const k = 3;
console.log(maxInKWindows(arr, k)); // Output: [3, 5, 8, 8, 9, 9]
```

### Explanation:

- **Deque Usage**: We use a deque (implemented as an array) to keep track of the indices of potential maximum values within each sliding window.

- **Window Management**:
  - Remove indices from the front if they are out of the current window.
  - Maintain elements in the deque in decreasing order of their values, removing elements from the back if they are smaller than the current element.

- **Result Collection**: Once the first `k` elements are processed, add the front of the deque to the result array, which represents the maximum of the current window.

This implementation efficiently processes each element in the array and maintains the maximum for each window with an overall time complexity of \(O(n)\).
