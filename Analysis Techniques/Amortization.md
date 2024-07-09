This term is borrowed from the financial world. If you buy a huge building, the IRS might let you spread out the cost of the purchase over multiple tax years, for tax advantages over all the years you spread them out over. Essentially, Amortization means spreading the cost.

For data structures with egregious but infrequent worst cases, we can spread out that load to other, less bad cases to make the average runtime more palatable. For certain algorithms, the path that leads to the worst case doesn't happen every time, and you can predict with some regularity the frequency with which it happens. That way, you can say the actual runtime is closer to a value better than originally intended.
# A Practical Example
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
There are wildly different results, but for most of the time it's 1 or 2 bit flips. We can "borrow" some work from the 1- and 2-bit-flip operations to lighten the load on the 3-, 4-, or 5-bit-flips. Note also that the space between each of the growing worst cases also grows. This is because the worst results happen going from a number with all 1s (say 7) to a number with all zeroes followed by a 1 (like 8). That happens less and less frequently as the numbers increase, as they happen on the powers of 2. It happens that these two quantities (the space between the worst case and the severity of the worst case) scale together, and we can use one to mitigate the other.
We can consider something called "credits," which represent a finite amount of work that we define at the start. For this example, we'll give each increment 2 credits each. A credit can flip 1 bit.
We also need a **credit invariant**, which is a way of measuring credits we have not spent, and it is a rule that never changes. For this example, we will consider each `1` to have a credit, and each `0` to not have a credit.

Consider the increment from 7 to 8, or `111` to `0001`. By the credit invariant, we know `111` has 3 credits stored up. We also have the 2 credits that were allotted to us for the transition. First, we have to spend 3 credits to flip all the `1` s to `0`, and a further credit to make the new highest bit `1`. So we've spent 4 credits to make all the changes. Now, by the credit invariant, the new `1` we just made needs a credit, so we put our fifth and final credit there. I recommend trying this for yourself to verify this works for any increment.
# Binomial Heaps
Recall [[Heaps#Binomial Heaps|Binomial Heaps]]. One of the properties of these objects is that two binomial trees of the same size can be combined, in constant time, into a tree of double the size. For the insertion, we say that in the worst case, an insert is an $O(n\log n)$ operation, but these operations are rare enough that, on the amortized scale, an insert ends up taking $O(1)$ time, since most of the inserts are constant, and the logarithmic inserts are rare.

This works for a very similar reason to the binary counter example. Since the insertion works by creating a binomial tree of size one containing the new object, for half of the possible binomial heaps this can be inserted with no issue. Consider this analogous to one bit flip from the previous example. On the other half, it would have to be combined with the size-one tree that was already there (this uses a credit, which would be "stored" in the existing tree). Now there's a size-two tree that needs to be inserted, and in half the cases that slot would also be empty. This continues until an empty slot is found for the tree. Note that, as in the bit flipping example, the worst cases happen when the size is one less than a power of two, so all the existing slots are filled. Adding one element causes all the trees to be combined and a new slot created.
# A Queue of Two Stacks
Related Homework: [[Homework 10]]
We can simulate a queue using two related stacks. The first stack stores some of the elements in order from least on the top to greatest on the bottom, and the second stores the rest of the elements in reverse order. For example:
```
Q1 = Stack(1, 2, 3)
Q2 = Stack(7, 6, 5, 4)

// Simulates Queue(1, 2, 3, 4, 5, 6, 7)
```
When both stacks have elements, an `enqueue` or `dequeue` is super easy. The problem arises when we try to `dequeue` when `Q1` is empty. Then, we have to move all the elements from `Q2` into `Q1` and try again. For cases where `Q1` is nonempty, `dequeue` is a constant operation, but an empty `Q1` makes it a linear operation.

We can create a more realistic picture of the function's runtime using amortization.
### Amortization
Let 1 credit be able to *either*:
* Push 1 element
	OR
* Pop 1 element
When we hit the situation where we need to move `Q2` into `Q1`, we need to have `2n` credits available, where `n` is the number of elements in `Q2`, since we need to pop each number out of `Q2`, then push it into `Q1`. They can also do some very minor miscellaneous things like returning a recently popped number, but nothing else major.
Let's devise our credit invariant such that every number in `Q2` is considered to have 2 credits (remember, these aren't present in the code anywhere, it's just an analysis technique). We can give each dequeue operation 1 credit to spend (in the best case, that's all it needs to dequeue an element from `Q1`). In the worst case, then it needs to move all of `Q2` over to `Q1`. Luckily, each element in `Q2` already has enough credits to move itself, so dequeue doesn't need to spend any more credits to make that happen.
For enqueue, it needs 3 credits, one to push the new number, then the 2 credits it needs to donate to that new number.
# Conclusion
It's important to remember that amortization is only an analysis technique. You won't find anywhere in the CPU or memory keeping track of how many credits are in a data structure. Amortization is a model that gives us a more realistic picture of how a data structure behaves in multiple scenarios and over time, while simple [[Big-O Notation]] analysis can only tell us about the worst case, no matter how rare that might be.
#### Links
[[Analysis Techniques|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#analysis_techniques