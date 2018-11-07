# Information Retrieval Exercises

Vincent Guidoux

## Part 1: Boolean Model and Vector Space Model

A. **For a conjunctive query (AND), is processing postings lists in order of size guaranteed to be optimal? Explain why it is, or give an example where it isn’t.**

Prenons cet exemple:

```
prune = 1, 8, 9
pomme = 4,13,44,75
poire = 10,31,60, 78, 90, 92, 48.
```

Pour  `prune AND pomme` il y a beaucoup trop de comparaison, alors que `prune AND poire` il n'y en a que trois.

B. **How should the Boolean query x AND NOT y be handled? Why is naive evaluation of this query normally very expensive? Write out a postings merge algorithm that evaluates this query efficiently.**

```
Intersect(x, y)
    answer <- []
    while x != NIL and y != NIL
    do if docID(x) = docID(y)
        then 
            x <- next(x)
            y <- next(y)
        else if docID(x) < docID(y)
            ADD(answer, docID(x))
            x <- next(x)
        else 
            y <- next(x)
    while x!=NIL				//In case y is finished but not x
    do ADD(answer, docID(x))
    	x <- next(x)
	return(answer)
```

c'est du O(n + m) pour des listes p1 et p2 de taille n et m. On est loin du O(n * m) d'une approche naive

C. **What is the idf of a term that occurs in every document? Compare this with the use of stop word lists.**

log(1) = 0

0, comme ce mot apparait tout le temps, il est considéré comme un _stopword_

D. **Consider the following collection of four documents:**

- Doc1: Shared Computer Resources

- Doc2: Computer Services

- Doc3: Digital Shared Components

- Doc4: Computer Resources Shared Components

  **Assuming each word is a term:**

1. What documents are retrieved, with the Boolean model, with the query “Computer AND NOT Components”?

  ```
  Doc1 and Doc2
  ```

2. Compute the idf value of the terms “Computer” and “Components” (consider we have only these 4 documents in the collection).

   | terms      | formulus    | calculus   | idf value |
   | ---------- | ----------- | ---------- | --------- |
   | cumputer   | Log10(N/df) | Log10(4/3) | 0.12      |
   | Components | Log10(N/df) | Log10(4/2) | 0.3       |
   | resources  | Log10(N/df) | Log10(4/2) | 0.3       |
   | shared     | Log10(N/df) | Log10(4/3) | 0.12      |

   "Components" est plus spécifique dans une recherche que "computer", car il est moins fréquent

3. Compute the vector model representation of Doc4 using tf-idf weights (logarithmic tf * idf).

   | terms      | calculus                    | result |
   | ---------- | --------------------------- | ------ |
   | Computer   | (1 + Log10(1)) * Log10(4/3) | 0.12   |
   | Resources  | ( 1+ Log10(1)) * Log10(4/2) | 0.3    |
   | Shared     | (1 + Log10(1)) * Log10(4/3) | 0.12   |
   | Components | (1 + Log10(1)) * Log10(4/2) | 0.3    |
   | Services   |                             | 0      |
   | Digital    |                             | 0      |

   Voc =<Computer, Resources, Shared, Components, Services, Digital>

   Doc4 = (0.1249387366, 0.3, 0.12, 0.3, 0, 0)

   "Ressources" est globalement moins présent que "computer", du coup il définit mieux les documents dans lequel il se trouve que "Computer"

4. Compute the vector model representation of the query “Computer Components” using only raw tf and no idf.

   ```
   Voc =<Computer, Resources, Shared, Components, Services, Digital>
   Q = <1, 0, 0, 1, 0, 0>
   ```

5. Compute the similarity between the query “Computer Components” and Doc4 with the cosine similarity measure.

   ```
   |Doc4| = sqrt( (2* 0.3)^2 + (2 * 0.12)^2 )
   ```

## Part 2: Evaluation in Information Retrieval

F. **Consider an information need for which there are 6 relevant documents in the collection. Contrast two systems run on this collection. Their top 10 results are judged for relevance as follows (the leftmost item is the top ranked search result):**

| System1 | NNNNR RRRRN |
| ------- | ----------- |
| System2 | NRRNR RNNNN |

1. What is the Precision, Recall, and F-Measure of each system for the top 10 documents? Comment on your results.

   |          | Precision  | Recall   | F-Measure                       |
   | -------- | ---------- | -------- | ------------------------------- |
   | System 1 | 5/10 = 0.5 | 5/6 = 0. | 5/8                             |
   | System 2 | 2/5        | 2/3      | ((4/5)*(2/3))/((2/5)+2/3))= 1/2 |

2. What is the MAP of each system? Which has a higher MAP?

   Map of system 1 ( 1/5 + 2/6 + 3/7 + 4/8 + 5/9) / 6 = 0.33

   Map of system 2 ( 1/2 + 2/3 + 3/5 + 4/6) / 6 = 0.4

3. Does the result in point b intuitively make sense? What does it say about what is important in getting a good MAP score?

4. What is the R-precision of each system? (Does it rank the systems the
   same as MAP?)

   R-Precision of system 1: 2/6

   R-Precision of system 2: 4/6


G. **The following list of R’s and N’s represents relevant (R) and nonrelevant (N) returned documents in a ranked list of 20 documents retrieved in response to a query from a collection of 10,000 documents. The top of the ranked list (the document the system thinks is most likely to be relevant) is on the left of the list. This list shows 6 relevant documents. Assume that there are 8 relevant documents in total in the collection.**

| RRNNN NNNRN RNNNR NNNNR |
| :---------------------: |
|                         |

1. What is the precision of the system on the top 20?

   precision 6/20 = 0.3

2. What is the F-Measure on the top 20?

   recall = 6/8 = 0.75 

   => F = 2R*P/(R+P)

   F=2 \* 0.75 \*0.3/(0.3+0.75) = 0.43

3. What is the uninterpolated precision of the system at 25% recall?

   ```
   (Relevant docs retreived / all relevant docs) = 0.25
   x/8 = 0.25
   x = 2
   The precision on the second retreived relevant doc is 2/2 = 1
   ```

4. What is the interpolated precision at 33% recall?

   ```
   Interpolated precision at recall 0.33 is the maximum value of the precision for all points having recall >= 0.33
   
   (R,P) = {(0)
   ```

5. Assume that these 20 documents are the complete result set of the system. What is the MAP for the query?

   MAP = (1 + 1 + 0.33 + 0.36 + 0.33 +0.3) / 8 = 0.415

6. What is the largest possible MAP that this system could have?

   21 an 22 

   MAP =  (1 + 1 + 0.33 + 0.36 + 0.33 +0.3 +0.3+0.36)/8 = 0.503

7. What is the smallest possible MAP that this system could have?

   (1 + 1 +0.33 + 0.36 + 0.33 + 0.3 + 0.0007 + 0.0008)/8 = 

8. In a set of experiments, only the top 20 results are evaluated by hand. The result in (e) is used to approximate the range (f) to (g). For this example, how large (in absolute terms) can the error for the MAP be by calculating (e) instead of (f) and (g) for this query?