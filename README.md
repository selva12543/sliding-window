# 1. Best Time to Buy and Sell Stock

## Problem Statement

You are given an array `prices` where `prices[i]` is the stock price on day `i`.

You may buy one stock and sell one stock exactly once.

Return the maximum profit possible.

If no profit can be made, return `0`.

### Example

```text
prices = [7,1,5,3,6,4]

Buy at 1
Sell at 6

Profit = 5
```

---

# What Are They Actually Asking?

This is not really a stock problem.

The interviewer is asking:

> Find the maximum difference where the smaller number appears before the larger number.

Because:

```text
Buy must happen before sell.
```

You cannot:

```text
Sell at 7
Buy at 1
```

Time moves forward.

---

# Visual Understanding

```text
Day:    0  1  2  3  4  5
Price: [7, 1, 5, 3, 6, 4]
```

At Day 4:

```text
Price = 6

Best buying price before this day = 1

Profit = 6 - 1 = 5
```

We want the maximum profit possible.

---

# What Pattern Does This Teach?

This problem teaches the **Running Minimum** pattern.

Keep track of:

- The smallest value seen so far
- The best answer seen so far

Formula:

```text
Profit = Current Price - Minimum Price Seen Before
```

This pattern appears in many interview questions involving:

- Maximum profit
- Maximum difference
- Best gain
- Prefix minimums
- One-pass optimization

---

# Brute Force Solution

## Idea

Try every possible buy day with every possible sell day.

### Java Solution

```java
public int maxProfit(int[] prices) {

    int maxProfit = 0;

    for (int i = 0; i < prices.length; i++) {

        for (int j = i + 1; j < prices.length; j++) {

            int profit = prices[j] - prices[i];

            maxProfit = Math.max(maxProfit, profit);
        }
    }

    return maxProfit;
}
```

---

## Dry Run

```text
[7,1,5,3,6,4]
```

Check every pair:

```text
7 -> 1 = -6
7 -> 5 = -2
7 -> 3 = -4
7 -> 6 = -1
7 -> 4 = -3

1 -> 5 = 4
1 -> 3 = 2
1 -> 6 = 5   <-- Best
1 -> 4 = 3

...
```

Answer:

```text
5
```

---

## Complexity Analysis

### Time Complexity

```text
O(n²)
```

Reason:

```text
Nested loops
```

### Space Complexity

```text
O(1)
```

Reason:

```text
Only a few variables used
```

---

# Optimal Solution

## Key Observation

For every day:

```text
Profit = Current Price - Minimum Price Seen Before
```

We only need:

1. Lowest price seen so far
2. Highest profit seen so far

---

## Algorithm

Initialize:

```text
minPrice = Infinity
maxProfit = 0
```

For every price:

1. Update minimum price
2. Calculate profit
3. Update maximum profit

---

## Java Solution

```java
public int maxProfit(int[] prices) {

    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;

    for (int price : prices) {

        minPrice = Math.min(minPrice, price);

        int profit = price - minPrice;

        maxProfit = Math.max(maxProfit, profit);
    }

    return maxProfit;
}
```

---

# Dry Run

```text
prices = [7,1,5,3,6,4]
```

---

### Day 1

```text
price = 7

minPrice = 7

profit = 0

maxProfit = 0
```

---

### Day 2

```text
price = 1

minPrice = 1

profit = 0

maxProfit = 0
```

---

### Day 3

```text
price = 5

profit = 5 - 1 = 4

maxProfit = 4
```

---

### Day 4

```text
price = 3

profit = 3 - 1 = 2

maxProfit = 4
```

---

### Day 5

```text
price = 6

profit = 6 - 1 = 5

maxProfit = 5
```

---

### Day 6

```text
price = 4

profit = 4 - 1 = 3

maxProfit = 5
```

Final Answer:

```text
5
```

---

# Why Does This Work?

At every position:

```text
Current Price = Selling Price
Minimum Price Seen So Far = Best Buying Price
```

Therefore:

```text
Profit = Sell Price - Buy Price
```

We continuously update the maximum profit.

---

# Interview Pattern Recognition

Look for keywords like:

```text
Maximum Profit
Maximum Difference
Largest Gain
Best Return
Buy Before Sell
```

Think:

```text
Running Minimum
```

Ask yourself:

> Can I keep track of the smallest value seen so far?

If yes, an O(n) solution is usually possible.

---

# Common Interview Traps

## Trap 1: No Profit Possible

Input:

```text
[7,6,4,3,1]
```

Many candidates return:

```text
-1
```

Wrong.

You are not required to make a trade.

Correct Answer:

```text
0
```

---

## Trap 2: Sorting

Interviewer asks:

> Why not sort the array?

Example:

```text
[7,1,5,3,6,4]
```

Sorted:

