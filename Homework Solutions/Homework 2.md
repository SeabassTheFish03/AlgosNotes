# Disclaimer
Use of this solution, in any form and on any graded assignment, without proper citation constitutes an honor violation under the USMA Honor Code. Artifacts have been hidden in the code which will alert your grader that it has been copied. Or maybe they haven't. Best not to risk it though.
# Problem 1
You are given an array of individual items for sale, together with a budget. Each item is represented by a pair of (_cost_, _value_). The array is sorted both by cost and by value (meaning that the cost at index `i` is $\leq$ the cost at index `i+1` and the value at index `i` is $\leq$ the value at index `i+1`). Your goal is to purchase two items with the greatest possible combined value, without the combined cost going over your budget. Your function will return the combined value of the purchased pair.

Clarifications:

- You cannot purchase the same item twice. (But you could purchase two items with identical costs and values as long as they were distinct entries in the array.)
- You must purchase two items, even if you could achieve a higher value by buying a single expensive item.
- If it is not possible to purchase two items, return -1.
## Solution
```
def maxCombinedValue(arr: Array[(Int, Int)], budget: Int): Int =
    var lo = 0
    var hi = arr.length - 1
    var best = -1

    while lo < hi do
	    val combCost = arr(lo)._1 + arr(hi)._1
	    val combValue = arr(lo)._2 + arr(hi)._2
      
	    if combCost <= budget then
	        best = best max combValue
	        lo += 1
	    else
	        hi -= 1

    best
```
# Problem 2
You are preparing goody bags for the children attending your niece's Lego-themed birthday party. Naturally, you will include a number of Lego bricks in each goody bag. However, based on several memorable temper tantrums during last year's M&M-themed party, you are quite determined that
- every child must receive the same number of bricks, and
- all the bricks received by a single child must be the same color (but it's okay if, say, one child receives blue bricks and another child receives purple bricks).

Given a list containing the number of bricks you have in each available color, and the number of kids, use **binary search** to determine the maximum number of bricks you can put in each goody bag.

For example, with 11 red bricks, 9 blue bricks, and 5 green bricks, you could give 5 children a maximum of 4 bricks each. You could not give each child 5 bricks without at least one child receiving bricks of different colors.

Hint: The instructions say that you must use binary search, but bricksPerColor is not sorted so you can't use binary search on it. (It's also a list rather than an array, which would cause efficiency problems for binary search.)

Instead, write a helper function groups that takes a number `b` and returns the number of groups you can make, each containing `b` bricks of the same color. For example, with 11 red bricks, 9 blue bricks, and 5 green bricks, groups(3) would return 7. Then do your binary search based on this helper function. In other words, treat the function _as if it were an array_, even though it's not.

For describing the Big-O running time in this problem, it is not clear what $N$ would mean. Instead of $N$, your running time should be written in terms of $C$ (the number of different colors), $K$ (the number of kids), and/or $T$ (the total number of bricks). For example, a running time might look like $O(K^2logC + T)$. If you need a letter for some property not mentioned here, feel free to make one up—just include a comment explaining this alternative property.
## Solution
```
def bricksPerKid(numKids: Int, bricksPerColor: List[Int]): Int =
    def groups(n: Int): Int =
	    bricksPerColor.foldRight(0)((a, b) => a/n + b) // O(C)

    var lo = 1
    var hi = bricksPerColor.maxOption.getOrElse(0) // O(C)
    var mid = 1
    var best = 0

    // O(logT*C)
    while lo < hi do
	    mid = (lo + hi) / 2
	    val possibleGroups = groups(mid)

	    if possibleGroups < numKids then // Too many bricks
			hi = mid
	    else if possibleGroups > numKids then // Not enough bricks, but still possible
		    best = mid
		    lo = mid + 1
	    else
	        best = mid
	        lo = hi + 1

    // O(logT)
    while groups(best + 1) == numKids do best += 1
  
    best
```
# Explanation
## Problem 1
This algorithm works by starting from the outside of the array and working in. Since we are given the items in sorted order, both by cost and by value, we can use that property to be a little more [[Greedy Algorithms|greedy]] as we search for the solution.
Rather than checking every single combination of items in the array, we can start by picking the most expensive (and most valuable) item, and seeing how costly we can make the other item before breaking out of the budget. The budget check is key, as that is what is keeping us from just exploring every combination. Once the budget is broken, we already know that every item with a greater cost than the current `lo` will also break the budget, so we don't even have to bother searching them. So, on average, this won't take as long as searching every combo.
However, when doing [[Big-O Notation|Big-O]] analysis, we care about the worst case, and that would be if the best combination is found right in the middle of the array. In that situation, we do have to end up searching every combination. For that reason, the runtime of this algorithm is considered $O(n^2)$.
## Problem 2
This problem requires a slight paradigm shift to make the [[Binary Search]] work. It's not initially apparent how
# Other Stuff
#### Links
[[Homework Solutions|Unit Home]]
[[CS385 - Algorithms|Course Home]]

Knapsack Problem
#### Tags
#homeworks 