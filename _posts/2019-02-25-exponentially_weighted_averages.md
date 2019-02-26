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

지수 가중 이동평균은 경사하강법 및 미니배치 경사하강법보다 효율적인 알고리즘을 이해하기위해 알아야하는 개념입니다. 이 개념은 최근 데이터에 더 많은 영향을 받는 데이터들의 평균 흐름을 계산하기 위한 것으로 최근 데이터에 더 높은 가중치를 줍니다. 수식으로 알아보겠습니다.

$$v_t = \beta v_{t-1} + (1-\beta)\theta_t$$

여기서 $$v_t$$는 $$t$$번째 데이터의 가중 지수가중이동평균입니다. 그리고 $$\beta$$ 값은 하이퍼파라미터로 최적의 값을 찾아야하는데 보통은 0.9를 많이 사용합니다. 마지막으로 $$\theta_t$$는 $$t$$번째 데이터의 값입니다. 그럼 먼저 $$\beta$$ 값을 변경해보면서 이 수식에 대한 이해를 해보겠습니다.

우선, $$\beta$$가 0.9일 경우를 보겠습니다. 사실 이 때의 $$v_t$$는 이전 10개의 데이터의 평균과 거의 같습니다. 이는 아래 식으로 계산해낼 수 있습니다.

$$v_t \approx \frac{1}{1-\beta}$$data's average
