Document frequency is the number of documents containing a particular term. Based on Figure 1, the word cent has a document frequency of 1. Even though it appeared 3 times, it appeared 3 times in only one document.

The word all on the other hand, has a document frequency of 5. Even though it appeared once in every document, it appeared in 5 documents.

![table](https://kavita-ganesan.com/wp-content/uploads/document-frequency-example.png)
figure 1

## Why Document Frequency?
- Document frequency has several uses. 
- First, it can be used to eliminate unimportant words from analysis. 
- For example, you can enforce that words that appeared in at least 80% of your documents, can be removed from further analysis. 
- This doesn’t just remove common words such as the, is and are but also domain specific words that are too frequent.

- Take a clinical text corpora for example. Words like patient, years and old are likely to appear in almost all  clinical notes. 
- Since these words are so common, they will have low predictive power for downstream machine learning tasks and can thus be eliminated with the use of document frequency.

- Document frequency can also be used to assign weights to boost / scale down the strength of words based on relative importance. This is done using the inverse of the frequency, known as inverse document frequency (IDF).

- The idea here is that the higher the document frequency, the lower the IDF weight. 
- With this, less weight is assigned to frequent terms and more to infrequent ones. 
- This is commonly used in information retrieval scoring algorithms to weight terms such that the topic words get a higher score than non topic words.

![table2](https://kavita-ganesan.com/wp-content/uploads/idf-values.png)

figure 2

## Document Frequency vs. Term Frequency

- While document frequency is number of documents containing a term, term frequency is the number of occurrences of a term within a document.

- The term frequency of cent in document 1 is 3 and in documents 2, 3, 4, 5 is 0 (see Figure 1).

- Corpus level frequency is the sum of the term frequencies across all documents. I like to refer to this as overall term frequency.The overall term frequency of cent is 3

- In eliminating unimportant words for analysis, one can use overall term frequency or the document frequency. Document frequency is sometimes a better way to do it as term frequency can be misleading.

- For example, let’s say 1 out of 10,000 documents in your clinical notes dataset, contains 500 occurrences of the word leukemia2000. If you use term frequency to eliminate rare words, the counts are so high that it may never pass your threshold for elimination. The word leukemia2000 is still rare as it appears in only one document.

- In contrast, if you use document frequency and you want to enforce that words to keep should at least be used in 25% of your documents. You can easily enforce this.

- In summary, document frequency, while being a very simple concept, is extremely powerful in text mining and NLP. You can use it to eliminate rare and low information words, curate stop words and boost and scale down scores of words.
