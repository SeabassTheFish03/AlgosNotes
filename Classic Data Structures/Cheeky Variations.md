Sometimes Dr. O likes to take normal data structures and add a little twist on them. This will most often happen on exams and homeworks. I'm adding them here, so you can see what weirdness he's gotten up to in the past.
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
# The AB List
Discussion of the AB List introduced in Homework ??