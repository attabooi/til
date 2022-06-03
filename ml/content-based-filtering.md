# Content Based Filtering

 There are two types of filtering methods: `Content Based Filtering` and `Collaborative Filtering`

 I tried to apply former type first for our team project. And planning to try another one and also `neural collaborative filtering` which is evaluated for having the highest performance for recommendation feature thesedays.

<br>

 ## Pros and Cons


### 🙆‍♂️Pros
- No need to have other customers' data.
- Possible to consider individual's epic taste.
- Plasible to recommend brand-new items or unique items.
- Easy to explain reasons of recommendation.

### 🤦‍♂️Cons

- Filter bubbles : It is a info anomaly from individual taste recommendation, means a phenomenon that users can only obtain contents which biased towards only one side.

## Code

I made a simple drama recommendation feature using content based filtering.

```bash
!pip install mecab-python3
!pip install unidic-lite
!pip install --no-binary :all: mecab-python3
```

<br><br>

```python
# dataset from https://www.kaggle.com/datasets/chanoncharuchinda/top-100-korean-drama-mydramalist
import MeCab
import pandas as pd

df = pd.read_csv('top100_kdrama_ko.csv')

mecab = MeCab.Tagger("-Owakati")
mecab.parse("kill bill").split()
test = mecab.parse(df['Synopsis'][0]).split()

df['token'] = 0
for i in range(0, len(df['Synopsis'])):
  df['token'][i] = mecab.parse(df['Synopsis'][i]).split()
  df['token'][i].extend(mecab.parse(df['Cast'][i]).split())
  df['token'][i].extend(mecab.parse(df['Genre'][i]).split())
  df['token'][i].extend(mecab.parse(df['Tags'][i]).split())

# remove keywords that seemed not related to similarity work.
list1 = [',','.','s',"'",'"','-','…','(',')','년']
for j in list1:
  for i in range(0, len(df['token'])):
    while j in df['token'][i]:
      df['token'][i].remove(j)
```

<br><br>

```python
from gensim.test.utils import common_texts
from gensim.models.doc2vec import Doc2Vec, TaggedDocument

documents = [TaggedDocument (doc, [i]) for i, doc in enumerate(df['token'])]
model = Doc2Vec(documents, vector_size=100, window=3, epochs=10, min_count=0, workers=4)
inferred_doc_vec = model.infer_vector(df['token'][0])

most_similar_docs = model.docvecs.most_similar([inferred_doc_vec], topn=10)

for index, similarity in most_similar_docs:
  print(f'{index}, similarity: {similarity}')
  print(documents[index])

index = []
similarity = []
for i in range(0,10):
  index.append(most_similar_docs[i][0])
  similarity.append(most_similar_docs[i][1])
```
<br><br>

```python
# definition for applying recommendation feature through Django
def content_recommendation(int):
    df = pd.read_csv('top100_kdrama_ko.csv')

    mecab = MeCab.Tagger("-Owakati")
    mecab.parse("kill bill").split()
    test = mecab.parse(df['Synopsis'][0]).split()

    df['token'] = 0
    for i in range(0, len(df['Genre'])):
        df['token'][i] = mecab.parse(df['Genre'][i]).split()
        df['token'][i].extend(mecab.parse(df['Tags'][i]).split())

    # 유사도와 상관없을 것 같은 요소 제거해주기
    
    list1 = [',','.','s',"'",'"','-','…','(',')','년']

    for j in list1:
        for i in range(0, len(df['token'])):
            while j in df['token'][i]:
                df['token'][i].remove(j)

    documents = [TaggedDocument(doc, [i]) for i, doc in enumerate(df['token'])]
    model = Doc2Vec(documents, vector_size=100, window=3, epochs=10, min_count=0, workers=4)
    inferred_doc_vec = model.infer_vector(df['token'][int])
    most_similar_docs = model.docvecs.most_similar([inferred_doc_vec], topn=10)

    for index, similarity in most_similar_docs:
        print(f'{index}, similarity: {similarity}')
        print(documents[index])

    index = []
    similarity = []
    for i in range(0, 10):
        index.append(most_similar_docs[i][0])
        similarity.append(most_similar_docs[i][1])

    return index, similarity
```