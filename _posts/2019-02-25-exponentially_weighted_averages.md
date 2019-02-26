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

여기서 $$v_t$$ 는 $$t$$ 번째 데이터의 가중 지수가중이동평균입니다. 그리고 $$\beta$$ 값은 하이퍼파라미터로 최적의 값을 찾아야하는데 보통은 0.9를 많이 사용합니다. 마지막으로 $$\theta_t$$ 는 $$t$$ 번째 데이터의 값입니다. 그럼 먼저 $$\beta$$ 값을 변경해보면서 이 수식에 대한 이해를 해보겠습니다.

우선, $$\beta$$ 가 0.9일 경우를 보겠습니다. 사실 이 때의 $$v_t$$ 는 이전 10개의 데이터의 평균과 거의 같습니다. 이는 아래 식으로 계산해낼 수 있습니다.

$$v_t \approx$$ average of the previous $$\frac{1}{1-\beta}$$ data

우선 지금은 $$\beta$$ 에 따른 지수가중이동평균값을 이해하기위해 위 식이 성립하는 이유는 나중에 언급하겠습니다. 다시 돌아와서 $$\beta$$ 가 0.9일때 지수가중이동평균 그래프는 다음과 같이 그려집니다. 아래 붉은선 그래프는 일년동안의 기온변화를 지수가중이동평균으로 나타낸 그래프입니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/exp_weighted_average_01.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://www.deeplearning.ai/">Andrew Ng 강의 슬라이드</a></center>
<br/>

그렇다면 $$\beta$$ 값을 조금 높여서 0.98일때는 어떤 그래프가 그려질까요?

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/exp_weighted_average_02.png" | relative_url }}' alt='absolute'></center>
<br/>

위와 같이 $$\beta$$ 가 0.98일때, 0.9일때보다 완만하고 부드러운 곡선인 이유는 1/(1-0.98)의 값이 50이므로 위 녹색 그래프는 50개 데이터의 평균값으로 만들어지기 때문입니다. 더 많은 데이터의 평균값을 이용하기 때문에 곡선이 더 부드러워 집니다. 이는 원래의 데이터의 값과 더 멀어진다고도 할 수 있습니다. 더 큰 범위 데이터에서 평균 낸 값이기 때문입니다. 여기서 내릴 수 있는 결론은 $$\beta$$ 값이 클수록 선이 더 부드러워진다는 것입니다. 그렇다면 $$\beta$$ 를 0.5로 낮춘다면 어떤 그래프가 그려질까요? 그래프를 그리기 이전에 어떻게 그려질지 예측할 수 있을 것입니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/exp_weighted_average_03.png" | relative_url }}' alt='absolute'></center>
<br/>

1/(1-0.5)의 값이 2이므로 2개의 데이터의 평균값으로 그래프가 그려집니다. 오직 2개의 데이터만 이용하므로 노이즈가 심하고 민감한 그려지는 것입니다.

그럼 이제 처음에 봤던 지수가중이동평균 식을 상세하게 분석해 보겠습니다.

$$
% <![CDATA[
\begin{eqnarray*}
v_t &=& \beta v_{t-1} + (1-\beta)\theta_t \\
v_{100} &=& 0.9v_{99} + 0.1\theta_{100} \\
v_{99} &=& 0.9v_{98} + 0.1\theta_{99} \\
v_{98} &=& 0.9v_{97} + 0.1\theta_{98} \\
...
\end{eqnarray*} %]]>
$$

설명을 편하게 하기위해서 $$v_t$$의 순서를 역순으로 했습니다. 맨 위에 $$v_{100}$$ 에 대한 식은 아래와 같이 대체할 수 있습니다.

$$
\begin{document}
\[v_{100} = 0.1\theta_{100} + 0.9 \Colorcancel[red]{v_{99}} (0.1\theta_{99} + 0.9v_{98})]
\end{document}
$$  
