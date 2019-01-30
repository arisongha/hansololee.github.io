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
그렇다면 어떻게 직관적인 숫자로 표현할 수 있을까요?

### 3. 단어간의 거리행렬 구하기

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

단어 사이의 거리행렬로 바꾸면 아래와 같이 표현됩니다.

|  /  | you | have | an | ... |
|:--:|:--:|:--:|:--:|:--:|
| you | 0| 1.7862 | 4.2347 | ... |
| have| 1.7862 | 0 | 2.9827 | ... |
| an  | 4.2347 | 2.9827 | 0 | ... |
| ... | ... | ... | ... | ... |

대각성분은 모두 0이고 대각을 중심으로 대칭인 행렬이 나왔습니다. 당연한 것이 'you'와 'you'는 같은 단어이므로 벡터의 거리도 '0'입니다. 또한 대칭인 이유는 같은 단어끼리의 거리가 대각을 중심으로 2번 표현되었기 때문입니다.

코드로 확인해보면

```python
distance(word2vec_vec[0],word2vec_vec[0])
```
> 결과: 0.0

```python
distance(word2vec_vec[1],word2vec_vec[3])
```
> 결과: 2.78799937717712

유클리드 거리를 구하는 `distance` function의 코드는 생략하였습니다. <a href="https://github.com/hansololee/data-science-school/blob/master/project/quora_insincere_questions_classification/word2vec_score.ipynb">이곳</a>에서 확인하실 수 있습니다.

### 4. 문장에 대한 스코어 구하기

우선 문장에 대한 스코어라고 표현했지만 어떤 것에 대한 스코어인지 궁금하실 겁니다. 예를 들자면 이런겁니다. `corpus`내에 인총차별을 대표하는 단어가 'racism'이라고 가정해봅니다. 그리고 우리가 dataset으로 사용하고 있는 Insincere한 문장 중에 아래와 같은 문장이 있습니다.

> 'Which baby be more sweeter to their parent Dark skin baby or light skin baby'

스코어라는 것은 이 문장의 각 단어들과 'racism' 이라는 단어간의 가중치(유사성)를 구하는 것이라고 생각하시면 됩니다. 그럼 천천히 그 방법을 알아보겠습니다.

먼저 우리는 위에서 단어 사이의 유클리드 거리를 구한 바 있습니다. 이 유클리드 거리로 표현한 방식에서는 숫자가 작을 수록 즉, 거리가 가까울 수록 유사한 단어라고 할 수 있습니다. 그렇다면 특정 단어와 거리가 가까운 단어(유사한 단어)는 높은 단어는 높은 가중치를 주려고 한다면 어떻게 하면 될까요? 아래와 같은 수식을 이용하면 됩니다.

$${ W }_{ ij }=exp\left( -\frac { d{ ({ x }_{ i },{ x }_{ j }) }^{ 2 } }{ 2\sigma^{2}  }  \right)$$

위 수식으로써 단어간의 거리는 정규분포의 모양으로 스케일링 되게 됩니다.

코드로 확인해보겠습니다. 우선 'racism'같이 문장을 스코어링 하고 싶은 대표단어와 `corpus`내의 모든 단어사이의 거리를 구해줍니다.

```python
def make_distance_matrix(word2vec_vec):
    word2vec_matrix = np.zeros((len(weight_word_index_list),len(word2vec_vec)))

    for i,word_index in enumerate(weight_word_index_list):
        for j in range(len(word2vec_vec)):
            word2vec_matrix[i][j] = distance(word2vec_vec[word_index],word2vec_vec[j])

    return word2vec_matrix

distance_matrix = make_distance_matrix(word2vec_vec)
distance_matrix
```
참고로 이 코드에서 'racism' 같은 단어가 담겨져 있는 `weight_word_index_list`는 위에서 변수에 저장한 word2vec으로 임베딩된 단어의 index입니다.

그 이후에 이 거리행렬을 위 수식으로 **표준화** 해줍니다.

