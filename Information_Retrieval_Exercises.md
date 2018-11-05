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

As we can see there is no document containing all three query terms. If we would have checked the
first posting of the third list right at the beginning, we would have noticed that there is no intersection
between the first and the third postings list. That would make any further search superfluous.

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

1. **What documents are retrieved, with the Boolean model, with the query “Computer AND NOT Components”?**

  Doc1 and Doc2

2. **Compute the idf value of the terms “Computer” and “Components” (consider we have only these 4 documents in the collection).**

3. **Compute the vector model representation of Doc4 using tf-idf weights (logarithmic tf * idf).**

4. Compute the vector model representation of the query “Computer Components” using only raw tf and no idf.

5. Compute the similarity between the query “Computer Components” and Doc4 with the cosine similarity measure.

