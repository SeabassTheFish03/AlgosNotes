# Graphs
A graph consists of two sets. The first set has the "nodes" or "vertices", and the second set contains the "edges," which connect two nodes to one another.
## Edge Properties
* In a directed graph, edges have **direction** (an edge goes $A \rightarrow B$, but not necessarily $B \rightarrow A$)
* Weight (one edge $A \rightarrow B$ might have a "weight" of $3$, and $B \rightarrow C$ has weight $4$. The total distance of $A \rightarrow B \rightarrow C$ is $3 + 4 = 7$)
## Expressing Graphs in Code
### Method 1: Adjacency List
Create an array, where each index of the array represents a node of the graph. Within each entry, you would have a linked list containing all the nodes a given node is adjacent to. In the worst case, and adjacency list could take up $2n^2$ space, but that would require that every node is connected to every other one. Often, this implementation is not as intensive. Lookups are harder, as you would need to step through every value in the list, but generally this is preferred to the [[#Method 2 Adjacency Matrix|Adjacency Matrix]], and certainly beats the [[#Method 3: Edge List|Edge List]].
#### Example:
```
Array(
	List(1, 3),
	List(0, 2),
	List(1),
	List(0)
)
```
Would represent
![[Example Graph]]
### Method 2: Adjacency Matrix
Similarly, we can make an array of arrays, and have true values represent an edge that exists, and false values represent edges that don't exist. 
An edge list always takes up $n^2$ space, since every theoretical edge has some representation, but lookups are much faster. These perform better in dense graphs (graphs with a lot of edges), but are generally not the most optimal option.
#### Example
We can make an adjacency matrix for the same graph above.
```
Array(
	Array(0, 1, 0, 1),
	Array(1, 0, 1, 0),
	Array(0, 1, 0, 0),
	Array(1, 0, 0, 0)
)
```
### Method 3: Edge List
This is the hardest format to work with, but the easiest to encode from one of the other forms. It's most useful for sending a representation of a graph somewhere else. We encode each edge as its own element in a list.
We can create an edge list representing the graph in the previous examples. For each edge `(a, b)` we also have to encode `(b, a)`, since the graph is undirected.
```
List(
	(0, 1),
	(0, 3),
	(1, 0),
	(1, 2),
	(2, 1),
	(3, 0)
)
```
# Other Stuff
#### Links
[[Classic Data Structures|Unit Home]]
[[CS385 - Algorithms|Course Home]]
#### Tags
#classic_data_structs 