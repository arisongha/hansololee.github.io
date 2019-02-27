---
title: Momentum 경사하강법(Gradient Descent With Momentum) *
tags:
  - Momentum
  - 모멘텀
  - 경사하강법

categories:
  - Optimization
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 deepLearning.ai의 <a href="https://www.deeplearning.ai/">Andrew Ng 교수님 강의</a>를 정리하였음을 미리 밝힙니다.

# Momentum 경사하강법(Gradient Descent With Momentum)

비용함수 최적화를 위해 다음과 같은 등고선을 경사하강법을 통해 최소값을 찾아나간다고 가정해봅니다. 파란색 직선과 같이 많은 단계를 거치면서 최소값으로 나아가며 천천히 진동하게 됩니다. 이렇게 위아래로 일어나는 진동은 경사하강법의 속도를 느리게 하고 더 큰 학습률(learning rate)을 사용하는 것을 막습니다. 왜냐하면 아래 그림의 보라색 화살표처럼 **Overshooting** 하게 되어서 최소값으로 수렴하지 않고 발산해 버릴 수 있기 때문입니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/momentum_01.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://deeplearning.ai">Andrew Ng 강의 슬라이드</a>
</center>
<br/>

이렇게 진동을 줄이면서 더 빠른 학습속도를 내기 위해서 고안된 방법이 바로 **Momentum** 입니다. 진행과정을 간단하게 확인해보겠습니다.

```python
On iteration t:
    #1
    compute dW, db on current mini-batch

    #2
    V_dW = beta * V_dW + (1-beta) * dW
    V_db = beta * V_db + (1-beta) * db

    #3
    W = W - alpha * V_dW
    b = b - alpha * V_db
```

중간에 `#2` 과정에 해당하는 코드는 이전에 소개해 드렸던 <a href="https://hansololee.github.io/optimization/exponentially_weighted_averages/">지수가중이동평균법</a>과 흡사합니다. 지수가중평균이동법에서 $$\beta$$ 값에 따라서 그려지는 그래프가 완만해지거나 민감해졌었습니다. Momentum 경사하강법에서도 이와같이 경사하강법의 단계를 부드럽게 만들어줍니다. 이해를 돕기위해 도함수 `dw`가 아래와 같다고 가정합니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/momentum_02.png" | relative_url }}' alt='absolute'></center>
<br/>

이 경사 수직방향의 평균을 구하면 진동이 '0'에 가까운 값이 나옵니다. 위 아래 방향의 양수와 음수를 평균하기 때문입니다. 반면에 수평방향의 평균은 모든 도함수가 오른쪽을 향하고 있으므로 꽤 큰 값을 가집니다. 그러므로 Momentum 경사하강법은 결국 수직방향에서는 훨씬 적은 진동을 가지게 되고, 수평방향으로는 더 빨리 진행할 수 있게 됩니다. 일반적인 경사하강법을 이용할 때와 비교한다면 다음과 같은 그림이 그려지게 되는 겁니다. 일반적으로 Momentum 경사하강법은 Momentum을 사용하지 않은 경사하강법보다 거의 항상 더 잘 작동합니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/momentum_03.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://arxiv.org/abs/1609.04747">출처</a>
</center>
<br/>
