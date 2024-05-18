This term is borrowed from the financial world. If you buy a huge building, the IRS might let you spread out the cost of the purchase over multiple tax years, for tax advantages over all the years you spread them out over. Essentially, Amortization means spreading the cost.

For data structures with egregious but infrequent worst cases, we can spread out that load to other, less bad cases to make the average runtime more palatable. For certain algorithms, the path that leads to the worst case doesn't happen every time, and you can predict with some regularity the frequency with which it happens. That way, you can say the actual runtime is closer to a value better than originally intended.

Consider counting with binary numbers (little-endian):
* 0
* 01
* 11
* 001
* 101
* 011
* 111
* 0001
The "cost" in this example is the number of bits that get changed. We can consider the new list:
* 0 -> 1: 1
* 1 -> 2: 2
* (etc.) 1
* 3
* 1
* 2
* 1
* 4
There are wildly different results, but for most of the time it's 1 or 2 bit flips. We can "borrow" some work from the 1- and 2- bit-flip operations to lighten the load on the 3-, 4-, or 5- bit-flips.
We can consider something called "credits," which represent a finite amount of work that we define at the start. For this example, we'll give each increment 2 credits each. A credit can flip 1 bit.
We also need a **credit invariant**, which is a way of measuring credits we have not spent. For this example, we will consider each `1` to have a credit, and each `0` to not have a credit.

Consider the increment from 7 to 8, or `111` to `0001`. By the credit invariant, we know `111` has 3 credits stored up. We also have the 2 credits that were allotted to us for the transition. First, we have to spend 3 credits to flip all the `1` s to `0`, and a further credit to make the new highest bit `1`. So we've spent 4 credits to make all the changes. Now, by the credit invariant, the new `1` we just made needs a credit, so we put our fifth and final credit there. I recommend trying this for yourself to verify this works for any increment.
# Binomial Heaps
Recall [[Types of Data Structures#Binomial Heaps|Binomial Heaps]]. One of the properties of these objects is that two binomial trees of the same size can be combined, in constant time, into a tree of double the size. For the insertion, we can say that in the worst case, an insert is an $O(n\log n)$ operation, but these operations are rare enough that, on the amortized scale, an insert ends up taking $O(1)$ time, since most of the inserts are constant, and the logarithmic inserts are rare.
# A Queue of Two Stacks
We can simulate a queue using two related stacks. The first stack stores the elements in order from least on the top to greatest on the bottom, and the second stores the rest of the elements in reverse order. For example:
```
Q1 = Stack(1, 2, 3)
Q2 = Stack(7, 6, 5, 4)
```
When both stacks are have elements, then an enqueue or dequeue is super easy. The problem arises when we try to dequeue when `Q1` is empty. Then, we have to move all the elements from `Q2` into `Q1` and try again. So we have either constant or linear operations. Let's amortize this.
### Amortization
Let 1 credit be able to either:
* Push 1 element
	OR
* Pop 1 element
When we hit the situation where we need to move `Q2` into `Q1`, we need to have `2n` credits available, where `n` is the number of elements in `Q2`, since we need to pop each number out of `Q2`, then push it into `Q1`. They can also do some very minor miscellaneous things like returning a recently popped number, but nothing else major.
Let's devise our credit invariant such that every number in `Q2` is considered to have 2 credits (remember, these aren't present in the code anywhere, it's just an analysis technique). We can give each dequeue operation 1 credit to spend (in the best case, that's all it needs to dequeue an element from `Q1`). In the worst case, then it needs to move all of `Q2` over to `Q1`. Luckily, each element in `Q2` already has enough credits to move itself, so dequeue doesn't need to spend any more credits to make that happen.
For enqueue, it needs 3 credits, one to push the new number, then the 2 credits it needs to donate to that new number.
# Skew Heaps


#### Links
[[Analysis Techniques|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#analysis_techniques