Sample text for processing

```python
sent_0 = final['Text'].values[0]
print(sent_0)
print("="*50)
```
This is the text

```
Why is this $[...] when the same product is available for $[...] here?<br />http://www.amazon.com/VICTOR-FLY-MAGNET-BAIT-REFILL/dp/B00004RBDY<br /><br />The Victor M380 and M502 traps are unreal, of course -- total fly genocide. Pretty stinky, but only right nearby.
```
## Remove urls from text

```python
# remove urls from text python: https://stackoverflow.com/a/40823105/4084039
sent_0 = re.sub(r"http\S+", "", sent_0)
print(sent_0)
```
The result is
```
Why is this $[...] when the same product is available for $[...] here?<br /> /><br />The Victor M380 and M502 traps are unreal, of course -- total fly genocide. Pretty stinky, but only right nearby.
```

## Remove HTML tags from text

```python
# https://stackoverflow.com/questions/16206380/python-beautifulsoup-how-to-remove-all-tags-from-an-element
from bs4 import BeautifulSoup

soup = BeautifulSoup(sent_0, 'lxml')
text = soup.get_text()
print(text)
```
This will give us

```
Why is this $[...] when the same product is available for $[...] here? />The Victor M380 and M502 traps are unreal, of course -- total fly genocide. Pretty stinky, but only right nearby.
```

## Remove words with numbers

```python
#remove words with numbers python: https://stackoverflow.com/a/18082370/4084039
sent_0 = re.sub("\S*\d\S*", "", sent_0).strip()
print(sent_0)
```

This removed M380 and M502

```
Why is this $[...] when the same product is available for $[...] here?<br /> /><br />The Victor  and  traps are unreal, of course -- total fly genocide. Pretty stinky, but only right nearby.
```

## Remove special characters

```python
#remove spacial character: https://stackoverflow.com/a/5843547/4084039
sent_0 = re.sub('[^A-Za-z0-9]+', ' ', sent_0)
print(sent_0)
```

This will remove special characters present in the review.
```
Why is this when the same product is available for here br br The Victor M380 and M502 traps are unreal of course total fly genocide Pretty stinky but only right nearby
```

## Decontract words

This is used to expand short form of commonly used sentences.

```python
# https://stackoverflow.com/a/47091490/4084039
import re

def decontracted(phrase):
    # specific
    phrase = re.sub(r"won't", "will not", phrase)
    phrase = re.sub(r"can\'t", "can not", phrase)

    # general
    phrase = re.sub(r"n\'t", " not", phrase)
    phrase = re.sub(r"\'re", " are", phrase)
    phrase = re.sub(r"\'s", " is", phrase)
    phrase = re.sub(r"\'d", " would", phrase)
    phrase = re.sub(r"\'ll", " will", phrase)
    phrase = re.sub(r"\'t", " not", phrase)
    phrase = re.sub(r"\'ve", " have", phrase)
    phrase = re.sub(r"\'m", " am", phrase)
    return phrase
```

## Creating Bag Of Words

CountVectorizer from sklearn is used to make BOW. We pass the reviews into the CountVectorizer during fit. On transform we gets the BOW representation.

```python
#BoW
count_vect = CountVectorizer() #in scikit-learn
count_vect.fit(preprocessed_reviews)
print("some feature names ", count_vect.get_feature_names()[:10])
print('='*50)

final_counts = count_vect.transform(preprocessed_reviews)
print("the type of count vectorizer ",type(final_counts))
print("the shape of out text BOW vectorizer ",final_counts.get_shape())
print("the number of unique words ", final_counts.get_shape()[1])
```

The BOW produced is a sparse matrix. Means it contains a lot of zeros. Here BOW was made using the count of words. If we need binary BOW we need to set the binary argument of the CountVectorizer constructor.

```
some feature names  ['aa', 'aahhhs', 'aback', 'abandon', 'abates', 'abbott', 'abby', 'abdominal', 'abiding', 'ability']
==================================================
the type of count vectorizer  <class 'scipy.sparse.csr.csr_matrix'>
the shape of out text BOW vectorizer  (4986, 12997)
the number of unique words  12997
```

## n-Grams

While making the unigram some important words were removes as stop words. To reduce the effect of that we can use n-gram representation.

```python
#removing stop words like "not" should be avoided before building n-grams
# count_vect = CountVectorizer(ngram_range=(1,2))
# please do read the CountVectorizer documentation http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html
# you can choose these numebrs min_df=10, max_features=5000, of your choice
count_vect = CountVectorizer(ngram_range=(1,2), min_df=10, max_features=5000)
final_bigram_counts = count_vect.fit_transform(preprocessed_reviews)
print("the type of count vectorizer ",type(final_bigram_counts))
print("the shape of out text BOW vectorizer ",final_bigram_counts.get_shape())
print("the number of unique words including both unigrams and bigrams ", final_bigram_counts.get_shape()[1])
```

As we have given ngram_range as (1,2) both unigram and bigram is made here. 

```
the type of count vectorizer  <class 'scipy.sparse.csr.csr_matrix'>
the shape of out text BOW vectorizer  (4986, 3144)
the number of unique words including both unigrams and bigrams  3144
```

## TF-IDF Vectorization

Here instead of count of word occurance, the matrix is made with the product of tf and idf. Here the min_df means `When building the vocabulary ignore terms that have a document frequency strictly lower than the given threshold. This value is also called cut-off in the literature.`

