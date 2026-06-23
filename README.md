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