```python
def make_weight_matrix(distance_matrix):
    word_weight_matrix = np.zeros((len(distance_matrix), len(distance_matrix[0])))

    std = np.std(word2vec_vec)
    for i in range(len(distance_matrix)):
        for j in range(len(distance_matrix[0])):
            word_weight_matrix[i][j] = exp((-distance_matrix[i][j]**2)/(2*(std**2)))
    return word_weight_matrix

word_weight_matrix = make_weight_matrix(word2vec_matrix)
word_weight_matrix
```

여기서 `make_distance_matrix` function과 `make_weight_matrix` function은 하나의 function 안에서 돌리는 것이 좋지만 코드의 이해를 돕기 위해 나누어서 표현해 주었습니다.

이렇게 되면 return된 `word_weight_matrix`는 'N X 67088' 의 행렬이 됩니다. 여기서 N은 문장을 스코어링해줄 'racism' 같은 단어를 얼마나 많이 `weight_word_index_list`에 포함시켰느냐에 따라 정해집니다.

마지막으로 문장을 스코어링하기전에 각각의 문장이 어떤 단어들로 표현되어 있는지 벡터화 해야합니다. 아래 표로 무슨 말인지 설명드리겠습니다.

|  /  | paragraph1 | paragraph2 | paragraph3 | ... |
|:--:|:--:|:--:|:--:|:--:|
| you | 1| 0 | 1 | ... |
| have| 1 | 0 | 0 | ... |
| an  | 1 | 1 | 0 | ... |
| ... | ... | ... | ... | ... |

위의 표를 보면 느낌이 오시겠지만 paragraph1에 해당하는 문장에는 you, have, an 이라는 단어가 모두 들어있어서 전부 1로 표현되었습니다. 반대로 해당 paragraph2 문장을 보시면 you, have 를 포함하지 않고 있어서 0으로 표현되어 있습니다.

이렇게 문장단위로 countvecorize 하는 코드를 보겠습니다.

```python
embed_word_list = embedding_model.wv.index2word
vect = CountVectorizer(vocabulary=embed_word_list)
cnt_matrix = vect.transform(train_target).toarray()
```

`cnt_matrix`를 확인해보면 아래와 같이 벡터화된 문장들의 리스트를 확인할 수 있습니다. 그리고 이 행렬의 사이즈는 `len(train_target)` X 67088 입니다.
> 결과: [[0,0,0,0,1,0,0,0,0,...0,1,0],...,]

이제 마지막 단계 입니다. 위에서 구했던 `word_weight_matrix` 와 방금 구했던 `cnt_matrix`를 **내적** 해주면 각 문장에 대해서 스코어를 구할 수 있게 됩니다. 무슨 말인지 아래 그림을 보면서 설명드리겠습니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/word2vec_withcode_02.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://hansololee.github.io/naturallanguageprocessing/word2vec_withcode/">출처</a></center>
<br/>

f1,f2,f3는 'racism' 같이 문장에 대해서 스코어링해주고 싶은 기준단어들입니다. 그리고 v1,v2,...,v7은 corpus내의 전체 단어의 개수입니다. 우리의 예시에서는 v67088까지 존재하겠군요. 즉 위 내적에서 첫번째 행렬은 기준단어들과 corpus내 모든 단어들 사이의 가중치행렬(`word_weight_matrix`)이라고 볼수 있습니다.  그리고 이 행렬에 곱해지는 벡터는 벡터화된 한 문장(`cnt_matrix`의 한 '행')이라고 볼수 있습니다. 이 들의 내적으로 나오는 행렬은 해당 문장의 f1에 대한 스코어가 0.8, f2에 대한 스코어가 0.7, f3에 대한 스코어가 0.4 가 되는 것입니다.

코드는 다음과 같습니다.

```python
def make_scored_matrix(word_weight_matrix,cnt_matrix):
    scored_matrix = []
    for i in cnt_matrix:
        scored_matrix.append(np.dot(word_weight_matrix,i))
    return scored_matrix

    scored_matrix = make_scored_matrix(word_weight_matrix,cnt_matrix)
    scored_matrix
```
이상으로 문장 스코어링에 대한 코딩을 마쳤습니다. 모든 코드를 보고 싶으시면 <a href="https://github.com/hansololee/data-science-school/blob/master/project/quora_insincere_questions_classification/word2vec_score.ipynb">이곳</a>을 방문해주시길 바랍니다.
