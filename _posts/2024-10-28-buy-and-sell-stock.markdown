---
layout: post
title:  "best time to buy and sell a stock (clojure)"
date:   2024-10-26
categories: clojure
---

## problem

[original on leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/){:target="_blank"}

we are given a list `prices`, which contains the prices of a stock on a given day, the goal is to find the maximum profit possible by choosing 1 day to buy and a subsequent day to sell the stock.

## examples

| input | output | description |
| --- | --- | --- |
| `'[7,1,5,3,6,4]` | 5 | if we buy on day 2 (price = 1) and sell on day 5 (price = 6) then we make a profit of 5. |
| `'[7,6,4,3,1]` | 0 | since the prices decrease day by day there is no way to buy the stock and sell it at a higher price. |

## intuition

the best time to buy the stock is at the lowest price and the best time to sell the stock is the highest price seen AFTER the lowest price. so we can simply process the prices sequentially, keeping track of the lowest price we've seen and using that to calculate if the current price gives us the highest profit so far.

## approach

functional programs are naturally suited to a recursive approach which works well for this problem. our function will fit the simple formula below.

#### input

| name | description | initial value |
| --- | --- | --- |
| prices | list of prices by day | provided |
| minPrice | the minimum price seen so far | Infinity |
| maxProfit | the max profit seen so far | 0 |

#### base case

1. if `prices` is empty, return `maxProfit`.
> this covers edge cases where the profit is zero, including an empty list, list of 1, and the second example above.

#### recursive case
1. remove the first element in `prices` and call it `currentPrice`.
2. find the minimum between `currentPrice` and `minPrice`.
3. calculate `currentProfit` as `currentPrice - minPrice`
4. find the maximum between the `currentProfit` and `maxProfit`.
5. call recursively with the updated values.

## code

```clojure
(defn max-profit
  ([prices] (max-profit prices ##Inf 0))                    ; driver function to set proper initial values
  ([prices minPrice maxProfit]                              ; recursive function
   (let [currentPrice (peek prices)]                        ; look at first element in prices
     (if (nil? currentPrice)                                ; return maxProfit if empty/first is nil
       maxProfit
       (let [minPrice (min currentPrice minPrice)
             currentProfit (- currentPrice minPrice)]
         (max-profit
           (pop prices)                                     ; removes first from prices, returns new list
           minPrice
           (max currentProfit maxProfit)))))))
```