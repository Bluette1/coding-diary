---
title: Coding Test - Coder:AI Training
date: "2024-05-29T22:40:32.169Z"
description: This is a description of my experience during the coding test.
---

I had a coding test today through BrainTrust for the parttime role of Coder-AI Training (Remotasks).

## Question

Write a function that takes in an array of N integers and performs a given operation k times. Afterwards return the outcome

Operation: 
- Choose any number
- culate the cieling of a third of that number
- Replace the number in the array with this result
- Add that number to max score

First Solution

```
const max_score = (nums, k)=>{
 let max_result = 0

  for (let i = 0; i < k; i += 1) {
    let choice = Math.max(...nums)

    max_result += choice

    let index = nums.indexOf(choice)

    nums[index] = Math.ceil(choice / 3);

  }
  return max_result


}
```

Second Attempt

```
const max_score = (nums, k)=>{
 let max_result = 0

 const sortedNums = [...nums].sort((a, b) => b - a)

  for (let i = 0; i < k; i += 1) {
    let choice = sortedNums[i] 
    
    max_result += choice

    let index = nums.indexOf(choice)

    nums[index] = Math.ceil(choice / 3);

  }
  return max_result
}
```

Best Solution

