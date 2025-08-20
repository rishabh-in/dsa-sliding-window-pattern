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

## 1) Maximum Sum Subarray of Size K

**Task:** Given `arr` and `k`, return the max sum of any contiguous subarray of size `k`.

**Brute force (O(n\*k))**

```ts
function maxSumSizeK_bruteforce(arr: number[], k: number): number {
  let ans = -Infinity;
  for (let i = 0; i + k <= arr.length; i++) {
    let s = 0;
    for (let j = i; j < i + k; j++) s += arr[j];
    ans = Math.max(ans, s);
  }
  return ans;
}
```

**Optimized (O(n), sliding window)**

```ts
function maxSumSizeK_optimized(arr: number[], k: number): number {
  let windowSum = 0, ans = -Infinity;
  for (let i = 0; i < arr.length; i++) {
    windowSum += arr[i];
    if (i >= k) windowSum -= arr[i - k];
    if (i >= k - 1) ans = Math.max(ans, windowSum);
  }
  return ans;
}
```

---

## 2) Minimum Sum Subarray of Size K

**Task:** Min sum of any window of size `k`.

**Brute force**

```ts
function minSumSizeK_bruteforce(arr: number[], k: number): number {
  let ans = Infinity;
  for (let i = 0; i + k <= arr.length; i++) {
    let s = 0;
    for (let j = i; j < i + k; j++) s += arr[j];
    ans = Math.min(ans, s);
  }
  return ans;
}
```

**Optimized**

```ts
function minSumSizeK_optimized(arr: number[], k: number): number {
  let windowSum = 0, ans = Infinity;
  for (let i = 0; i < arr.length; i++) {
    windowSum += arr[i];
    if (i >= k) windowSum -= arr[i - k];
    if (i >= k - 1) ans = Math.min(ans, windowSum);
  }
  return ans;
}
```

---

## 3) First Negative Number in Every Window of Size K

**Task:** For each window of size `k`, output the first negative number (or 0 if none).

**Brute force (O(n\*k))**

```ts
function firstNegatives_bruteforce(arr: number[], k: number): number[] {
  const out: number[] = [];
  for (let i = 0; i + k <= arr.length; i++) {
    let pushed = false;
    for (let j = i; j < i + k; j++) {
      if (arr[j] < 0) { out.push(arr[j]); pushed = true; break; }
    }
    if (!pushed) out.push(0);
  }
  return out;
}
```

**Optimized (O(n), queue of indices)**

```ts
function firstNegatives_optimized(arr: number[], k: number): number[] {
  const out: number[] = [];
  const dq: number[] = []; // stores indices of negatives
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] < 0) dq.push(i);
    while (dq.length && dq[0] <= i - k) dq.shift();
    if (i >= k - 1) out.push(dq.length ? arr[dq[0]] : 0);
  }
  return out;
}
```

---

## 4) Average of All Subarrays of Size K

**Task:** Return averages of every window of size `k`.

**Brute force**

```ts
function averagesSizeK_bruteforce(arr: number[], k: number): number[] {
  const out: number[] = [];
  for (let i = 0; i + k <= arr.length; i++) {
    let s = 0;
    for (let j = i; j < i + k; j++) s += arr[j];
    out.push(s / k);
  }
  return out;
}
```

**Optimized**

```ts
function averagesSizeK_optimized(arr: number[], k: number): number[] {
  const out: number[] = [];
  let s = 0;
  for (let i = 0; i < arr.length; i++) {
    s += arr[i];
    if (i >= k) s -= arr[i - k];
    if (i >= k - 1) out.push(s / k);
  }
  return out;
}
```

---

## 5) Count Occurrences of Anagrams

**Task:** Count substrings of `txt` that are anagrams of `pat`.

**Brute force (O(n \* 26))**

