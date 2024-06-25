For certain problems, we can create an algorithm that is dramatically faster than we might normally expect. We can do this by making our algorithm **greedy**, meaning it behaves the same given only the next input, with no regard for what may happen later on in the algorithm.

The majority of this explanation is adapted from the course website, which at the time of writing can be found [here.](https://eecscourses.westpoint.edu/courses/cs385/greedy.html)
# Definition
A greedy algorithm is characterized by a **heuristic** and a tweaking argument.
## The Tweaking Argument
We can use something called a **tweaking argument** to prove that a greedy algorithm arrives at the most optimal solution for a given problem. Without specifying a tweaking argument, we can't guarantee that the solution a greedy algorithm arrives at is the _most_ correct.

![[Greedient Descent]]
>_This type of problem will stump a greedy algorithm_

Greedy algorithms always go towards the local minimum that is "closest" to the start point. The exact definition of this "closeness" and "correctness" depend on the context of the problem.
### Composing a Tweaking Argument
Assume an arbitrary algorithm, which gives some answer to the problem, and assume that it gives the correct answer every time (greedy or otherwise). The exact details of this algorithm don't need to be specified. You need to show that using your proposed greedy algorithm would perform the same _or better_ than this proposed solution. If the correctness or speed gets worse, then the greedy algorithm is suboptimal.
# Links
[[Design Techniques|Unit Home]]
[[CS385 - Algorithms|Course Home]]

[Course Website](https://eecscourses.westpoint.edu/courses/cs385/greedy.html)
## Tags
#design_techniques 