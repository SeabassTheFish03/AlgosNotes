# Disclaimer
Use of this solution, in any form and on any graded assignment, without proper citation constitutes an honor violation under the USMA Honor Code. Artifacts have been hidden in the code which will alert your grader that it has been copied. Or maybe they haven't. Best not to risk it though.
# Problem
This homework involves variations on a theme: For the first and second problems, you are given an array of integers, and want to find the contiguous chunk of the array that has the highest sum. For example, in the array (1,2,-4,5,6,-1,7,3,-5,0,2), the highest sum is 20, achieved by the chunk 5,6,-1,7,3. The function should return that sum. The third and fourth problems are similar but allow you to ignore one or two numbers, respectively.

Above each problem is a comment of the form "// O(?)". As you finish each problem, replace the ? with the running time of your algorithm, such as $O(n)$ or $O(n^2)$. (The problem descriptions below mostly include descriptions of what the running times _ought_ to be, but you should write the running times that your algorithms actually achieve. For example, if the problem asked for an $O(n^2)$ solution, but your algorithm was actually $O(n^3)$, then you should say $O(n^3)$.

Variation 1:
Use two nested for loops that loop over ranges of indices (not over the _elements_ of the array), where those two indices are the boundaries of the current chunk. Also use two integer variables (not counting the loop variables): one to hold the best sum found so far and one to hold the sum of the current chunk. Your running time for this version should be $O(n^2)$.

Variation 2:
Use a single for loop that loops over the elements of the array (not the _indices_). Again use two integer variables to hold the best sum found so far and sum of the current chunk. The trick is to recognize that when the current sum goes negative, those numbers can no longer contribute to future chunks because the future chunk would be better off _without_ the negative sum. Your running time for this version should be $O(n)$.

Variation 3:
Almost the same as Variation 1, except that you are allowed to ignore **one** negative number. For example, the best sum for `(3,-6,4,5,-7,1)` would be 12, achieved by ignoring the `-6` in chunk `3,-6,4,5`. This version should still use two nested for loops over ranges, but should track three integer variables instead of two. Your running time for this version should be $O(n^2)$.

Variation 4
Similar to Variation 3, except that now you can ignore **two** negative numbers.
## Solution
```
// O(n^2)
def maxSum1(arr: Array[Int]): Int =
	var best: Int = 0
	
	for start <- 0 until arr.length do
		var chunk = 0
		for finish <- start until arr.length do
			chunk += arr(finish)
			if chunk > best then best = chunk
	
	best

// O(n)
def maxSum2(arr: Array[Int]): Int =
	var best: Int = 0
	var sum: Int = 0

    for x <- arr do
	    sum = (sum + x) max 0
	    best = best max sum
    best

// O(n^2)
def maxSum3(arr: Array[Int]): Int = // like makeSum1 but can skip one value
    var best = 0
    for lo <- 0 until arr.length do
	    var sum = 0
	    var neg = 0
	    for hi <- lo until arr.length do
	        neg = arr(hi) min neg
	        sum += arr(hi)
	        best = best max (sum - neg)
    best

// O(n^2)
def maxSum4(arr: Array[Int]): Int = // like makeSum3 but can skip two values
    var best = 0
    for lo <- 0 until arr.length do
	    var sum = 0
	    var neg = 0
	    var neg2 = 0
	    for hi <- lo until arr.length do
	        if arr(hi) < neg then
		        neg2 = neg
		        neg = arr(hi)
	        else if arr(hi) < neg2 then
			    neg2 = arr(hi)

	        sum += arr(hi)
	        best = best max (sum - neg - neg2)
    best
```
# Explanation
## Variation 1
The problem is asking us to first solve the problem using two nested `for` loops. While this is not the most efficient solution, it is a useful [[Brute Force]] technique that will help us understand the nature of the problem.
The exterior loop can keep track of our starting index. Since we are looking only at contiguous chunks, we only need to keep track the the starting and ending index. The ending index is defined in the interior loop, and it moves from the starting index to the end of the array. We do this because there is no reason to look backwards from the start. If, for example, we have `start` at index 1, there would be no reason to put `finish` at index 0, because that would be equivalent to having `start` at 0 and `finish` at 1. By the time we've reached this spot in the array, this chunk would have already been checked, so this saves us a lot of work and potential for error.
Note that we put `finish` on the exact spot of `start` for the first iteration of the loop. This is so we don't miss the value `start` is pointing to, because we decided to do this *inclusively*. The choice to do something inclusively vs. exclusively can be made by you, just make sure it's consistent throughout the algorithm.

The final runtime of this algorithm is $O(n^2)$. The exterior loop will execute $n$ times, once for each index. The interior loop executes a decreasing number of times, starting at $n$ and working down to $0$ times, after which the exterior loop exits. In total this means the interior of the loop will run $\frac{n^2}{2}$ times, which in [[Big-O Notation]] is considered equivalent to $O(n^2)$.
### Common Mistakes
Note that we are keeping track of the chunk's sum throughout the course of the program. A naive approach would be to use a method like `Array.slice(start, end)` and summing that. This summation, however, is itself an $O(n)$ operation, which would increase our overall runtime to $O(n^3)$. Rather than doing that, we can remember the sum of the previous iteration of the loop, and simply add the next number to it.
## Variation 2
By making some educated assumptions about our problem, we can simplify the solution for [[#Variation 1]] and use only 1 loop. walking across the array only once. As stated in the problem, we can do this by recognizing that any sum less than 0 is detrimental to the total, meaning we can throw it out. Notice how the problem doesn't care about where in the array the chunk is found, only how much it adds up to. This saves us some overhead because we can just "forget" about where we got the number.
You might be unfamiliar with the use of `max` as you see it in this problem. This is just shorthand for the already defined function `max(x, y)`. Scala defines it as an [Infix Operator](https://docs.scala-lang.org/scala3/reference/changed-features/operators.html) making `x max y` and equivalent statement to `max(x, y)` or `x.max(y)`.
On the interior of the loop, we are checking to see if adding `x` takes the sum below 0. If it does, just throw it all away and start from 0 for the next iteration. Then, we check to see if this new sum beats the `best` we had already stored. If it does, then store the new sum as the `best` and continue the process.

The final runtime of this algorithm is $O(n)$. Though we are doing a little bit more work inside the loop, each operation is still considered to contribute $O(1)$ (constant time), meaning the loop is the only thing adding to the [[Big-O Notation|Big-O]].
### Common Mistakes
Check out the line that says `sum = (sum + x) max 0`. You might be tempted instead to write this as `sum = sum + (x max 0)`. The first checks the new sum against 0 and takes the maximum of that. The second checks `x` alone against 0, and takes the max of that. Semantically, if `sum + x` is the max, then this consists of adding the next element in the array to the currently focused chunk. If 0 is the max, then it means you are throwing the chunk away in favor of starting again. The incorrect solution would be to max `x` with 0, which could constitute skipping a value if `x` is negative.
## Variation 3
As the problem states, this is just like [[#Variation 1]] except you can skip one value. This looks very similar to the code for Variation 1, but we are also keeping track of the most negative value in our chunk (notice how it gets reset to 0 every time the outer loop restarts). We're then checking to see if skipping over `neg` (by subtracting it from the sum) yields a better value than the previously stored `best`. Notice too that we're using `min` as an [Infix Operator](https://docs.scala-lang.org/scala3/reference/changed-features/operators.html).

Though we are doing a little bit more work than in Variation 1 inside the loops, these are all constant time operations. As such, the final runtime is the same as Variation 1: $O(n^2)$.
## Variation 4
Cranking up the heat a bit, we can now skip *two* negative numbers. This code looks very similar to [[#Variation 3]], except we have to add a little more overhead to keep track of two negative numbers. We decide at the start that we want `neg` to be the most negative number, and `neg2` to be the second-most negative. So when a new number is introduced, we first check to see if it is less than `neg`, which would make it the new most negative number, pushing `neg`'s value into `neg2` and discarding the value previously stored in `neg2`.
Perhaps the new value is greater than `neg`, but less than `neg2`. In that case, it simply takes the place of `neg2` without affecting `neg`. Since both `neg` and `neg2` are negative we know, if any value is going to be better than `best`, it will be `(sum - neg - neg2)`, so that's what we compare `best` with.

The final runtime for this algorithm is $O(n^2)$ because, even though we are adding more overhead to each loop, these are all constant time operations, which do not affect the [[Big-O Notation|Big-O]] runtime.
# Other Stuff
#### Links
[[Homework Solutions|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#homeworks 