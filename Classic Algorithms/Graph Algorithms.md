Related: [[Graphs]]
# Depth-First Search
For this searching algorithm, we use a stack to keep track of which item we have left to visit, until eventually the sought-after value is found or the entire graph is traversed. We push our initial value onto the stack, then pop one value at a time off the stack and visit it. For each node we visit, we add all the children to the stack. Due to the nature of the stack, this means the next node we visit will be the child of the current node, or, in the event that node has no children, one of its siblings.
If we find the value we're looking for, we can just break out of the `while` loop early, but for our example we will pretend we're not searching for anything, for simplicity's sake.
#### Example
Given an adjacency list and a starting node:
```
import scala.collection.mutable.Stack

def depthFirstSearch(adjacency: Arry[List[Int]], start: Int): Unit =
	var todo: Stack[Int] = Stack(start)
	var visited: Array[Boolean] = Array.fill(adjacency.length)(false)

	visited(start) = true

	while todo.nonEmpty do
		val toStudy: Int = todo.pop()

		adjacency(toStudy).foreach(newNode =>
			if !visited(newNode) then
				todo.push(newNode)
				visited(newNode) = true
		)
		print(toStudy) // Our way of saying this node was "visited"
```
# Breadth-First Search
This algorithm is very, very similar to [[#Depth-First Search]] in implementation, but wildly different in concept. To get to BFS from DFS, just change the stack and stack functions to a queue and queue functions. The "First-In, First-Out" nature of the Queue means we will end up visiting all the children of one node before we consider visiting the children of any of those nodes.
#### Example
Given an adjacency list and a starting node:
```
import scala.collection.mutable.Queue

// O(v*e)
def breadthFirstSearch(adjacency: Array[List[Int]], start: Int): Unit =
	var todo: Queue[Int] = Queue(start)
	var visited: Array[Boolean] = Array.fill(adjacency.length)(false) // O(v)

	visited(start) = true
	// O(v*e)
	while todo.nonEmpty do
		val toStudy: Int = todo.dequeue()

		// O(v)
		adjacency(toStudy).foreach(newNode =>
			if !visited(newNode) then
				todo.enqueue(newNode)
				visited(newNode) = true
		)
		print(toStudy)
```
Where `v` is the number of vertices and `e` is the number of edges.
# Minimal Spanning Trees
* [[#Kruskal's Algorithm]]
* [[#Prim's Algorithm]]
* [[#Dykstra's Algorithm]]
# Topological Sort
Topological sorting is only tangentially related to topology, so don't let that scare you. We're given a list of elements and constraints in the form of an [[#Method 3 Edge List|edge list]], and we need to create an order of these elements. For any edge `(a, b)`, our final order must have `a` coming before `b`, though we don't care about what or how many elements come between. This implies that the edges are directed, as it would be impossible to have `a` come before `b` while simultaneously having `b` come before `a`. Let's throw some arbitrary directions on the earlier example and work with it.
![[Example DiGraph]]
And this would be represented in an Edge List by
```
List(
	(0, 1),
	(1, 2),
	(3, 0)
)
```
And a valid ordering (indeed, the only one) would be `3, 0, 1, 2`
## Inferences
We can infer a few things from this.
First, we know that the first element in the order has no prerequisites. If it did, then it would have an element coming before, and would no longer be the first element. If no such element exists, we can say right out that there is no valid order.
We can assume something similar about the last element, notably that it has no post-requisites.
# Kruskal's Algorithm
* From the starting node, choose the edge leaving that node (which does not create a cycle) with the smallest weight
* Add the node on the other edge to the tree, and consider all edges connected to both nodes. Choose the one with the smallest weight and add it to the tree
* Continue until all nodes in the original graph have been added to the tree.
![[Kruskal's Algorithm]]
We can use the [[Union-Find Data Structure]] to make it very simple to avoid cycles. If two nodes have the same root, we don't union them and add them to the tree (since that would create a cycle). If they are different, then we can union them
# Prim's Algorithm
1. Choose a starting node
![[Prim's Algorithm]]

# Dykstra's Algorithm