```ts
function countAnagrams_bruteforce(txt: string, pat: string): number {
  const need = new Array(26).fill(0);
  for (const c of pat) need[c.charCodeAt(0) - 97]++;
  const check = (l: number, r: number): boolean => {
    const have = new Array(26).fill(0);
    for (let i = l; i <= r; i++) have[txt.charCodeAt(i) - 97]++;
    for (let i = 0; i < 26; i++) if (have[i] !== need[i]) return false;
    return true;
  };
  let cnt = 0;
  for (let i = 0; i + pat.length - 1 < txt.length; i++) {
    if (check(i, i + pat.length - 1)) cnt++;
  }
  return cnt;
}
```

**Optimized (fixed window + freq match)**

```ts
function countAnagrams_optimized(txt: string, pat: string): number {
  const k = pat.length;
  const need = new Map<string, number>();
  const have = new Map<string, number>();
  for (const c of pat) need.set(c, (need.get(c) || 0) + 1);
  let matches = 0;
  const kinds = need.size;
  const add = (m: Map<string, number>, c: string, d: 1 | -1) => {
    const v = (m.get(c) || 0) + d;
    if (v === 0) m.delete(c); else m.set(c, v);
  };

  // initialize first k-1 chars
  for (let i = 0; i < k - 1 && i < txt.length; i++) {
    add(have, txt[i], 1);
  }

  let ans = 0;
  for (let r = k - 1; r < txt.length; r++) {
    add(have, txt[r], 1);
    // compare maps in O(needed chars) by tracking balance difference:
    let ok = true;
    for (const [c, cnt] of need) {
      if ((have.get(c) || 0) !== cnt) { ok = false; break; }
    }
    if (ok) ans++;
    const l = r - k + 1;
    add(have, txt[l], -1);
  }
  return ans;
}
```

---

## 6) Longest Substring with K Distinct Characters

**Task:** Length of the longest substring with at most `k` distinct chars.

**Brute force (O(n²))**

```ts
function longestKDistinct_bruteforce(s: string, k: number): number {
  let ans = 0;
  for (let i = 0; i < s.length; i++) {
    const cnt = new Map<string, number>();
    for (let j = i; j < s.length; j++) {
      cnt.set(s[j], (cnt.get(s[j]) || 0) + 1);
      if (cnt.size <= k) ans = Math.max(ans, j - i + 1);
      else break;
    }
  }
  return ans;
}
```

**Optimized (O(n))**

