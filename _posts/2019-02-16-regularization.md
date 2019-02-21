---
title: 정규화(Regularization) *
tags:
  - 정규화
  - Regularization

categories:
  - DeepLearning
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 deepLearning.ai의 <a href="https://www.deeplearning.ai/">Andrew Ng 교수님 강의</a>를 정리하였음을 미리 밝힙니다.

# 정규화(Regularization)

**정규화(Regularization)** 란 높은 분산으로 인해 뉴럴네트워크가 과대적합(Overfitting)하는 문제가 발생했을 때 가장 먼저 시도해야하는 해결 방법입니다. 물론 더 많은 훈련 데이터를 얻을 수 있어 높은 분산을 해결할 수 있다면 좋겠지만 그러기엔 시간과 비용이 너무 많이 소요됩니다. 먼저 정규화가 어떻게 작동하는지 <a href="https://hansololee.github.io/machinelearning/logistic_regression/">로지스틱 회귀</a>를 통해 살펴보겠습니다.

로지스틱 회귀는 아래에 정의된 비용함수 $$J$$를 최소화하는 것입니다.

$$J(w,b) = \frac {1}{m} \sum_{i=1}^m L\left(\hat y^{(i)},y^{(i)}\right)$$

여기에 정규화를 추가하기 위해서 정규화 매개변수라고 불리는 $$\lambda$$를 추가해야 합니다. 그럼 식은 아래와 같이 됩니다.

$$J(w,b) = \frac {1}{m} \sum_{i=1}^m L\left(\hat y^{(i)},y^{(i)}\right) + \frac{\lambda}{2m} ||w||^{2}$$

위와 같은 정규화 방법을 L2정규화라고 부릅니다. 이와 같이 매개변수 $$w$$만 정규화 하는 이유는 보통 매개변수 $$w$$는 꽤 높은 차원의 매개변수 벡터인데 반해 $$b$$는 하나의 숫자라서 거의 모든 매개변수는 $$b$$가 아닌 $$w$$에 있기 때문입니다. 그래서 L2정규화에서 $$b$$에 대한 정규화는 통상 무시해버립니다.

그럼 이제 본격적으로 뉴럴네트워크의 정규화에 대해 알아보겠습니다. 뉴럴네트워크도 로지스틱 회귀와 마찬가지로 비용함수 $$J$$를 가집니다. 다만 $$J$$는 모든 매개변수 $$w^{[1]},b^{[1]},...,w^{[L]},b^{[L]}$$를 갖습니다.(여기서 L은 뉴럴네트워크에 있는 층의 개수입니다.) 이러한 특성을 반영해 정규화 식을 도출해 낸다면 아래와 같은 식이 나오게 됩니다.

$$J(w,b) = \frac {1}{m} \sum_{i=1}^m L\left(\hat y^{(i)},y^{(i)}\right) + \frac{\lambda}{2m} \sum_{l=1}^{L} ||w^{[l]}||^{2}$$

마지막 과정으로 정규화가 추가된 비용함수의 경사하강법을 구현하는 방법을 알아보겠습니다. 우리는 이전에 역전파를 통해서 $$J$$의 $$w$$편미분값인 $$dw$$를 계산했습니다. 그리고 $$w^{[l]}$$에 기존의 $$w^{[l]}$$에서 $$\alpha dw^{[l]}$$를 뺀 값을 대입해 업데이트 하였습니다. 여기에 정규화 항을 추가하게 되면 $$dw$$는 $$dw^{[l]} + \frac{\lambda}{m}w^{[l]}$$ 이 됩니다. 그리고 이 새로운 $$dw$$를 경사하강법 식에 대입하게 되면 아래와 같은 새로운 식이 도출됩니다.

$$
% <![CDATA[
\begin{align*}
w^{[l]} = w^{[l]} - \alpha (dw^{[l]} + \frac{\lambda}{m}w^{[l]})
&=\begin w^{[l]} - \frac{\alpha \lambda}{m} w^{[l]} - \alpha dw^{[l]}
\end
&=\begin
\left(1- \frac{\alpha \lambda}{m} \right) w^{[l]} - \alpha dw^{[l]}
\end
\end{align*} %]]>
$$
