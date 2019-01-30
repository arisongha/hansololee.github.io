---
title: Word2Vec으로 문장 스코어링하기 *
tags:
  - word2vec

categories:
  - NaturalLanguageProcessing
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 kaggle competition 중 Quora Insincere Questions Classification을 진행하면서 시도해본 것을 정리해놓은 글입니다. 기술적인 부분은 <a href="https://ratsgo.github.io/">이기창님의 ratsgo's blog</a>를 참고하면서 코딩하였습니다.


# Word2Vec으로 문장 스코어링하기

우선 word2vec을 사용해서 문장을 스코어링? 하게 된 배경부터 말씀드리겠습니다. 사실 kaggle competition 중 Quora Insincere Questions Classification(<a href="https://github.com/hansololee/data-science-school/tree/master/project/quora_insincere_questions_classification">이곳 참고</a>)을 진행하던 도중에 `f-score` 가 잘 나오지 않아서 `f-score`를 높이기 위해서 시도해 방법 중 하나입니다. 비록 **word2vec** 을 사용해서도 좋은 점수를 얻지는 못하였지만, 임베딩을 적용하면서 공부한 내용을 올립니다.

우선 이 competition은 Quora에 올라온 질문 중에 Insincere한 질문을 골라내는 Classification 문제입니다.
그리고 제가 시도했던 방법이자 여기서 보여드리고자 하는 방법은 Insincere한 문장에서 많이 나오는 단어 뭉치(단, sincere한 문장에서도 많이 나오는 단어는 제거한 단어 뭉치)와 유사도가 높은 문장에 높은 score를 부여해서 Insincere한 문장을 골라내는 방법입니다. 즉 **학습기반** Classification이 아니라 **규칙기반** Classification으로 `f-score`를 높이려고 했던것이지요.

그러면 word2vec을 사용하여 임베딩 후 스코어링한 과정을 살펴보겠습니다.

**임베딩(Embedding)** 이란 단어를 R차원의 벡터로 변환시켜주는 방법입니다. 벡터로의 변환은 Weight matrix에 의해 이루어지는데 이전 <a href="https://hansololee.github.io/naturallanguageprocessing/word2vec/">포스팅</a>을 참고하여 주시길 바랍니다.

### 1. 문장을 tokenizing 하기

첫번째로 진행할 작업은 train data로 주어진 문장을 tokenizing하는 것입니다. tokenizing을 해주어야 word2vec의 파라미터 값으로 들어갈 수 있기때문입니다.

우선 `csv` file을 read한 DataFrame이 아래 나와있습니다. 우리가 word2vec 모델에 적용할 corpus는 5번째 열, `lemmatized`가 될 것입니다. 참고로 이것은 `origin_question_text`에 'lemmatizing' 전저치를 해준 결과값입니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/word2vec_withcode_01.png" | relative_url }}' alt='absolute'></center>
<br/>

```python
final_df = pd.read_csv('./train_final5.csv')

train_target = final_df["lemmatized"]
train_target = train_target.apply(lambda x: str(x))
train_corpus = list(train_target.apply(lambda x: word_tokenize(x)))
```
이렇게 tokenizing 해주게 되면 아래와 같이 한 문장이 tokenizing 되어 list로 표현되고 이러한 list 들이 모여 전체 list를 이루게 됩니다.

```python
train_corpus[1]
```
> 결과 : ['Doe','you','have','an','adopt','dog','how','would','you','encourage','people','to','adopt','and','not','shop']

### 2. word2vec으로 단어 임베딩하기

그 다음으론 tokenized corpus에 gensim패키지를 활용해서 word2vec을 적용합니다.

```python
embedding_model = Word2Vec(train_corpus, size=100, window = 2, min_count=3, workers=2, iter=100, sg=1)
```

