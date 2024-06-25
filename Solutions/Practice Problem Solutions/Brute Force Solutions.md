# Problem 1: Word Counter
## Procedural
```
def wordCounter(word: String, doc: String): Int =
	// Runtime: O(m*n) where m is string.length, n is doc.length
	
	var counter = 0
	val windows: Array[String] = doc.sliding(word.length)
	
	for window <- windows do
		if window == word then counter += 1
	counter
```
## Functional
```
def wordCounter(word: String, doc: String): Int =
	// Runtime: O(m*n) where m is string.length, n is doc.length
	
	doc.sliding(word.length).count(_ == word)
```
# Problem 2: Sphere Interior
## Procedural
```
def sphereInterior(points: Array[(Int, Int, Int)], r: Int): Int =
	// Runtime: O(n)
	
	var count = 0
	for point <- points do
		if scala.math.sqrt(point._1^2 + point._2^2 + point._3^2) <= r then count += 1
	count
```
## Functional
```
def sphereInterior(points: Array[(Int, Int, Int)], r: Int): Int =
	// Runtime: O(n)
	
	points.count(point => scala.math.sqrt(point._1^2 + point._2^2 + point._3^2) <= r)
```
# Problem 3: Black Square
## Procedural
```
def blackSquare(grid: Array[Array[Int]]): Int =
	// Runtime: O(n^4)
	
	var size = grid.length min grid(0).length

	def solid(start: (Int, Int), sz: Int): Boolean =
		if start._1 + sz >= grid.length || start._2 + sz >= grid(0).length then false
		else
			for i <- 0 to sz do
				for j <- 0 to sz do
					if grid(start._1 + i)(start._2 + j) == 1 then return false
			true
			
	while size > 0 do
		for i <- 0 until grid.length - size do
			for j <- 0 until grid(0).length - size do
				if solid((i, j), size) then return size^2
		size -= 1
	0
```
## Functional
```
def blackSquare(grid: Array[Array[Int]]): Int =
	// Runtime: O(n^4)
	
	def solid(start: (Int, Int), sz: Int): Boolean =
		if start._1 + sz >= grid.length || start._2 + sz >= grid(0).length then false
		else
			(grid.map(_.slice(start._2, start._2 + sz))
				.slice(start._1, start._1 + sz)
				.flatten
				.sum
			) == 0

	(0 until (grid.length min grid(0).length)).foldRight((0, false))((a, b) =>
		if b._2 then b
		else
			(a,
				(0 until grid.length - a).map(i =>
					(0 until grid(0).length - a).map(j =>
						(i, j)
					)
				).foldRight(false)((x, y) => solid(x, a) || y)
			)
	)._1
```
# Problem 4: 0-1 Knapsack Problem
## Procedural
```
def knapsack(arr: Array[(Int, Int)], maxWeight: Int): Int =
	var maxValue = 0
	
	for i <- 0 until arr.length - 1 do
		for j <- i until arr.length do 
			val combWeight = arr(i)(1) + arr(j)(1)
			
			if combWeight <= maxWeight then
				maxValue = maxValue max (arr(i)(0) + arr(j)(0))
	maxValue
```