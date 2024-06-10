# Disclaimer
Use of this solution, in any form and on any graded assignment, without proper citation constitutes an honor violation under the USMA Honor Code. Artifacts have been hidden in the code which will alert your grader that it has been copied. Or maybe they haven't. Best not to risk it though.
# Problem 1
For your XE401/402 capstone project, you are building a prototype wireless jammer. When activated in a wireless-enabled room like an internet cafe, it will jam packets with IP addresses in certain specified ranges but allow packets with IP addresses outside those ranges to pass freely.

Because this is a prototype device, it can only jam a maximum of `K` address ranges. If there are more than `K` specific addresses or address ranges that you would like to block, you can accomplish this by blocking ranges that are larger than the ranges actually specified. For example, if you want to block addresses 13, 24, and 50, but you can only block a maximum of 2 ranges (`K=2`), then you might block the ranges 13-24 and 50-50. Of course, this means that you might block addresses that you didn't actually intend to block, such as addresses 14-23, but this “collateral damage” is considered an acceptable cost. Still, it is desirable to minimize the number of addresses that are blocked unintentionally.

You will be given `K`, the maximum number of address ranges that can be blocked, and an array of specific addresses that you want to block. You should determine which address ranges to block so that all the specified addresses are blocked, while minimizing the number of addresses that are blocked unintentionally. You will return the number of addresses that are blocked unintentionally.

Remember that the point of this problem is to practice Transform & Conquer. Your implementation should be organized as a series of ~4 transformations of the data, followed by some trivial code to generate the final answer from the result of the last transformation. **Include a comment** at the top of each transformation briefly explaining the goal of that particular transformation.
## Solution
```
def wirelessJammer(k: Int, addrs: Array[Int]): Int =
    val sorted = addrs.sorted
    val gaps = Array.tabulate(addrs.length - 1)(i => sorted(i+1) - sorted(i) - 1)
    val sortedGaps = gaps.sorted
    sortedGaps.take(addrs.length - k).sum
```
# Problem 2
Given an array of integers, generate the sum of these integers in a Divide-and-Conquer manner, where at each recursive step you split the remaning integers in half, make a recursive call on each half, and add the two results.

Instead of simply returning the sum, you'll return a trace of all the additions in the form of a binary tree. For example, given the input (1,2,4,5,7,8), you would return the tree
```
		__27__
	   /      \
	  7        20
	 / \      /  \
	3   4   12    8
   / \     /  \
  1   2   5    7
```
The tree itself will use the BinaryTree type you should remember from CS384, defined as
```
enum BinaryTree[A]:
    case Empty
    case Node(item: A, left: BinaryTree[A], right: BinaryTree[A])
```
For example, the tree
```
   5
  / \
 1   4
```
would be represented as

```Node(5, Node(1, Empty, Empty), Node(4, Empty, Empty))```

You might wonder why you would ever do this in a Divide & Conquer fashion instead of using a simple loop. One answer is that the Divide & Conquer approach may be better able to exploit concurrency by doing the left call and the right call in parallel. Another answer is that the data itself might be too big to fit on one processor and so might be distributed across multiple machines. (All that is true, but, of course, the real answer about why we are using Divide & Conquer is so you can focus on the details of Divide & Conquer with a problem that is simple enough that the logic of the _problem_ shouldn't be a barrier.)
## Solution
```
def addEmUp(arr: Array[Int]): BinaryTree[Int] =
    def root(tree: BinaryTree[Int]): Int =
	    tree match
	        case Node(i,_,_) => i
	        case Empty =>
		        throw new NoSuchElementException(
			        "Can't find root of Empty BinaryTree"
			    )

    def make(lo: Int, hi: Int): BinaryTree[Int] =
	    if lo == hi then Node(arr(lo), Empty, Empty)
		else
	        val mid = (lo+hi)/2
	        val left = make(lo, mid)
	        val right = make(mid + 1, hi)
	        Node(root(left) + root(right), left, right)

    if arr.isEmpty then Empty
    else make(0, arr.length-1)
```
# Problem 3
You are given a list of characters, where you are guaranteed that the size of the list is a power of 2. You will permute the list so that the element at index `i` ends up at index `j`, where `j` is the number you get by reversing the bits of `i`.

For example, suppose the list is size 16. Then each index has 4 meaningful bits. If `i` is 1101 in bits (ie, 13) then `j` would be 1011 in bits (ie, 11).

There are many ways to solve this problem, but the point is to practice Divide & Conquer where you are breaking the input in half at each step. You will get no credit if you solve it a different way. (But be aware that there is more than one way to break the input in half!)

Also, notice that the input is a **list**, not an array.
## Solution
```
def flipTheBits(list: List[Char]): List[Char] =
    // O(n)
    def split(list: List[Char]): (List[Char], List[Char]) =
	    if list.isEmpty then (List.empty, List.empty)
	    else
	        val (odds, evens) = split(list.tail)
	        (list.head :: evens, odds)

    if list.tail.isEmpty then list
    else
	    val (evens, odds) = split(list)
	    // O(n)
		flipTheBits(evens) ::: flipTheBits(odds)
```
# Other Stuff
#### Links
[[Homework Solutions|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#homeworks 