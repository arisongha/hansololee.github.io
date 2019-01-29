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

## word2vec 단어 임베딩

**임베딩(Embedding)** 이란 단어를 R차원의 벡터로 변환시켜주는 방법입니다. 벡터로의 변환은 Weight matrix에 의해 이루어지는데 이전 <a href="https://hansololee.github.io/naturallanguageprocessing/word2vec/">포스팅</a>을 참고하여 주시길 바랍니다.