```ts
function longestKDistinct_optimized(s: string, k: number): number {
  let l = 0, ans = 0;
  const cnt = new Map<string, number>();
  for (let r = 0; r < s.length; r++) {
    cnt.set(s[r], (cnt.get(s[r]) || 0) + 1);
    while (cnt.size > k) {
      const c = s[l];
      cnt.set(c, (cnt.get(c) || 0) - 1);
      if (cnt.get(c) === 0) cnt.delete(c);
      l++;
    }
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 7) Longest Substring Without Repeating Characters

**Task:** Max length with all unique chars.

**Brute force**

```ts
function lengthOfLongestSubstring_bruteforce(s: string): number {
  let ans = 0;
  for (let i = 0; i < s.length; i++) {
    const seen = new Set<string>();
    for (let j = i; j < s.length; j++) {
      if (seen.has(s[j])) break;
      seen.add(s[j]);
      ans = Math.max(ans, j - i + 1);
    }
  }
  return ans;
}
```

**Optimized (O(n))**

```ts
function lengthOfLongestSubstring_optimized(s: string): number {
  let l = 0, ans = 0;
  const last = new Map<string, number>();
  for (let r = 0; r < s.length; r++) {
    const c = s[r];
    if (last.has(c) && last.get(c)! >= l) l = last.get(c)! + 1;
    last.set(c, r);
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 8) Minimum Window Substring

**Task:** Smallest substring of `s` containing all chars of `t`.

**Brute force (O(n² \* Σ))**

```ts
function minWindow_bruteforce(s: string, t: string): string {
  const need = new Map<string, number>();
  for (const c of t) need.set(c, (need.get(c) || 0) + 1);

  const covers = (l: number, r: number): boolean => {
    const have = new Map<string, number>();
    for (let i = l; i <= r; i++) have.set(s[i], (have.get(s[i]) || 0) + 1);
    for (const [c, v] of need) if ((have.get(c) || 0) < v) return false;
    return true;
  };

  let best = "";
  for (let i = 0; i < s.length; i++) {
    for (let j = i; j < s.length; j++) {
      if (covers(i, j)) {
        const cand = s.slice(i, j + 1);
        if (!best || cand.length < best.length) best = cand;
        break;
      }
    }
  }
  return best;
}
```

**Optimized (O(n))**

```ts
function minWindow_optimized(s: string, t: string): string {
  if (!t.length) return "";
  const need = new Map<string, number>();
  for (const c of t) need.set(c, (need.get(c) || 0) + 1);
  let required = need.size;

  const have = new Map<string, number>();
  let formed = 0, l = 0;
  let bestLen = Infinity, bestL = 0;

  for (let r = 0; r < s.length; r++) {
    const c = s[r];
    have.set(c, (have.get(c) || 0) + 1);
    if (need.has(c) && have.get(c) === need.get(c)) formed++;

    while (formed === required) {
      if (r - l + 1 < bestLen) { bestLen = r - l + 1; bestL = l; }
      const d = s[l];
      have.set(d, (have.get(d) || 0) - 1);
      if (need.has(d) && (have.get(d) || 0) < need.get(d)!) formed--;
      l++;
    }
  }
  return bestLen === Infinity ? "" : s.slice(bestL, bestL + bestLen);
}
```

---

## 9) Longest Substring with At Most Two Distinct Characters

**Brute force**

```ts
function longestAtMostTwo_bruteforce(s: string): number {
  let ans = 0;
  for (let i = 0; i < s.length; i++) {
    const cnt = new Map<string, number>();
    for (let j = i; j < s.length; j++) {
      cnt.set(s[j], (cnt.get(s[j]) || 0) + 1);
      if (cnt.size > 2) break;
      ans = Math.max(ans, j - i + 1);
    }
  }
  return ans;
}
```

**Optimized**

```ts
function longestAtMostTwo_optimized(s: string): number {
  let l = 0, ans = 0;
  const cnt = new Map<string, number>();
  for (let r = 0; r < s.length; r++) {
    cnt.set(s[r], (cnt.get(s[r]) || 0) + 1);
    while (cnt.size > 2) {
      const c = s[l];
      cnt.set(c, (cnt.get(c)! - 1));
      if (cnt.get(c) === 0) cnt.delete(c);
      l++;
    }
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 10) Fruit Into Baskets (LC 904)

**Brute force**

```ts
function totalFruit_bruteforce(fruits: number[]): number {
  let ans = 0;
  for (let i = 0; i < fruits.length; i++) {
    const cnt = new Map<number, number>();
    for (let j = i; j < fruits.length; j++) {
      cnt.set(fruits[j], (cnt.get(fruits[j]) || 0) + 1);
      if (cnt.size > 2) break;
      ans = Math.max(ans, j - i + 1);
    }
  }
  return ans;
}
```

**Optimized**

```ts
function totalFruit_optimized(fruits: number[]): number {
  let l = 0, ans = 0;
  const cnt = new Map<number, number>();
  for (let r = 0; r < fruits.length; r++) {
    cnt.set(fruits[r], (cnt.get(fruits[r]) || 0) + 1);
    while (cnt.size > 2) {
      const x = fruits[l];
      cnt.set(x, (cnt.get(x)! - 1));
      if (cnt.get(x) === 0) cnt.delete(x);
      l++;
    }
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 11) Permutation in String (LC 567)

**Brute force (check each window)**

```ts
function checkInclusion_bruteforce(p: string, s: string): boolean {
  const k = p.length;
  const need = new Array(26).fill(0);
  for (const c of p) need[c.charCodeAt(0) - 97]++;
  const same = (l: number): boolean => {
    const have = new Array(26).fill(0);
    for (let i = l; i < l + k; i++) have[s.charCodeAt(i) - 97]++;
    for (let i = 0; i < 26; i++) if (need[i] !== have[i]) return false;
    return true;
  };
  for (let i = 0; i + k <= s.length; i++) if (same(i)) return true;
  return false;
}
```

**Optimized (fixed window)**

```ts
function checkInclusion_optimized(p: string, s: string): boolean {
  const k = p.length;
  if (k > s.length) return false;
  const need = new Array(26).fill(0), have = new Array(26).fill(0);
  for (const c of p) need[c.charCodeAt(0) - 97]++;
  for (let i = 0; i < k; i++) have[s.charCodeAt(i) - 97]++;
  const eq = () => need.every((v, i) => v === have[i]);
  if (eq()) return true;
  for (let i = k; i < s.length; i++) {
    have[s.charCodeAt(i) - 97]++;
    have[s.charCodeAt(i - k) - 97]--;
    if (eq()) return true;
  }
  return false;
}
```

---

## 12) Longest Repeating Character Replacement (LC 424)

**Brute force**

```ts
function characterReplacement_bruteforce(s: string, k: number): number {
  let ans = 0;
  for (let i = 0; i < s.length; i++) {
    const cnt = new Array(26).fill(0);
    let maxFreq = 0;
    for (let j = i; j < s.length; j++) {
      const idx = s.charCodeAt(j) - 65;
      cnt[idx]++; maxFreq = Math.max(maxFreq, cnt[idx]);
      const len = j - i + 1;
      if (len - maxFreq <= k) ans = Math.max(ans, len);
    }
  }
  return ans;
}
```

**Optimized (track max freq in window)**

```ts
function characterReplacement_optimized(s: string, k: number): number {
  let l = 0, ans = 0, maxFreq = 0;
  const cnt = new Array(26).fill(0);
  for (let r = 0; r < s.length; r++) {
    const idx = s.charCodeAt(r) - 65;
    cnt[idx]++; maxFreq = Math.max(maxFreq, cnt[idx]);
    while (r - l + 1 - maxFreq > k) {
      cnt[s.charCodeAt(l) - 65]--;
      l++;
    }
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 13) Binary Subarray with Sum (LC 930)

**Brute force**

```ts
function numSubarraysWithSum_bruteforce(nums: number[], goal: number): number {
  let ans = 0;
  for (let i = 0; i < nums.length; i++) {
    let s = 0;
    for (let j = i; j < nums.length; j++) {
      s += nums[j];
      if (s === goal) ans++;
    }
  }
  return ans;
}
```

**Optimized (prefix counts)**

```ts
function numSubarraysWithSum_optimized(nums: number[], goal: number): number {
  const freq = new Map<number, number>();
  freq.set(0, 1);
  let sum = 0, ans = 0;
  for (const x of nums) {
    sum += x;
    ans += (freq.get(sum - goal) || 0);
    freq.set(sum, (freq.get(sum) || 0) + 1);
  }
  return ans;
}
```

---

## 14) Max Consecutive Ones III (LC 1004)

**Brute force**

```ts
function longestOnes_bruteforce(nums: number[], k: number): number {
  let ans = 0;
  for (let i = 0; i < nums.length; i++) {
    let zero = 0;
    for (let j = i; j < nums.length; j++) {
      if (nums[j] === 0) zero++;
      if (zero > k) break;
      ans = Math.max(ans, j - i + 1);
    }
  }
  return ans;
}
```

**Optimized**

```ts
function longestOnes_optimized(nums: number[], k: number): number {
  let l = 0, zeros = 0, ans = 0;
  for (let r = 0; r < nums.length; r++) {
    if (nums[r] === 0) zeros++;
    while (zeros > k) {
      if (nums[l] === 0) zeros--;
      l++;
    }
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 15) Sliding Window Maximum (LC 239)

**Task:** For window size `k`, return max of each window.

**Brute force**

```ts
function maxSlidingWindow_bruteforce(nums: number[], k: number): number[] {
  const out: number[] = [];
  for (let i = 0; i + k <= nums.length; i++) {
    let m = -Infinity;
    for (let j = i; j < i + k; j++) m = Math.max(m, nums[j]);
    out.push(m);
  }
  return out;
}
```

**Optimized (monotonic deque, O(n))**

```ts
function maxSlidingWindow_optimized(nums: number[], k: number): number[] {
  const out: number[] = [];
  const dq: number[] = []; // stores indices, values decreasing
  for (let i = 0; i < nums.length; i++) {
    while (dq.length && dq[0] <= i - k) dq.shift();
    while (dq.length && nums[dq[dq.length - 1]] <= nums[i]) dq.pop();
    dq.push(i);
    if (i >= k - 1) out.push(nums[dq[0]]);
  }
  return out;
}
```

---

## 16) Longest Subarray with Sum at Most K (non-negative nums)

**Brute force**

```ts
function longestAtMostKSum_bruteforce(nums: number[], K: number): number {
  let ans = 0;
  for (let i = 0; i < nums.length; i++) {
    let s = 0;
    for (let j = i; j < nums.length; j++) {
      s += nums[j];
      if (s <= K) ans = Math.max(ans, j - i + 1);
      else break;
    }
  }
  return ans;
}
```

**Optimized (two pointers, non-negative constraint)**

```ts
function longestAtMostKSum_optimized(nums: number[], K: number): number {
  let l = 0, s = 0, ans = 0;
  for (let r = 0; r < nums.length; r++) {
    s += nums[r];
    while (s > K) {
      s -= nums[l++];
    }
    ans = Math.max(ans, r - l + 1);
  }
  return ans;
}
```

---

## 17) Subarrays with K Different Integers (LC 992)

**Brute force**

```ts
function subarraysWithKDistinct_bruteforce(nums: number[], K: number): number {
  let ans = 0;
  for (let i = 0; i < nums.length; i++) {
    const cnt = new Map<number, number>();
    for (let j = i; j < nums.length; j++) {
      cnt.set(nums[j], (cnt.get(nums[j]) || 0) + 1);
      if (cnt.size === K) ans++;
      else if (cnt.size > K) break;
    }
  }
  return ans;
}
```

**Optimized (atMost(K) − atMost(K−1))**

```ts
function subarraysWithKDistinct_optimized(nums: number[], K: number): number {
  const atMost = (k: number): number => {
    const cnt = new Map<number, number>();
    let l = 0, res = 0;
    for (let r = 0; r < nums.length; r++) {
      cnt.set(nums[r], (cnt.get(nums[r]) || 0) + 1);
      while (cnt.size > k) {
        const x = nums[l];
        cnt.set(x, (cnt.get(x)! - 1));
        if (cnt.get(x) === 0) cnt.delete(x);
        l++;
      }
      res += (r - l + 1);
    }
    return res;
  };
  return atMost(K) - atMost(K - 1);
}
```

---

## 18) Shortest Subarray with Sum ≥ K (LC 862)

**Brute force (O(n²))**

```ts
function shortestSubarray_bruteforce(nums: number[], K: number): number {
  const n = nums.length;
  let best = Infinity;
  for (let i = 0; i < n; i++) {
    let s = 0;
    for (let j = i; j < n; j++) {
      s += nums[j];
      if (s >= K) { best = Math.min(best, j - i + 1); break; }
    }
  }
  return best === Infinity ? -1 : best;
}
```

**Optimized (prefix + monotonic deque, O(n))**

```ts
function shortestSubarray_optimized(nums: number[], K: number): number {
  const n = nums.length;
  const prefix = new Array(n + 1).fill(0);
  for (let i = 0; i < n; i++) prefix[i + 1] = prefix[i] + nums[i];

  const dq: number[] = [];
  let ans = Infinity;

  for (let i = 0; i <= n; i++) {
    while (dq.length && prefix[i] - prefix[dq[0]] >= K) {
      ans = Math.min(ans, i - dq.shift()!);
    }
    while (dq.length && prefix[i] <= prefix[dq[dq.length - 1]]) {
      dq.pop();
    }
    dq.push(i);
  }
  return ans === Infinity ? -1 : ans;
}
```

---

## 19) Maximize the Confusion of an Exam (LC 2024)

**Brute force**

```ts
function maxConsecutiveAnswers_bruteforce(answerKey: string, k: number): number {
  const solve = (ch: string): number => {
    let ans = 0;
    for (let i = 0; i < answerKey.length; i++) {
      let flips = 0;
      for (let j = i; j < answerKey.length; j++) {
        if (answerKey[j] !== ch) flips++;
        if (flips > k) break;
        ans = Math.max(ans, j - i + 1);
      }
    }
    return ans;
  };
  return Math.max(solve('T'), solve('F'));
}
```

**Optimized (two passes, fix target char)**

```ts
function maxConsecutiveAnswers_optimized(answerKey: string, k: number): number {
  const best = (target: string): number => {
    let l = 0, flips = 0, ans = 0;
    for (let r = 0; r < answerKey.length; r++) {
      if (answerKey[r] !== target) flips++;
      while (flips > k) {
        if (answerKey[l] !== target) flips--;
        l++;
      }
      ans = Math.max(ans, r - l + 1);
    }
    return ans;
  };
  return Math.max(best('T'), best('F'));
}
```

---

## 20) Longest Subarray with Equal Number of 0 and 1

**Task:** Given a binary array, longest subarray with equal 0 and 1.

**Brute force**

```ts
function findMaxLength_bruteforce(nums: number[]): number {
  let ans = 0;
  for (let i = 0; i < nums.length; i++) {
    let zeros = 0, ones = 0;
    for (let j = i; j < nums.length; j++) {
      if (nums[j] === 0) zeros++; else ones++;
      if (zeros === ones) ans = Math.max(ans, j - i + 1);
    }
  }
  return ans;
}
```

**Optimized (prefix transform 0→−1, first index map)**

```ts
function findMaxLength_optimized(nums: number[]): number {
  const first = new Map<number, number>();
  let sum = 0, ans = 0;
  first.set(0, -1);
  for (let i = 0; i < nums.length; i++) {
    sum += (nums[i] === 0 ? -1 : 1);
    if (first.has(sum)) ans = Math.max(ans, i - first.get(sum)!);
    else first.set(sum, i);
  }
  return ans;
}
```

---

# Bonus: A couple more common variants (same pattern)

## A) Minimum Size Subarray Sum (sum ≥ target, shortest length)

* Brute force: O(n²) try all starts.
* Optimized: two-pointer window that shrinks when sum ≥ target.

```ts
function minSubArrayLen_optimized(target: number, nums: number[]): number {
  let l = 0, s = 0, ans = Infinity;
  for (let r = 0; r < nums.length; r++) {
    s += nums[r];
    while (s >= target) {
      ans = Math.min(ans, r - l + 1);
      s -= nums[l++];
    }
  }
  return ans === Infinity ? 0 : ans;
}
```

## B) Number of Substrings Containing All Three Characters (a,b,c)

* Optimized: count substrings ending at r that are valid once all three exist.

```ts
function numberOfSubstrings_abc(s: string): number {
  const last = { a: -1, b: -1, c: -1 } as Record<string, number>;
  let ans = 0;
  for (let i = 0; i < s.length; i++) {
    last[s[i]] = i;
    const minLast = Math.min(last.a, last.b, last.c);
    if (minLast !== -1) ans += minLast + 1;
  }
  return ans;
}
```

---

## How to use this set effectively

1. **Start with fixed windows** (#1–#4) to master O(1) add/remove.
2. **Move to variable windows** (#5–#14) to manage counts/constraints.
3. **Finish with deque/prefix hybrids** (#15–#20) for advanced scenarios.

If you want, I can bundle this into a single `.md` file with a table of contents and push-ready section anchors.