```text
[1,3,4,5,6,7]
```

Problem:

```text
Sorting destroys the day order.
```

Buying must happen before selling.

---

## Trap 3: Duplicate Values

Input:

```text
[2,2,2,2]
```

Profit:

```text
0
```

The algorithm still works correctly.

---

# How Interviewers Extend This Problem

## Version 1

One transaction

```text
Buy once
Sell once
```

Current problem.

---

## Version 2

Unlimited transactions

```text
Buy Sell Buy Sell ...
```

Requires a different greedy approach.

---

## Version 3

At Most Two Transactions

```text
Buy Sell Buy Sell
```

Usually solved using Dynamic Programming.

---

## Version 4

At Most K Transactions

More advanced DP problem.

---

## Version 5

Cooldown Days

Cannot buy immediately after selling.

Another DP variation.

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Brute Force | O(n²) | O(1) |
| Optimal | O(n) | O(1) |

---

# Key Takeaway

This problem is actually teaching:

```text
While scanning left to right:

Keep track of the best value seen so far
Use it to optimize future decisions
```

Pattern:

```text
Running Minimum
+
One Pass Traversal
+
Greedy Update
```

Once you understand this idea, many array interview problems become much easier to recognize and solve.



# 2. Longest Repeating Character Replacement (LeetCode 424)

## Problem Statement

Given a string `s` consisting of uppercase English letters and an integer `k`, you can replace at most `k` characters in the string.

Return the length of the longest substring containing the same character after performing at most `k` replacements.

### Example

```java
Input:
s = "AABABBA"
k = 1

Output:
4
```

Explanation:

Replace one `B` with `A`.

```text
AABA -> AAAA
```

The longest valid substring length is `4`.

---

# Why This Problem Is Important

This is one of the most frequently asked Sliding Window interview problems.

It teaches:

* Sliding Window Technique
* Frequency Counting
* Window Validation
* Optimization from O(n²) to O(n)
* Two Pointer Approach

Many interview problems are variations of this pattern.

Examples:

* Max Consecutive Ones III
* Longest Substring with At Most K Distinct Characters
* Fruits Into Baskets
* Longest Substring Without Repeating Characters
* Minimum Window Substring

---

# Key Insight

For any window:

```text
Replacements Needed =
Window Length - Frequency of Most Common Character
```

Example:

```text
Window = AABAB

Frequency:
A = 3
B = 2

Window Length = 5
Max Frequency = 3

Replacements Needed = 5 - 3 = 2
```

If:

```text
Window Length - Max Frequency <= k
```

the window is valid.

Otherwise, shrink the window.

---

# How To Identify This Problem In Interviews

Look for keywords such as:

* Longest substring
* Maximum length
* At most k replacements
* At most k changes
* At most k modifications
* Flip at most k values
* Replace characters

These keywords usually indicate a Sliding Window problem.

---

# Common Interview Variations

### Variation 1

Given a string and k replacements, find the longest substring consisting of the same character.

### Variation 2

You may change at most k characters. Find the longest sequence of identical characters.

### Variation 3

Flip at most k zeros in a binary array. Find the longest sequence of ones.

### Variation 4

Modify at most k elements in an array to make all elements equal.

All of these use the same Sliding Window pattern.

---

# Brute Force Solution

## Approach

Generate every possible substring.

For each substring:

1. Count character frequencies.
2. Find maximum frequency.
3. Calculate replacements needed.
4. Check if valid.
5. Update answer.

## Code

```java
public int characterReplacement(String s, int k) {

    int n = s.length();
    int answer = 0;

    for (int i = 0; i < n; i++) {

        int[] freq = new int[26];

        for (int j = i; j < n; j++) {

            freq[s.charAt(j) - 'A']++;

            int maxFreq = 0;

            for (int x = 0; x < 26; x++) {
                maxFreq = Math.max(maxFreq, freq[x]);
            }

            int windowLength = j - i + 1;

            if (windowLength - maxFreq <= k) {
                answer = Math.max(answer, windowLength);
            }
        }
    }

    return answer;
}
```

## Complexity

### Time Complexity

```text
O(n²)
```

Reason:

* Outer loop → O(n)
* Inner loop → O(n)
* Frequency scan → O(26) ≈ O(1)

Overall:

```text
O(n²)
```

### Space Complexity

```text
O(26) = O(1)
```

---

# Optimal Solution (Sliding Window)

## Approach

Maintain:

* Left pointer
* Right pointer
* Frequency array
* Maximum frequency inside the window

Expand the window using the right pointer.

If:

```text
Window Length - Max Frequency > k
```

shrink the window from the left.

Keep updating the maximum valid window length.

---

# Optimal Java Solution

