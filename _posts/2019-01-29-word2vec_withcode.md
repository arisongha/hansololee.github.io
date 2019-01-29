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
