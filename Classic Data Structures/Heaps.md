# The Priority Queue/Heap
A **priority queue**, or **heap** is most often implemented using a balanced n-ary tree. The root of the tree is the value with the highest priority (most often, this will be the minimum or maximum value). Duplicates are allowed.
## Order Invariants
* The value with the highest priority is at the root.
* The two values with the next-highest priority will be the children of the root.
* When a value is added or removed, the tree must be rebalanced to preserve the above two properties.
# Binomial Heaps
The **binomial heap** is a specific type of Priority Queue. They are lists of binomial trees, which always have $2^n$ nodes for all $n \in \mathbb{N}$.
### Binomial Trees
![[Binomial Heap]]
Every tree of size $2^n$ is two copies of a tree of size $2^{n-1}$, connected at the roots. As a result, these trees can end up having more than two children. No other way of constructing these trees is considered valid.
### From Trees to Heaps
When you combine these trees into a list, you can decide which trees to use, and add their sizes together to hit any other size in $\mathbb{N}$. To do so, express that size in binary with the least significant bit first, and choose the associated binomial trees. Together, you can get a binomial heap of your chosen size.

We can implement this using [case classes](https://docs.scala-lang.org/tour/case-classes.html) in Scala.
```
type BHeap = List[Tree]

case class Tree(item: Int, size: Int, children: List[Tree]):
	// Note that we can't make children a BHeap, because
	// BHeap comes with smallest trees first, and children has
	// largest trees first. Also, all possible tree sizes are
	// "present" in the list

	/**
	* Private method that links 2 trees of the same size together
	* t1 and t2 must be the same size. Since it's internal, we
	* don't bother checking
	* 
	* O(1)
	*/
	private def link(t1: Tree, t2: Tree): Tree =
		// Takes 2 trees, links them together into 1
		// t1 and t2 must be the same size
		if t1.item >= t2.item then
			new Tree(t1.item, 2*t1.size, t2 :: t1.children)
		else
			new Tree(t2.item, 2*t2.size, t1 :: t2.children)

	/** 
	* Adds an element into the BHeap. We define the private
	* method ins to make things easier. We know because of the
	* way we write insert that the param t will never be bigger
	* than h.head
	*
	* O(heap.length), where heap.length <= log2(total_size)
	**/
	def insert(x: Int, heap: BHeap): BHeap =
		def ins(t: Tree, h: BHeap): BHeap =
			if h.isEmpty then List(t)
			else if t.size < h.head.size then
				t :: h
			else // We know ==, since insert doesn't allow >
				ins(link(t, h.head), h.tail)
		ins(Tree(x, 1, Nil), heap)

	/**
	*
	*/
	def deleteMax(heap: BHeap): BHeap =
		val heapMax = heap.maxWith(_.item)
		???
```
# Skew Heap
Consider the [[#Binomial Heaps|Binomial Heap]], where the heap consists of multiple trees of very specific sizes and shapes. The trees don't have to be binary trees, but they better have a number of elements equal to a power of two. When choosing to use a binomial heap, the programmer cares a lot about finding the size of the tree in $O(\log n)$ time, among other priorities.
If, instead, you want a data structure better suited for faster merging at the cost of the rigid structural constraints of a Binomial Heap, then you might end up choosing a Skew Heap.
## Definition ([source](https://en.wikipedia.org/wiki/Skew_heap#Definition)])
You can define a Skew Heap recursively with the following rules:
* A heap with only one element is a skew heap
* The result of *skew merging* two skew heaps $sh_1$ and $sh_2$ is also a skew heap
## Skew Merging
In plain English, we merge two skew heaps $p$ and $q$ (where $p$ has the smaller root) into the new tree $r$ by putting the root of $p$ as the root of $r$, the left subtree of $p$ as the right subtree of $r$, and recursively merging the right subtree of $p$ and the full tree $q$. The behavior looks like swapping two trees back and forth as the recursion moves down the tree. A value that starts on the left side of the tree will end up on the right after a merge, and vice versa.
Here's an implementation in Scala:
```
// First, we need a way to express a Skew Heap. Nothing too crazy here
// The real changes come in the implementation
enum SkewHeap:
	case Empty
	case Node(left: SkewHeap, item: Int, right: SkewHeap)

// The behavior of merge defines how the rest of the functions behave, so we
// will create that function first
def merge(p: SkewHeap, q: SkewHeap): SkewHeap =
	(p, q) match
		case (Empty, Empty) => Empty
		case (Node(_,_,_), Empty) => p
		case (Empty, Node(_,_,_)) => q
		case (Node(pl,pi,pr), Node(_,qi,_)) =>
			if qi < pi then merge(q, p)
			else Node(merge(pr, q), pi, pl)

// Other behaviors are just merge in disguise
def insert(item: Int, p: SkewHeap): SkewHeap =
	merge(Node(Empty, item, Empty), p)

def deleteMin(p: SkewHeap): SkewHeap =
	p match
		case Empty => throw new Exception("Can't deleteMin from Empty")
		case Node(left,_,right) => merge(left, right)

/* Other Misc. Functions because they're my notes and I can do what I want */
// Look at the top element
def head(p: SkewHeap): Int =
	p match
		case Empty => throw new Exception("Nothing in the SkewHeap")
		case Node(_,i,_) => i
		
// Delete the min and also return
def pop(p: SkewHeap): (Int, SkewHeap) =
	(peek(p), deleteMin(p))
```
## Runtime
For our purposes, it's enough to say the [[Amortization|amortized]] runtime of each operation is $O(\log n)$. Specifically, the upper bound of a skew merge is $\log_\phi n$ for any $n$. Yep, that's the [Golden Ratio](https://en.wikipedia.org/wiki/Golden_ratio). The paper proving this can be found [here](https://www.win.tue.nl/~berry/papers/topskew.pdf), but that's well beyond what you need to know for this course. In raw [[Big-O Notation|big-O]], the worst case merge would be if the tree was a zig-zag, such that every recursive call would be done on a tree with only one less element than the previous call. Together, this can take $O(n^2)$. However, if you have constructed the heap using only skew merges, it's exceedingly unlikely for this to happen.
# Other Stuff
#### Links
[[Classic Data Structures|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#classic_data_structs