# Overview
Big-O Notation is a method of analyzing how long an algorithm would take to run in the worst case. It's less about exactly how long the algorithm would take in a single given case, more of how that run time would scale with respect to the size of the input.
# The Notation Itself
If you encounter Big-O, it generally looks like this: $O(n^2)$. The $O()$ simply means it's referring to Big-O notation (as opposed to a different notation like Big-$\Omega$ or Big-$\Theta$). The algebra inside the parenthesis is what is actually referring to the algorithm's run time. The $n$ stands for the size of the input, and whatever is modifying it represents how the algorithm's run time scales with $n$. We don't care about constant coefficients or lower-degree terms. Therefore $O(3n)$ is equivalent to $O(n)$, and $O(n^3 + n^2)$ is equivalent to $O(n^3)$.

These simplifications mean that Big-O is not a predictor of how long the algorithm will take, given any $n$; it's not meant to be. Rather, if you were to plot an algorithm's runtime based on a bunch of different input sizes, what would be the general trend, and how would you draw a line of best fit for the plot? If the line of best fit is a parabola, then it's $O(n^2)$. If it's a sloped line, it's $O(n)$
# Aliases
When speaking about Big-O, you'll often hear words like "polynomial time" or "linear time" instead of actual Big-O. These are just different names for the same concepts. Here are the most common colloquial terms:

| Algebra     | Name             |
| ----------- | ---------------- |
| $O(1)$      | Constant Time    |
| $O(\log n)$ | Logarithmic Time |
| $O(n)$      | Linear Time      |
| $O(n^2)$    | Quadratic Time   |

# Other Stuff
#### Links
[[Analysis Techniques|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#analysis_techniques 