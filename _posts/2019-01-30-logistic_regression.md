---
title: 로지스틱 회귀 *
tags:
  - 로지스틱
  - logistic
  - regression

categories:
  - MachineLearning
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.

# 로지스틱 회귀(Logistic regression)

로지스틱 회귀에 대해서 설명하기 이전에 이진분류에 대한 개념부터 짚고 넘어가겠습니다. **이진분류(Binary Classification)** 란 말 그대로 YES/NO 2가지로 구분하는 것입니다. YES 면 '1'로 표현하고 'NO'이면 0으로 표현합니다.
로지스틱 회귀란 바로 이런 이진 분류를 하기 위해서 사용되는 알고리즘입니다.

예를 들어 핫도그인지 핫도그가 아닌지 구분하고 싶은 사진이 주어졌다고 가정합니다. 그리고 이것을 feature $$x$$ 라고 합니다. 그리고 이 사진이 핫도그일거라는 예측값 $$\hat y$$ 를 구하는 알고리즘을 원합니다. 정확히 말하자면 $$\hat y$$ 는 $$x$$ 가 핫도그일 확률을 말합니다.

$$\hat y = P(y=1|x)$$

그리고 아래의 회귀분석에서 처럼 파라미터 $$W$$와 $$b$$ 가 존재합니다.

$$\hat y = W^Tx + b$$

하지만 로지스틱 회귀분석은 위에서 말씀드렸다시피 $$\hat y$$가 어떤 사건에 대한 확률값이므로 $$0 \leqq \hat y \leqq 1$$의 조건을 만족해야합니다. 그런데 회귀분석의 우변은 그 조건을 만족하지 않습니다. 그래서 여기에 **시그모이드 함수** 를 적용해줍니다.

$$\hat y = \sigma (W^Tx + b)$$

시그모이는 함수는 이렇게 생겼습니다.
<center><img data-action="zoom" src='{{ "/assets/img/logistic_regression_01.png" | relative_url }}' alt='absolute'></center>
<br/>
