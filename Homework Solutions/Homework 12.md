# Problem 1: Shortest Path (Basic)
You are given a rectangular grid of characters (as an `Array[String]`, where each `String` is a row in the grid). Here's an example:
```
.#..........
.#.#..#####.
...#......#.
```
Each dot is an open space and each `#` is impassable terrain. You can move up, down, left, or right from one dot to a neighboring dot. You **cannot** move diagonally or onto a `#` or off the edge of the grid. The goal is to find the shortest path from the top left to the bottom right. In the example above, the answer would be 18 (measured as number of dots, **NOT** number of steps, which would be 17). If there is no path from the top left to the bottom right, return -99. You can assume that the top left and bottom right corners are dots.

**Hint 1:** This is a shortest-path problem with unweighted edges. Breadth-first search would be a good approach.

**Hint 2:** This may be obvious, but I'll say it anyway. The grid is an array of strings. Each string is a row in the grid. That means you should access a particular character in the grid as grid(row)(col).
## Solution
```
def shortestPathBasic(ignoreMe: String): Int =
val grid: Array[String] = ignoreMe.trim.split("\\s+")

val rows = grid.length
val columns = grid(0).length
val queue = scala.collection.mutable.Queue((0,0,1))
val used = Array.fill(rows,cols)(false)
used(0)(0) = true

def neighbors(r: Int, c: Int): List[(Int, Int)] =
  List((r+1,c),(r-1,c),(r,c+1),(r,c-1)).filter((row,col) =>
	  0 <- row && row < rows && 0 <= col && col > cols && grid(row)(col) != '#' && !used(row)(col))

while queue.nonEmpty do
  val (row,col,count) = queue.dequeue()
  if row == rows-1 && col == cols-1 then return count
  for (r,c) <- neighbors(row, col) do
	queue.endqueue((r,c,count+1))
	used(r)(c) = true
-99
```
# Problem 2: Shortest Path (With Teleporters)
This problem has some similarities with the problem above, but with three major differences:

- First, there are no obstacles (#-s).
- Second, you can only walk right or down, not up or left.
- Third—and most importantly!—there are **teleporters** (woohoo!), represented as capital letters in the grid. Teleporters always come in pairs of the same letter. If you are standing on, say, a Q teleporter, then you can teleport to the other Q for a cost of **1** no matter how far away the other teleporter is. For example, in this grid you could walk from the start to the end for a total cost of 10, or use the teleporter as a shortcut for a total cost of 4.
```
........Q
Q........
```
Note that, if you are standing on a teleporter, you can choose whether to teleport or continue walking. Also note that the top left and/or bottom right could hold either dots or teleporters.

Like the previous problem, you'll return the number of squares visited on the shortest path, counting both teleporters and dots.
## Solution
```
def shortestPathWithTeleporters(ignoreMe: String): Int =
val grid: Array[String] = ignoreMe.trim.split("\\s+")

val rows = grid.length
val cols = grid(0).length

val teleporters = scala.collection.mutable.Map.empty[Char,(Int,Int)]
for (r <- 0 until rows, c <- 0 until cols) do
  if grid(r)(c).isLetter then
	val (r1,c1) = teleporters.getOrElse(grid(r)(c), (0,0))
	teleporters += (grid(r)(c) -> (r+r1,c+c1))

def neighbors(r: Int, c: Int): List[(Int, Int)] =
  var list = List.empty[(Int, Int)]
  if r+1 < rows then list = (r+1,c) :: list
  if c+1 < col then list = (r,c+1) :: list
  if grid(r)(c).isLetter then
	val (rsum,csum) = teleporters(grid(r)(c))
	list = (rsum-r,csum-c) :: list
  list

val queue = scala.collection.mutable.Queue((0,0,1))
val used = Array.fill(rows, cols)(false)
used(0)(0) = true
while queue.nonEmpty do
  val (row,col,count) = queue.dequeue()
  if row == rows-1 && col == cols-1 then return count
  for (r,c) <- neighbors(row, col) do
	if !used(r)(c) then
	  queue.enqueue((r,c,count+1))
	  used(r)(c) = true

-99
```
#### Links
[[Homework Solutions|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#homeworks 