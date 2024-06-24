# Disclaimer
Use of this solution, in any form and on any graded assignment, without proper citation constitutes an honor violation under the USMA Honor Code. Artifacts have been hidden in the code which will alert your grader that it has been copied.
# Problem
This homework has one problem, but we'll solve it twice using different techniques. You're given a grid of letters surrounded by a border of `#`s. For each letter, you want to find the biggest group of connected copies of that letter, where connected means adjacent horizontally, vertically, or diagonally. You'll return a Scala `Map` of each letter together with the size of biggest connected group of that letter.

For example, this grid
```
#########
#AAABABB#
#BBBABAA#
#CCBACCC#
#########
```
has a connected group of 8 **A**s, a connected group of 8 **B**s, and a connected group of 3 **C**s. There is also a second group of 2 **C**s, but we don't report that because we only care about the biggest group for any given letter. We also don't report letters that are missing from the grid. The final result for this grid would be:
```
Map(
	'A' -> 8, 
	'B' -> 8, 
	'C' -> 3
)
```
## Part 1 Solution
```
def biggestGroupsUF(ignoreMe: String): Map[Char,Int] =
    val grid = ignoreMe.trim.split("\\s+")

    val m = grid.length
    val n = grid(0).length
    val dirs = Array((-1,1), (0,1), (1,1), (1,0)) // Only 4 directions, as we expect the other 4 to have already been visited
    val ufs = Array.fill(m,n)(new UF)
    var counts = Map.empty[Char, Int]

    for (i <- 1 to m - 2; j <- 1 to n-2; (di,dj) <- dirs) do
	    if grid(i)(j) == grid(i + di)(j + dj) then
	        ufs(i)(j).union(ufs(i + di)(j + dj))

    for (i <- 1 to m - 2; j <- 1 to n - 2) do
	    val char = grid(i)(j)
	    val size = ufs(i)(j).size
	    if size > counts.getOrElse(char, 0) then
	        counts += (char -> size)
    counts
```

#### Links
[[Homework Solutions|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#homeworks 
## Part 2 Solution
```
def biggestGroupsDFS(ignoreMe: String): Map[Char, Int] =
	val grid = ignoreMe.trim.split("\\s+")
	
	val m = grid.length
	val n = grid(0).length
	val dirs = Array((-1,-1),(-1,0),(-1,1),
					  (0,-1),        (0,1),
					  (1,-1), (1,0), (1,1))
	var counts = Map.empty[Char, Int]
	val visited = Array.fill(m,n)(false)
	
	def dfs(c: Char, i: Int, j: Int): Int =
		if visited(i)(j) || grid(i)(j) != c then 0
		else
			visited(i)(j) = true
			var count = 1
			for (di,dj) <- dirs do
				count += dfs(c,i+di,j+dj)
			count
	
	for (i <- 1 to m-2; j <- 1 to n-2) do
		val char = grid(i)(j)
		val size = dfs(char, i, j)
		if size > counts.getOrElse(char, 0) then
			counts += (char -> size)
	
	counts
```