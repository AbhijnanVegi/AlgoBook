# How does Find work ?

One of the most commonly used shortcut is Ctrl + F, which opens a find menu in most of the application to let you search for particular text. This algorithms used for such an operation are called string search algorithms. We will now explore the Boyer-Moore string-search algorithm.

## Boyer-Moore string-search algorithm

The Boyer-Moore string-search algoritm is an efficient string-search algorithm and has been the standard benchmark for the practical string search literature. 

We will start by defining a few terms

- **T** denotes the input text to be searched. Its length is **n**
- ***P*** denotes the string to be searched for, called the *pattern*. Its length is **m**.
- **S[i]** denotes the character at index *i* of string *S*, counting from 1.
- **S[i..j]** denotes the sub of string *S* starting at index *i* and ending at *j*, inclusive.
- An alignment of P to T is an index *k* in T such that the last character of P is aligned with index k of T.
- A match or occurrence of P occurs at an alignment k if P is equivalent to T[(k-m+1)..k].

The Boyer-Moore algorithm searches for occurrences of P in T by performing explicit character comparisions at different alignments. The algorithm uses the imformation gained by preprocession P to skip as many alignments as possible.

The usual way for string search is examine each character for the first character of the patter and match the subsequent characters. Thus every character in the input text T needs to be checked. The idea in the Byer-Moore string search algorithm is that if the end of the pattern is compared to the text, then jumps along the text can be made rather than checking every character of the text.

The pattern P is aligned with the start of text string and then compares the character of a pattern from right to left, beginning with the righmost character. If a character is compared that is not within the pattern, no match can be found by analyzing any further aspects at this position so the pattern can be changed entirely past the mismatching character.

For deciding the possible shifts, the algorithm uses two preprocession strategies simultaneously. Whenever a mismatch occurs the algorithm calculates a variation using both approaches and selects the more significant shift.

1. **Bad characyer heuristics**
   The hueristics has two implications

   1. Suppose there is a character ina text in which does not occue in patetrn at all. When a mismatch happens at this character, called a bad character, the whole pattern can be changed, begin matchin from substring next to this bad character.
   2. If the bad character is a part of the pattern, align the pattern with the bad character in text.
      <img src="https://static.javatpoint.com/tutorial/daa/images/boyer-moore-algorithm.png" alt="The Boyer-Moore Algorithm" style="zoom:67%;" />

   In some cases, Bad character Heuristics can produce negative shifts. Thus we need extra information to produce a shift on encountering a bad character.

2. **Good suffix heuristics**
   A good suffix is a suffix that has matched successfully. After a mismatch which has a negative shift in bad character heuristics, look if a substring of pattern matched till bad character has a good suffix in it, if it is so then we have an onward jump equal to the length of suffix found.
   ![The Boyer-Moore Algorithm](https://static.javatpoint.com/tutorial/daa/images/boyer-moore-algorithm4.png)

The pseudocode is given by

```bash
COMPUTE-LAST-OCCURRENCE-FUNCTION (P, m, ∑ )
	for each character a ∈ ∑
		do λ [a] = 0
	for j ← 1 to m
		do λ [P [j]] ← j
	Return λ
	
COMPUTE-GOOD-SUFFIX-FUNCTION (P, m)
	Π ← COMPUTE-PREFIX-FUNCTION (P)
	P'← reverse (P)
	Π'← COMPUTE-PREFIX-FUNCTION (P')
	for j ← 0 to m
		do ɣ [j] ← m - Π [m]
	for l ← 1 to m
		do j ← m - Π' [L]
	If ɣ [j] > l - Π' [L]
		then ɣ [j] ← 1 - Π'[L]
	Return ɣ
	
BOYER-MOORE-MATCHER (T, P, ∑)
	n ←length [T]
 	m ←length [P]
 	λ← COMPUTE-LAST-OCCURRENCE-FUNCTION (P, m, ∑ )
 	ɣ← COMPUTE-GOOD-SUFFIX-FUNCTION (P, m)
 	s ←0
 	While s ≤ n - m
 		do j ← m
 	While j > 0 and P [j] = T [s + j]
 		do j ←j-1
 	If j = 0
 		then print "Pattern occurs at shift" s
 		s ← s + ɣ[0]
 	else s ← s + max (ɣ [j], j -  λ[T[s+j]])
```

