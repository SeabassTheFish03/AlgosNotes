# Underlying Data Types
We have a lot of different ways we can store data in a collection, but under the hood they can be expressed using just a few collection types:
* Tree
* Array
* HashSet
## Binomial Heaps
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
# The Cthulhu List
Discussion of the List data type introduced in [[Homework 8]]
## Definition
We define a Cthulhu List (CList) using the following Scala enum:
```
enum CList[+A]:
	case Empty
	case Cons1(item: A, next: CList[(A, A)])
	case Cons2(item1: A, item2: A, next: CList[(A, A)])
```
A good example of a `CList` in practice might look like
```
Cons2(1, 2, Cons1((3, 4), Cons1(((5, 6), (7, 8)), Empty)))
```
Or, with newlines added:
```
Cons2(1, 2,
	Cons1((3, 4),
		Cons1(((5, 6), (7, 8)),
			Empty
		)
	)
)
```
If you take a look at the section on [[#Binomial Heaps]], you'll notice how we can express the heap very concisely using binary, where 0 meant the [[#Binomial Trees|Binomial Tree]] of that size was not included, and 1 meant it was. We can do something very similar, where we can encode the structure of our `CList`. Take the example above. Working from inside to out, we've buried a `Cons1` in a `Cons1`, then buried that in a `Cons2`. If we don't care about the values themselves, we can write that as `2 1 1`, where a `2` means there's a `Cons2`, and `1` means there's a `Cons1`. Even though this uses `1`s and `2`s, it is equivalent to the `0`s and `1`s of a Binomial Heap.