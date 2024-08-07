Brute-force is rarely the most optimal solution for an algorithm, though it is often the simplest to imagine and implement. It's a great starting place for optimizing into a faster and more efficient algorithm.

At the time of writing, the reading and exercises for Brute Force can be found on the [[#Links|course website]]. For each of the examples and problems found there (1-6 and 1-3, respectively), I've copied the problem statements exactly, but written the solutions in Scala (on the website, they are in Ada).
# Definition
A brute-force algorithm finds a way to generate all possible solutions in the problem space, then decides if that solution is correct or not. A brute-force algorithm is characterized by two parts: a **traveler** and a **checker**.

The traveler is some method of moving through the problem space, and the only requirement is that it will eventually make its way to every possible solution to a problem.

The checker takes each possible solution offered by the traveler and checks it against some condition stipulated by the problem. If that condition is met, the algorithm stops. If not, the traveler is on to its next destination.
# Why?
If you never want to use a brute-force algorithm in an implementation, why bother writing one at all? Sometimes, it is not obvious what the correct solution would look like. A brute force solution, executed correctly, guarantees that every possible solution is checked and that the logic of the checker is correct. More efficient algorithms are, for the most part, only more optimized methods of traversing the problem space. A key example of this would be [[Binary Search]].

A key advantage of a Brute Force answer is that it will be the _most_ correct solution, since every possible solution is considered.
# Examples
## Example 1: Checking for Prime Numbers
**Problem**: Is $n>1$ a prime number?
```
def isPrime(n: Int): Boolean =
	for x <- 2 until n do
		if n % x == 0 then return false
	true
```
The **traveler** is the `for` loop. It checks every number from `2` to `n-1` (that's what the `until` keyword does) to see if it divides `x` evenly. Our **checker** actually has a chance to break out early if it finds a divisor of `x`. The chance of this happening increases as `n` increases, but if `n` is prime the loop will run to completion. 

The runtime of this example in the worst case is still $O(n)$. Even though it has a chance to break out early, the loop will run to completion if the number is prime, so the worst case scenario is that the loop runs $n$ times.
## Example 2: Brute Force Exponentiation
**Problem**: Calculate $x^n$, where $n$ is a non-negative integer.
```
def pow(x: Int, n: Int): Int =
	var a = 1
	for a <- 1 to n do
		a = a * x
	a
```
In this example, the **traveler** is the `for` loop, which executes a number of times equal to `n`. Since exponentiation is repeated multiplication, we can multiply `a` by `x` every iteration, which is equivalent to multiplying `x` with itself `n` times. In the case that `n = 0`, the range will be invalid and the loop with not run at all, returning `1` as expected.

The **checker** is a little sneakier. There are no `if` statements, but the key lesson of this example is to show how the checker can be built into the logic of the program. We know exactly when the desired outcome is reached: when the loop ends. So, the question the checker is notionally asking is "has the loop ended?" If yes, then it returns the answer; if not, it executes the loop again.

The runtime of this example is $O(n)$ (see [[Big-O Notation]]). In fact, we know the runtime is exactly `n`, not just in the worst case, but in all cases. Since the loop never has a chance to break early, the worst case scenario is the only possible scenario. A possible speedup for this would be using a technique like [[Dynamic Programming]], at the cost of using more memory.
## Example 3: Linear Search
**Problem**: Check if a given value is present in an array
```
def contains(x: Int, arr: Array[Int]): Boolean =
	for item <- arr do
		if x == item then return true
	false
```
You might notice this looks very similar to the last example. It's actually nearly the same problem. The **traveler** is array elements instead of numbers in a range, but the key similarity is the early breakout when a match is found. Our **checker** is now asking if the current `item` we're focused on is equal to the `x` we are looking for.

As with the last example, the runtime is $O(n)$
## Example 4: Double Dipping
**Problem**: An enterprising - but unethical - cadet has taken out two cow loans from separate banks (which, just in case there is any doubt, *you are not supposed to do*). Given two arrays of names representing the cadets taking out loans from each of the institutions, find the cheater.
```
def cheater(bank1: Array[String], bank2: Array[String]): Option[String] =
	for x <- bank1 do
		for y <- bank2 do
			if x == y then return Some(x)
	None // No matches found
```
*Note: I used Options here because they represent the most idealized way of handling outputs which may or may not exist. In practice, you might see "" or some other flag value to signify that there are no matches. However, it is up to the caller of this function to handle that condition. The Option type forces that handling to happen.*

We've moved from one-dimensional arrays and numerical spaces into 2-dimensional ones, but the fundamental principal of travelers and checkers remains the same. In this example, the traveler is moving through both arrays piece by piece. For each element in the first array, it is checking against every element in the second array, then progressing one element in the first array and repeating the process until both arrays are empty (they don't need to be the same size).

The checker is asking if the two elements are equal to one another. If they are, then the loops exit early and the function returns. One of the fundamental assumptions is that there is only one cheater in the mix, as the early return means any subsequent cheaters would be ignored. Technically, this makes it a [[Greedy Algorithms|greedy algorithm]]
## Example 5: Shortest Distance
**Problem**: Find the shortest distance between two points in an array of points (Assume there are at least two points, that all points are distinct, and that you have a function `distance` to calculate the distance between two points)
```
def shortestDistance(points: Array[Int]): Int =
	var shortest = Int.MaxValue
	for i <- 0 until points.length do
		for j <- i + 1 until points.length do
			shortest = shortest min distance(points[i], points[j])
	shortest
```
*Note: Scala allows min to be an infix operator. In this case, it means to take the minimum of shortest and the distance between the two focused points. This provides a readable way to write maxes and mins*

Our **traveler** is a little bit smarter than in previous examples, but only so that it avoids doing the same computation more than once. Rather than checking every point in the array against the focused point at `i`, it instead looks only the subsequent ones. A comparison between `i` and a point earlier in the array would have already been done is a previous iteration of the loop, where the previous point was `i` and the current point was `j`.

We have no **checker** in this case, other than the loop being over. That's because we can't know the shortest distance until we've checked every pair.

The runtime for this example is $O(n^2)$
## Example 6: 3 to make 100
**Problem**: Does an array of numbers contain three separate entries that sum to `100`?
```
def sum100(arr: Array[Int]): Boolean =
	for i <- 0 until arr.length do
		for j <- i + 1 until arr.length do
			for k <- j + 1 until arr.length do
				if arr[i] + arr[j] + arr[k] == 100 then return true
	false
```
Yikes. Even though we are diminishing the number of entries `j` and `k` need to look at (see [[Brute Force#Example 5 Shortest Distance|Example 5]]), there are still a lot of calculations happening. The **traveler** needs to traverse the array three nested times, which is what leads to the unfortunate structure of this example.

The runtime for this example is $O(n^3)$
# Practice Problems
## Problem 1: Word Counter
**Problem**: Given a *word* and a *document* (both as strings), count how many times the word appears in the document. Assume that the words is not empty, and that capitalization matters. For example, "cat" and "Cat" do not match. Be aware of the possibility that, for some words, multiple occurrences of that word could overlap.
## Problem 2: Sphere Interior
**Problem**: Consider points in a three-dimensional grid, that is, coordinates in the form (X, Y, Z), where X, Y, and Z are all integers. Given a *Radius*, count how many grid points lie on or inside a sphere of that radius centered at the origin.
## Problem 3: Black Square
**Problem**: Given a black-and-white image as a two-dimensional array of pixels (`0`=black, `1`=white), find the size (number of pixels) of the largest solid black square in the image.
## Problem 4: The [0-1 Knapsack Problem](https://en.wikipedia.org/wiki/Knapsack_problem#0-1_knapsack_problem)
**Problem**: You are a thief, and you are about to bring in the haul of a lifetime. The only problem: you only have enough room in your knapsack for `maxWeight` pounds worth of loot. Given an array of items with their values and weights, find the maximum value you can steal without going over the weight limit. Assume no item has exactly the same weight AND value as another (they may share one or the other, but not both).
# Links
[[Design Techniques|Unit Home]]
[[CS385 - Algorithms|Course Home]]

Examples 1 - 6 and Problems 1 - 3 were taken from [the course website](https://eecscourses.westpoint.edu/courses/cs385/bruteforce.html)
[The 0-1 Knapsack Problem](https://en.wikipedia.org/wiki/Knapsack_problem#0-1_knapsack_problem)
## Tags
#design_techniques 