```python
tf_idf_vect = TfidfVectorizer(ngram_range=(1,2), min_df=10)
tf_idf_vect.fit(preprocessed_reviews)
print("some sample features(unique words in the corpus)",tf_idf_vect.get_feature_names()[0:10])
print('='*50)

final_tf_idf = tf_idf_vect.transform(preprocessed_reviews)
print("the type of count vectorizer ",type(final_tf_idf))
print("the shape of out text TFIDF vectorizer ",final_tf_idf.get_shape())
print("the number of unique words including both unigrams and bigrams ", final_tf_idf.get_shape()[1])
```

This matrix is still sparse as there are many empty cells.

```
some sample features(unique words in the corpus) ['ability', 'able', 'able find', 'able get', 'absolute', 'absolutely', 'absolutely delicious', 'absolutely love', 'absolutely no', 'according']
==================================================
the type of count vectorizer  <class 'scipy.sparse.csr.csr_matrix'>
the shape of out text TFIDF vectorizer  (4986, 3144)
the number of unique words including both unigrams and bigrams  3144
```

## Word2Vec

Here we can capture the semantic meaning of the words. We can approach this in two ways. If we have lot of data, we can create a model ourself. Else we can download existing models.

### To make the model

```python
model=Word2Vec(list_of_sentance,min_count=5,size=50, workers=4)
```
### To use existing model

```python
model=KeyedVectors.load_word2vec_format('GoogleNews-vectors-negative300.bin', binary=True)
```

Lets see the words similar to dirty.
```python
w1 = "dirty"
model.wv.most_similar (positive=w1)
```

The model gave us a list of words that are similar to dirty.

```
[('filthy', 0.871721625328064),
 ('stained', 0.7922376990318298),
 ('unclean', 0.7915753126144409),
 ('dusty', 0.7772612571716309),
 ('smelly', 0.7618112564086914),
 ('grubby', 0.7483716011047363),
 ('dingy', 0.7330487966537476),
 ('gross', 0.7239381074905396),
 ('grimy', 0.7228356599807739),
 ('disgusting', 0.7213647365570068)]
 ```
 
 Now lets try with polite.
 
 ```python
 # look up top 6 words similar to 'polite'
w1 = ["polite"]
model.wv.most_similar (positive=w1,topn=6)
```

```
[('courteous', 0.9174547791481018),
 ('friendly', 0.8309274911880493),
 ('cordial', 0.7990915179252625),
 ('professional', 0.7945970892906189),
 ('attentive', 0.7732747197151184),
 ('gracious', 0.7469891309738159)]
 ```
 
 Now lets looks at words similar to france
 

```python
# look up top 6 words similar to 'france'
w1 = ["france"]
model.wv.most_similar (positive=w1,topn=6)
```

```
[('canada', 0.6603403091430664),
 ('germany', 0.6510637998580933),
 ('spain', 0.6431018114089966),
 ('barcelona', 0.61174076795578),
 ('mexico', 0.6070996522903442),
 ('rome', 0.6065913438796997)]
 ```
 
 Lets try with  shocked
 
 ```python
 # look up top 6 words similar to 'shocked'
w1 = ["shocked"]
model.wv.most_similar (positive=w1,topn=6)
```

```
[('horrified', 0.80775386095047),
 ('amazed', 0.7797470092773438),
 ('astonished', 0.7748459577560425),
 ('dismayed', 0.7680633068084717),
 ('stunned', 0.7603034973144531),
 ('appalled', 0.7466776371002197)]
 ```
 
Lets try to get everything related to stuff on the bed

```python
w1 = ["bed",'sheet','pillow']
w2 = ['couch']
model.wv.most_similar (positive=w1,negative=w2,topn=10)
```

```
[('duvet', 0.7086508274078369),
 ('blanket', 0.7016597390174866),
 ('mattress', 0.7002605199813843),
 ('quilt', 0.6868821978569031),
 ('matress', 0.6777950525283813),
 ('pillowcase', 0.6413239240646362),
 ('sheets', 0.6382123827934265),
 ('foam', 0.6322235465049744),
 ('pillows', 0.6320573687553406),
 ('comforter', 0.5972476601600647)]
 ```
 
 ## Similarity between two words in the vocabulary
 
 ```python
 # similarity between two different words
model.wv.similarity(w1="dirty",w2="smelly")
```
```
0.76181122646029453
```

```python
# similarity between two identical words
model.wv.similarity(w1="dirty",w2="dirty")
```

```
1.0000000000000002
```

```python
# similarity between two unrelated words
model.wv.similarity(w1="dirty",w2="clean")
```

```
0.25355593501920781
```

Under the hood, the above three snippets computes the cosine similarity between the two specified words using word vectors of each. From the scores, it makes sense that dirty is highly similar to smelly but dirty is dissimilar to clean. If you do a similarity between two identical words, the score will be 1.0 as the range of the cosine similarity score will always be between [0.0-1.0

## Find the odd one out

```python
# Which one is the odd one out in this list?
model.wv.doesnt_match(["cat","dog","france"])
```
```
'france'
```

```python
# Which one is the odd one out in this list?
model.wv.doesnt_match(["bed","pillow","duvet","shower"])
```

```
'shower'
```


