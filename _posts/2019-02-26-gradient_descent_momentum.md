---
title: Momentum 최적화(Gradient Descent With Momentum) *
tags:
  - Momentum
  - 모멘텀

categories:
  - Optimization
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 deepLearning.ai의 <a href="https://www.deeplearning.ai/">Andrew Ng 교수님 강의</a>를 정리하였음을 미리 밝힙니다.

# Momentum 최적화(Gradient Descent With Momentum)

비용함수 최적화를 위해 다음과 같은 등고선을 경사하강법을 통해 최소값을 찾아나간다고 가정해봅니다. 파란색 직선과 같이 많은 단계를 거치면서 최소값으로 나아가며 천천히 진동하게 됩니다. 이렇게 위아래로 일어나는 진동은 경사하강법의 속도를 느리게 하고 더 큰 학습률(learning rate)을 사용하는 것을 막습니다. 왜냐하면 아래 그림의 보라색 화살표처럼 **Overshooting** 하게 되어서 최소값으로 수렴하지 않고 발산해 버릴 수 있기 때문입니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/momentum_01.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://deeplearning.ai">Andrew Ng 강의 슬라이드</a>
</center>
<br/>

이렇게 진동을 줄이면서 더 빠른 학습속도를 내기 위해서 고안된 방법이 바로 **Momentum** 입니다.
