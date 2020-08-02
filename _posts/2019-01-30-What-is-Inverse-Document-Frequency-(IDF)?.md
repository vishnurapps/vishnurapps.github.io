- **Inverse Document Frequency (IDF)** is a weight indicating how commonly a word is used. 
- The more frequent its usage across documents, the lower its score. 
- The lower the score, the less important the word becomes.

- For example, the word **the** appears in almost all English texts and would thus have a very low IDF score as it carries very little topic information. 
- In contrast, if you take the word **coffee**, while it is common, it’s not used as widely as the word the. 
- Thus, **coffee** would have a higher IDF score than the. Traditionally IDF is computed as:

![equation](https://latex.codecogs.com/gif.latex?{IDF}&space;=log\frac{N}{DF_{t}})

where N is the total number of documents in your text collection and DFt is the number of documents containing the term t and t is any word in your vocabulary. 

- IDF is typically used to boost the scores of words that are unique to a document with the hope that you surface high information words that characterize your document and suppress words that don’t carry much weight in a document.

- Let’s take an example. In a given document, if the word the appeared 10 times and its IDF weight is 0.1, its resulting score would be 1 (since 10*0.1=1). 
- Now if the word **coffee** also appeared 10 times and its IDF weight is 0.5 the resulting score would be 5. 
- When you rank the words by the resulting scores (in descending order of course!), **coffee** would appear before the, indicating that **coffee** is more important than the word **the**.

