# Sliding Window Problems – Complete Guide (with Solutions in TypeScript)

This document contains **20 sliding window problems** arranged in **progression order**.  
Each includes:
- **Problem Description**
- **Brute Force Approach (TypeScript)**  
- **Optimized Sliding Window Solution (TypeScript)**  

---

## Table of Contents

1. [Maximum Sum Subarray of Size K](#1-maximum-sum-subarray-of-size-k)  
2. [Minimum Sum Subarray of Size K](#2-minimum-sum-subarray-of-size-k)  
3. [First Negative Number in Every Window of Size K](#3-first-negative-number-in-every-window-of-size-k)  
4. [Average of All Subarrays of Size K](#4-average-of-all-subarrays-of-size-k)  
5. [Count Occurrences of Anagrams](#5-count-occurrences-of-anagrams)  
6. [Longest Substring with K Distinct Characters](#6-longest-substring-with-k-distinct-characters)  
7. [Longest Substring Without Repeating Characters](#7-longest-substring-without-repeating-characters)  
8. [Minimum Window Substring](#8-minimum-window-substring)  
9. [Longest Substring with At Most Two Distinct Characters](#9-longest-substring-with-at-most-two-distinct-characters)  
10. [Fruit Into Baskets](#10-fruit-into-baskets)  
11. [Permutation in String](#11-permutation-in-string)  
12. [Longest Repeating Character Replacement](#12-longest-repeating-character-replacement)  
13. [Binary Subarray with Sum](#13-binary-subarray-with-sum)  
14. [Max Consecutive Ones III](#14-max-consecutive-ones-iii)  
15. [Sliding Window Maximum](#15-sliding-window-maximum)  
16. [Longest Subarray of Sum at Most K](#16-longest-subarray-of-sum-at-most-k)  
17. [Subarrays with K Different Integers](#17-subarrays-with-k-different-integers)  
18. [Shortest Subarray with Sum ≥ K](#18-shortest-subarray-with-sum-k)  
19. [Maximize the Confusion of an Exam](#19-maximize-the-confusion-of-an-exam)  
20. [Longest Subarray with Equal Number of 0 and 1](#20-longest-subarray-with-equal-number-of-0-and-1)  

---

## 1. Maximum Sum Subarray of Size K

**Problem:** Given an array of integers and a number k, find the maximum sum of any contiguous subarray of size k.

### Brute Force Approach
- Check every subarray/window.
- Time Complexity: O(n*k)

```typescript
function maxSumSubarrayBrute(arr: number[], k: number): number {
  let maxSum = -Infinity;
  for (let i = 0; i <= arr.length - k; i++) {
    let sum = 0;
    for (let j = i; j < i + k; j++) {
      sum += arr[j];
    }
    maxSum = Math.max(maxSum, sum);
  }
  return maxSum;
}
```

### Optimized Sliding Window
- Maintain running sum or map to reduce to O(n).

```typescript
function maxSumSubarray(arr: number[], k: number): number {
  let sum = 0;
  for (let i = 0; i < k; i++) sum += arr[i];
  let maxSum = sum;
  for (let i = k; i < arr.length; i++) {
    sum += arr[i] - arr[i - k];
    maxSum = Math.max(maxSum, sum);
  }
  return maxSum;
}
```

---

## 2. Minimum Sum Subarray of Size K

**Problem:** Find the minimum sum of any contiguous subarray of size k.

### Brute Force Approach
- Check every subarray/window.
- Time Complexity: O(n*k)

```typescript
function minSumSubarrayBrute(arr: number[], k: number): number {
  let minSum = Infinity;
  for (let i = 0; i <= arr.length - k; i++) {
    let sum = 0;
    for (let j = i; j < i + k; j++) sum += arr[j];
    minSum = Math.min(minSum, sum);
  }
  return minSum;
}
```

### Optimized Sliding Window
- Maintain running sum or map to reduce to O(n).

```typescript
function minSumSubarray(arr: number[], k: number): number {
  let sum = 0;
  for (let i = 0; i < k; i++) sum += arr[i];
  let minSum = sum;
  for (let i = k; i < arr.length; i++) {
    sum += arr[i] - arr[i - k];
    minSum = Math.min(minSum, sum);
  }
  return minSum;
}
```

---