```java
public int characterReplacement(String s, int k) {

    int[] freq = new int[26];

    int left = 0;
    int maxFreq = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {

        char currentChar = s.charAt(right);

        freq[currentChar - 'A']++;

        maxFreq = Math.max(maxFreq,
                           freq[currentChar - 'A']);

        while ((right - left + 1) - maxFreq > k) {

            freq[s.charAt(left) - 'A']--;

            left++;
        }

        maxLength = Math.max(maxLength,
                             right - left + 1);
    }

    return maxLength;
}
```

---

# Dry Run

Input:

```java
s = "AABABBA"
k = 1
```

Window expansion:

```text
A
AA
AAB
AABA
```

At:

```text
AABAB
```

Window Length = 5

Max Frequency = 3

Required Replacements:

```text
5 - 3 = 2
```

Since:

```text
2 > 1
```

window becomes invalid.

Shrink from left.

Continue until the window becomes valid again.

Final Answer:

```text
4
```

---

# Why Don't We Recalculate maxFreq While Shrinking?

We update maxFreq only while expanding:

```java
maxFreq = Math.max(maxFreq,
                   freq[currentChar - 'A']);
```

We never decrease it.

Although maxFreq may become slightly outdated after shrinking, the algorithm still works correctly.

This optimization avoids recalculating frequencies repeatedly and keeps the solution O(n).

---

# Complexity Analysis

## Time Complexity

Each character:

* Enters the window once.
* Leaves the window once.

Therefore:

```text
O(n)
```

## Space Complexity

Frequency array size:

```java
int[26]
```

Constant space.

```text
O(1)
```

---

# Interview Cheat Sheet

### Pattern Recognition

If you see:

```text
Longest Substring
+
At Most K Changes/Replacements/Flips
```

Think:

```text
Sliding Window
```

### Window Validation Formula

```text
Replacements Needed =
Window Length - Max Frequency
```

### Valid Window

```text
Window Length - Max Frequency <= k
```

### Invalid Window

```text
Window Length - Max Frequency > k
```

### Complexity

```text
Brute Force:
Time  = O(n²)
Space = O(1)

Optimal:
Time  = O(n)
Space = O(1)
```

### Core Interview Insight

The entire problem is based on one formula:

```text
Window Length - Max Frequency <= k
```

If true, expand.

If false, shrink.

---

# Permutation in String - Brute Force Approach

---

## Problem Statement

Given two strings `s1` and `s2`, return true if any permutation of `s1` is a substring of `s2`.

---

## Key Idea

We are checking:
> Does any substring of length `s1.length()` contain the same characters as `s1`?

Instead of using frequency, we:
- Generate all substrings
- Sort and compare

---

## Approach

1. Take every substring of size `k = s1.length()`
2. Sort `s1`
3. Sort substring
4. Compare

If match → return true

---

## Code

```java
import java.util.Arrays;

class Solution {

    public boolean checkInclusion(String s1, String s2) {

        int k = s1.length();

        if (k > s2.length())
            return false;

        char[] arr = s1.toCharArray();
        Arrays.sort(arr);
        String target = new String(arr);

        for (int i = 0; i <= s2.length() - k; i++) {

            String sub = s2.substring(i, i + k);

            char[] temp = sub.toCharArray();
            Arrays.sort(temp);

            if (target.equals(new String(temp)))
                return true;
        }

        return false;
    }
}


---

---

### 📄 File 2: SlidingWindow_Optimal_PermutationInString.md

```md
# Permutation in String - Sliding Window (Optimal)

---

## Problem Statement

Return true if any permutation of `s1` exists as a substring in `s2`.

---

## Key Idea

Instead of generating permutations:

> Compare character frequencies.

A permutation means:
- Same characters
- Same frequency

---

## Approach

We use:
- `need[]` → frequency of s1
- `window[]` → frequency of current window in s2

### Sliding Window Trick

Instead of rebuilding every window:
- Add new character (right side)
- Remove old character (left side)

---

## Code

```java
class Solution {

    public boolean checkInclusion(String s1, String s2) {

        int k = s1.length();

        if (k > s2.length())
            return false;

        int[] need = new int[26];
        int[] window = new int[26];

        for (char c : s1.toCharArray()) {
            need[c - 'a']++;
        }

        for (int i = 0; i < k; i++) {
            window[s2.charAt(i) - 'a']++;
        }

        if (matches(need, window))
            return true;

        for (int i = k; i < s2.length(); i++) {

            window[s2.charAt(i) - 'a']++;      // add new character
            window[s2.charAt(i - k) - 'a']--;  // remove old character

            if (matches(need, window))
                return true;
        }

        return false;
    }

    private boolean matches(int[] a, int[] b) {
        for (int i = 0; i < 26; i++) {
            if (a[i] != b[i])
                return false;
        }
        return true;
    }
}
