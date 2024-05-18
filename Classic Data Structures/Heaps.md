# The Priority Queue/Heap
A **priority queue**, or **heap** is most often implemented using a balanced n-ary tree. The root of the tree is the value with the highest priority (most often, this will be the minimum or maximum value). Duplicates are allowed.
## Order Invariants
* The value with the highest priority is at the root.
* The two values with the next-highest priority will be the children of the root.
* When a value is added or removed, the tree must be rebalanced to preserve the above two properties.
## Red-Black Tree
The Red-Black Tree is a method for balancing a binary tree. It comes with further shape invariants, in addition to the existing order invariants
### Shape Invariants
* Every node in the tree is colored red or black
* No red node can have red children
* For any given pack from the root to a leaf, they all have the same number of black nodes
* Every level except (possibly) the bottom level must be completely full
* The bottom level, which may not be completely full, fills up from left to right
![[Red-Black Tree]]
# Other Stuff
#### Links
[[Classic Data Structures|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#classic_data_structs