위 코드의 파라미터를 알아보자면, 첫번째 파라미터로 tokenized된 문장의 corpus를 넣어주었습니다. 그리고 `size=100`으로 설정하였으므로 이 단어들을 학습해서 각 단어를 100차원의 벡터로 변경해줍니다. 또한 `sg=1`은 방법론으로 'skip-gram' 방식을 채택했음을 의미합니다. `window=2`이므로 중심 단어 앞뒤로 2개까지 학습합니다. 즉 중심단어와 앞뒤로 2개니까 총 4번 학습하게 되는 것이지요. `min_count=3`이므로 corpus 내 최소 3번이상 출현한 단어를 대상으로 합니다. `worker=2`는 듀얼코어 사용을 의미(작업 여건에 따라 다르겠지만 보통 core-1정도 하는걸로 알고있습니다.)하고, 마지막으로 `iter=100`은 100번 반복 학습하라는 의미입니다.

그렇다면 임베딩 결과를 간단하게 테스트해보겠습니다. gensim 패키지의 기능 중에 ‘most_similar’은 대상 단어벡터와 그 외의 단어벡터 사이의 코사인 유사도를 구해줍니다. 그 값이 클 수록 비슷한 단어라는 뜻인데, 아래 코드는 ‘black’이라는 단어와 가장 비슷한 10개 단어를 출력하라는 코드입니다.

```python
print(embedding_model.most_similar(positive=["black"], topn=10))
```
>결과: [('white', 0.9206088185310364), ('brown', 0.691301703453064), ('Black', 0.6657844185829163), ('African', 0.6525812745094299), ('Hispanic', 0.6395251154899597), ('yellow', 0.6385411024093628), ('nonwhite', 0.6384801268577576), ('red', 0.6357205510139465), ('blue', 0.6329607963562012), ('White', 0.6251962184906006)]

결과값을 보시면 꽤나 유의미한 단어들이 추출되었음을 확인할 수 있습니다.

이제 벡터화된 단어들을 변수에 저장하겠습니다.
```python
word2vec_vec = embedding_model.wv.syn0
```
이 변수의 `shape`를 확인해보면
```python
word2vec_vec.shape
```
> 결과: (67088, 100)

67088개의 단어가 100차원으로 벡터화된 것을 확인할 수 있습니다. 예를 들어 아래와 같이 하나의 단어가 v1,v2,...,v100의 숫자로 이루어진 벡터로 표현됩니다.

|  /  | v1 | v2 | v3 | ... |
|:--:|:--:|:--:|:--:|:--:|
| you | 0.2385| 0.8465 | 0.3542 | ... |
| have| 0.8435 | 0.3451 | 0.7122 | ... |
| an| 0.1232 | 0.3425 | 0.8347 | ... |
| adopt| 0.7834 | 0.6519 | 0.2154 | ... |
| dog| 0.1894 | 0.9815 | 0.2344 | ... |
| ... | ... | ... | ... | ... |

하지만 위와 같이 100차원으로 표현된 단어에서 우리는 어떤 직관적인 뜻을 찾을 수 없습니다. 이건 그저 컴퓨터가 이해하는 단어의 표현일 뿐이지요.
그래서 이 단어 벡터를 주변 단어들간의 거리로 표현해 보겠습니다. 두 벡터가 가리키는 점 사이의 거리인 **유클리드 거리(Euclidean distance)** 를 사용할텐데, 이 거리는 **벡터의 차의 길이**로 구할 수 있습니다.

$$
\begin{eqnarray}
\| a - b \|
&=& \sqrt{\sum_{i=1} (a_i - b_i)^2} \\
&=& \sqrt{\sum_{i=1} ( a_i^2 - 2 a_i b_i + b_i^2 )} \\
&=& \sqrt{\sum_{i=1} a_i^2 + \sum_{i=1} b_i^2 - 2 \sum_{i=1} a_i b_i} \\
&=& \sqrt{\| a \|^2 + \| b \|^2  - 2 a^Tb }
\end{eqnarray}
$$

즉, 위 과정을 한눈에 표현하자면 아래 식과 같습니다.
$$\| a - b \|^2 = \| a \|^2 + \| b \|^2 - 2 a^T b$$
