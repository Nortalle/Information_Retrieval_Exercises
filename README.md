# Information Retrieval Exercises

## Part 1: Boolean Model and Vector Space Model



A. **For a conjunctive query (AND), is processing postings lists in order of size guaranteed to be optimal? Explain why it is, or give an example where it isn’t.**

Processing postings list in order of size (i.e. the shortest postings list first) is usually a good approach.
But it is not optimal e. g. in a conjunctive query with three terms:

```
word 1 = 1,25,33
word 2 = 2,13,44,75
word 3 = 10,31,20,60,48.
```

As we can see there is no document containing all three query terms. If we would have checked the first posting of the third list right at the beginning, we would have noticed that there is no intersection between the first and the third postings list. That would make any further search superfluous.

B. **How should the Boolean query x AND NOT y be handled? Why is naive evaluation of this query normally very expensive? Write out a postings merge algorithm that evaluates this query efficiently.**

A naive evaluation of the query x AND (NOT y) would be to calculate (NOT y) first as a new postings list, which takes O(N) time, and the merge it with x. Therefore, the overall complexity will be O(N).

An efficient postings merge algorithm to evaluate x AND (NOT y) is:

```
MERGE1 (x, y)
1 answer <- ( )
2 while x!= NIL and y!= NIL
3 do if docID(x)=docID(y)
4 then x<- next(x)
5 y<-next(y)
6 else if docID(x)<docID(y)
7 then ADD(answer,docID(x))
8 x<- next(x)
9 else y<-next(y)
10 return(answer)
```

C. **What is the idf of a term that occurs in every document? Compare this with the use of stop word lists.**

It is 0. For a word that occurs in every document, putting it on the stop list has the same effect as idf weighting: the word is ignored.

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

   ```
   
   ```

3. Compute the vector model representation of Doc4 using tf-idf weights (logarithmic tf * idf).

   ```
   
   ```

4. Compute the vector model representation of the query “Computer Components” using only raw tf and no idf.

   ```
   
   ```

5. Compute the similarity between the query “Computer Components” and Doc4 with the cosine similarity measure.

   ```
   
   ```

E. **Compute the vector space similarity between:**

1. The query: "digital cameras" and 
2. The document: "digital cameras and video cameras" by filling out the empty columns in the table bellow.
    - Assume total number of documents in the collection N = 10,000,000
    - use logarithmic term weighting (wf columns) for query and document,
    - idf weighting for the query only, and
    - cosine normalization for the document only.
      Treat and as a stop word. Enter term counts in the tf columns. What is the final similarity score ?

## Part 2: Evaluation in Information Retrieval

F. **Consider an information need for which there are 6 relevant documents in the collection. Contrast two systems run on this collection. Their top 10 results are judged for relevance as follows (the leftmost item is the top ranked search result):**

| System1 | NNNNR RRRRN |
| ------- | ----------- |
| System2 | NRRNR RNNNN |

1. What is the Precision, Recall, and F-Measure of each system for the top 10 documents? Comment on your results.

   ```
   
   ```

2. What is the MAP of each system? Which has a higher MAP?

3. Does the result in point b intuitively make sense? What does it say about what is important in getting a good MAP score?

4. What is the R-precision of each system? (Does it rank the systems the
   same as MAP?)


G. **The following list of R’s and N’s represents relevant (R) and nonrelevant (N) returned documents in a ranked list of 20 documents retrieved in response to a query from a collection of 10,000 documents. The top of the ranked list (the document the system thinks is most likely to be relevant) is on the left of the list. This list shows 6 relevant documents. Assume that there are 8 relevant documents in total in the collection.**

| RRNNN NNNRN RNNNR NNNNR |
| :---------------------: |
|                         |

1. What is the precision of the system on the top 20?

2. What is the F-Measure on the top 20?

3. What is the uninterpolated precision of the system at 25% recall?

4. What is the interpolated precision at 33% recall?

5. Assume that these 20 documents are the complete result set of the system. What is the MAP for the query?

6. What is the largest possible MAP that this system could have?

7. What is the smallest possible MAP that this system could have?

8. In a set of experiments, only the top 20 results are evaluated by hand. The result in (e) is used to approximate the range (f) to (g). For this example, how large (in absolute terms) can the error for the MAP be by calculating (e) instead of (f) and (g) for this query?
