---
title: 지수 가중 이동평균(Exponentially Weighted Averages) *
tags:
  - Exponentially Weighted Averages
  - 지수 가중 이동평균

categories:
  - Optimization
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 deepLearning.ai의 <a href="https://www.deeplearning.ai/">Andrew Ng 교수님 강의</a>를 정리하였음을 미리 밝힙니다.

# 지수 가중 이동평균(Exponentially Weighted Averages)

지수 가중 이동평균은 경사하강법 및 미니배치 경사하강법보다 효율적인 알고리즘을 이해하기위해 알아야하는 개념입니다. 이 개념은 최근 데이터에 더 많은 영향을 받는 데이터들의 평균 흐름을 계산하기 위한 것으로 최근 데이터에 더 높은 가중치를 줍니다.
