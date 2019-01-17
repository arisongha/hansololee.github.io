---
title: 라그랑주 승수법(Lagrange multiplier) *
tags:
  - 라그랑주
  - Lagrange

categories:
  - optimization
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 김도형 박사님의 <a href="https://datascienceschool.net/">데이터사이언스스쿨</a>의 강의록과 <a href="http://darkpgmr.tistory.com"> 다크프로그래머님의 블로그</a>를 참고하였음을 미리 밝힙니다.

# 라그랑주 승수법(Lagrange multiplier)

현실의 최적화 문제는 **제한조건이 있는 최적화 문제**가 많습니다. 간단한 예시로는 다음과 같이 등식 제한조건이 있는 경우입니다.

$$x^{\ast} = \text{arg} \min_x f(x) \;\; (x \in \mathbf{R}^N) \\  
g_j(x)=0 \;\; (j=1, \ldots, M)$$

단순히 $$f(x)$$를 가장 작게하는 $$x$$값을 찾는게 아니라 $$g_1(x) = 0, g_2(x) = 0, \cdots, g_M(x) = 0$$을 모두 만족시키면서 $$f(x)$$를 가장 작게하는 $$x$$값을 찾아야 합니다.

예를 들어 목적함수와 제한조건이 다음과 같은 경우를 생각해 봅니다.

$$f(x_1, x_2) = x_1^2 + x_2^2$$
$$g(x_1, x_2) = x_1 + x_2 - 1 = 0$$

이 문제는 다음 그림처럼 $$g(x_1,x_2)=0$$으로 정의되는 직선상에서 가장 $$f(x1,x2)$$값이 작아지는 점 $$(x_1^{\ast}, x_2^{\ast})$$을 찾는 문제가 됩니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/con_optimization.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://datascienceschool.net/view-notebook/0c66f1810445488baf19cac79305793b/">출저</a></center>
<br/>

이렇게 등식 제한조건이 있는 최적화 문제는 **라그랑주 승수법(Lagrange multiplier)**을 사용하여 최적화를 할 수 있습니다.
라그랑주 승수법은 $$f(x)$$가 아니라 제한조건 등식$$g_j(x)$$에 $$\lambda$$라는 새로운 변수를 곱해서 더한 함수
$$h(x, \lambda) = f(x) + \sum_{j=1}^M \lambda_j g_j(x)$$
를 목적 함수로 간주하여 최적화합니다. 제한조건 등식 각각마다 새로운 $$\lambda_i$$를 추가해주어야 하므로 만약 제한조건이 $$M$$개이면 $$\lambda_1, \cdots , \lambda_M$$개의 변수가 새로 생긴 것과 같습니다.

확장된 목적함수 $$h(x_1, x_2, \ldots , x_N, \lambda_1, \ldots , \lambda_M)$$는 입력변수가 더 늘어났기 때문에 그레디언트 벡터를 영벡터로 만드는 조건이 다음처럼 $$N+M$$개가 됩니다.

$$
\begin{eqnarray}
\dfrac{\partial h(x, \lambda)}{\partial x_1}
&=& \dfrac{\partial f}{\partial x_1} + \sum_{j=1}^M \lambda_j\dfrac{\partial g_j}{\partial x_1} = 0 \\
\dfrac{\partial h(x, \lambda)}{\partial x_2}
&=& \dfrac{\partial f}{\partial x_2} + \sum_{j=1}^M \lambda_j\dfrac{\partial g_j}{\partial x_2} = 0 \\
\vdots & & \\
\dfrac{\partial h(x, \lambda)}{\partial x_N}
&=& \dfrac{\partial f}{\partial x_N} + \sum_{j=1}^M \lambda_j\dfrac{\partial g_j}{\partial x_N} = 0 \\
\dfrac{\partial h(x, \lambda)}{\partial \lambda_1} &=& g_1 = 0 \\
\vdots & & \\
\dfrac{\partial h(x, \lambda)}{\partial \lambda_M} &=& g_M = 0
\end{eqnarray}
$$

이 $$N+M$$개의 연립 방정식을 풀면 $$N+M$$개의 미지수 $$x_1, x_2, \ldots, x_N, \lambda_1, \ldots , \lambda_M$$를 구할수 있습니다. 구한 결과에서 $$x_1, x_2, \cdots, x_N$$
은 제한조건을 만족하는 최소값위치를 나타냅니다.

예를 들어 위에서 제시한 함수 $$f(x)$$를 최소화하는 문제를 라그랑주 승수법을 풀어보겠습니다.

$$h = f + \lambda g = x_1^2 + x_2^2 + \lambda ( x_1 + x_2 - 1 )$$

라그랑주 승수법을 적용하여 미분이 0인 위치를 구합니다.

$$
\begin{eqnarray}
\dfrac{\partial h}{\partial x_1}
&=& 2{x_1} + \lambda = 0 \\
\dfrac{\partial h}{\partial x_2}
&=& 2{x_2} + \lambda = 0 \\
\dfrac{\partial h}{\partial \lambda}
&=& x_1 + x_2 - 1 = 0
\end{eqnarray}
$$

고로 위 방정식을 풀면

$$x_1 = x_2 = \dfrac{1}{2}, \;\;\; \lambda = -1$$
을 구할 수 있습니다.

## 라그랑주 승수의 의미

만약 등식 제한조건 $$g_i$$이 있는가 없는가에 따라 최적화 해의 위치가 달라진다면 이 등식 제한조건에 대응하는 라그랑주 승수 $$\lambda_i$$는 0이 아닌 값이어야 합니다. $$\lambda_i=0$$이라면 원래의 문제와 같은 문제가 되어 최적화 해의 위치도 같게 나오기 때문입니다.
