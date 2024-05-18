# Definition
A Union-Find data structure is represented as simply a node which knows its parent, and how many children it has (including itself). The more interesting properties emerge in the implementation.
```
class UF:
	private var parent: UF = null
	private var count: Int = 1
	def find: UF =
		if parent == null then this
		else
			val root = parent.find
			parent = root
			root

	def union(other: UF): Unit =
		val root1 = this.find
		val root2 = other.find

		if root1 == root2 then return
		if root1.count > root2.count then 
			root1.count += root2.count
			root2.parent = root1
		else
			root2.count += root1.count
			root1.parent = root2
	def size: Int = find.count
```
# Properties
Conceptually, you can visualize a Union-Find as a collection of trees, except instead of the parent pointing to its children, as you would normally expect, the children all point to the parent. The children know who their parent is, but the parent doesn't exactly know which children point to it, only how many there are. Note that there is no requirement for the tree to be binary.
![[Union-Find Data Structure]]
# Complexity
In the absolute worst case, We could have a very tall tree, with each level containing only one element, and essentially create a worse version of the Singly-Linked List. Therefore, we want our trees as wide as possible. Ideally, with every child just one level below the parent and pointing to it. The only way to guarantee this is by cleaning the tree up as more trees get union-ed in. Take a look at the implementation for `find`.
```
def find: UF =
	if parent == null then this
	else
		val root = parent.find
		parent = root
		root
```
Sure, it gives back the root of the tree, but notice how it also changes the node's own parent to be the root of the tree.
#### Links
[[Classic Data Structures|Unit Home]]
[[CS385 - Algorithms|Course Home]]

[[Homework 11]]
#### Tags
#classic_data_structs 