
# Problem 1
## Procedural
```
def wordCounter(word: String, doc: String): Int =
	// Runtime: O(m*n) where m is string.length, n is doc.length
	
	var counter = 0
	val windows: Array[String] = doc.sliding(word.length)
	
	for window <- windows do
		if window == word then counter += 1
	counter
```
## Functional
```
def wordCounter(word: String, doc: String): Int =
	// Runtime: O(m*n) where m is string.length, n is doc.length
	
	doc.sliding(word.length).count(_ == word)
```