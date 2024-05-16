Conceptually speaking, sorting algorithms make comparisons between two elements in a collection, and decide whether or not to swap them. The choice for each pair of elements can be represented through a binary tree, with every possible configuration of the collection ending up as a leaf at the bottom of the tree (the intermediate nodes are unimportant, but you can consider them as sub-collections of the original collection, partially ordered). Only one of these leaves is the correct configuration, so a sorting algorithm needs to, in the worst case, traverse this tree in such a way that it lands on this configuration.
In the best case, you might write some initial checkers to look through the collection and see how much of it is already sorted, then work from there. However, in the worst case, the lower bound on sorting depends upon this tree.

The height of this tree is dictated by how wide it is on the bottom. Since there are $n!$ possible ways of ordering a collection, that's how many leaves there are. This forces the height of the tree to be $\log_2(n!)$. This is our lower bound on sorting
# Stirling's Approximation
$n! \approx \sqrt{2\pi n}(\frac{n}{e})^n$
What a wild equation
But if we take the $\log$ of this, then we end up with $$\log(\sqrt{2\pi n}(\frac{n}{e})^n)$$
Using our log rules, we can turn this into $$\log(\sqrt{2\pi n})+\log((\frac{n}{e})^n)$$
Big-O wise, we can ignore the first term and focus on the second: $$n\log(\frac{n}{e})$$
$$n(\log(n)-\log(e))$$
$$\approx n(\log(n)-1)$$
$$\approx n\log(n)$$
Which is what we're used to. This is why, in the worst case, you'll never be able to construct an algorithm that runs faster than $n\log(n)$.

#### Links
[[Analysis Techniques|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#analysis_techniques 