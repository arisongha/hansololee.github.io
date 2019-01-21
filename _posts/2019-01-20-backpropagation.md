---
title: 오차역전파(error backpropagation) *
tags:
  - error backpropagation
  - backpropagation
  - 역전파
  - 오차역전파

categories:
  - DeepLearning
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 김도형 박사님의 <a href="https://datascienceschool.net/">데이터사이언스스쿨</a>의 강의록과 <a href="https://ratsgo.github.io/">이기창님의 ratsgo's blog</a>, 그리고 도서로는 <a href="https://www.google.com/imgres?imgurl=http://t1.gstatic.com/images?q%3Dtbn:ANd9GcQTNaO1S8OepMrlVwqXRaZZrRA6r20i5YVs7W8DrmqUUFI4hMGu&imgrefurl=https://books.google.com/books/about/Pattern_Recognition_and_Machine_Learning.html?id%3DkOXDtAEACAAJ%26source%3Dkp_cover&h=1080&w=753&tbnid=RaJaTb74pCAENM:&q=%ED%8C%A8%ED%84%B4+%EC%9D%B8%EC%8B%9D%EA%B3%BC+%EA%B8%B0%EA%B3%84+%ED%95%99%EC%8A%B5&tbnh=160&tbnw=111&usg=AI4_-kRrLNV8X_BiAzeQJwy9KQJE9XHfGA&vet=12ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ..i&docid=b2dKjxvzbtRRzM&itg=1&hl=ko-KR&sa=X&ved=2ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ">비숍의 패턴인식과 머신러닝</a> 을 참고하였음을 미리 밝힙니다.

# 오차역전파(error backpropagation)

피드포워드 뉴럴 네트워크에서 오류 함수 $$E(w)$$의 기울기를 계산하는 효율적인 테크닉 중 하나인 **오차역전파**는 정보가 네트워크상에서 앞/뒤로 전달되는 지역적인 메시지 전달 방법입니다.

역전파라는 단어는 또한 다층퍼셉트론을 경사 하강법과 제곱합 오류함수를 바탕으로 훈련시키는 것을 지칭하기도 합니다.

대부분의 훈련 알고리즘들은 오류 함수의 값을 줄이는 반복적인 과정을 내포하고 있으며, 각 과정에서 순차적으로 가중치에 대한 조정이 일어납니다. 각 단계를 두개의 더 작은 개별 단계로 나눠보자면, 첫 번째 단계는 오류함수를 가중치에 대해서 미분하는 것입니다. 역전파 테크닉의 중요한 공헌은 이 미분을 시행하는 계산적으로 효율적인 방법을 제공했다는 점입니다. 이 단계에서 오류값이 네트워크를 타고 역방향으로 흘러가게됩니다.
두 번째 단계에서는 미분값을 사용하여 가중치를 조정해야 할 정도를 계산하게 됩니다.

## 오류함수 미분의 계산

일반적인 피드포워드 네트워크에서 각 유닛들은 입력값들의 가중 합산을 계산합니다.

$$a_j = \sum_i w_{ji}z_i$$

여기서 $$z_i$$는 유닛 $$j$$로 연결을 보내는 유닛(입력)의 활성도에 해당하며, $$w_{ji}$$는 이 연결에 해당하는 가중치다. 위 식의 합산은 비선형 활성화 함수 $$h$$에 의해 변화되어 유닛 $$j$$의 활성도 $$z_j$$가 된다.

$$z_j = h(a_j)$$

변수 $$z_i$$들 중 하나 또는 여럿이 입력일 수 있으며, $$z_j$$가 출력일 수 있습니다.

훈련 집합의 각 패턴들을 네트워크에 입력하고 위 두 식을 연속적으로 적용해서 모든 은닉 유닛과 출력 유닛의 활성도를 계산했다고 가정 하겠습니다. 이 과정은 **순전파**에 해당합니다. 왜냐하면 정보가 네트워크상에서 앞쪽 방향으로 흐로기 때문입니다.

이제는 $$E_n$$을 가중치 $$w_{ji}$$에 대해 미분하는 것을 고려해보겠습니다. 각 유닛들의 출력은 특정 입력 패턴 $$n$$에 따라 결정될 것입니다.

$$E_n$$은 합산된 입력값 $$a_j$$를 통해서만 가중치 $$w_{ji}$$에 종속적임을 알 수 있습니다. 따라서 편미분의 연쇄 법칙을 적용하여 다음과 같이 적을 수 있습니다.

$$\frac { \partial E_n }{ \partial w_{ji} } =\frac { \partial E_n }{ \partial a_j } \frac { \partial a_j }{ \partial w_{ji} }$$

$$\delta_j = \frac {\partial E_n}{\partial a_j}$$

여기서 $$\delta$$를 **오류(error)**라고 합니다.

또한

$$\frac {\partial a_j}{\partial w_{ji}} = z_i$$

이므로

$$\frac {\delta E_n}{\delta w_{ji}} = \delta_j z_i$$

와 같은 식을 도출할 수 있습니다. 이와 같은 식은 해당 유닛에 대한 가중치들 중 출력값 쪽에 있는 유닛의 $$\delta$$ 값과 가중치들 중 입력값 쪽에 있는 유닛의 $$z$$값을 곱해서 필요한 미분을 계산할 수 있다는 사실을 보여줍니다. 따라서 미분을 시행하기 위해서는 각각의 은닉 유닛들과 출력 유닛들에 대해서 $$\delta_j$$값을 구한 후 $$\frac {\delta E_n}{\delta w_{ji}} = \delta_j z_i$$ 을 적용하기만 하면 됩니다.

출력 유닛 활성화 함수로 정준 연결 함수를 사용한다는 가정하에 출력 유닛들의 $$\delta$$를 다음과 같이 구할 수 있습니다.

$$\delta_k = y_k - t_k$$

은닛 유닛들의 $$\delta$$를 구하기 위해서는 편미분의 연쇄법칙을 다시 한번 적용해야 합니다.

$$\delta_j = \frac { \partial E_n }{ \partial a_j } =\sum_k \frac { \partial E_n }{ \partial a_k } \frac { \partial a_k }{ \partial a_j }$$

이때 합산은 $$j$$가 연결을 보내는 모든 유닛 $$k$$들에 대해 이루어지게 됩니다.

이와 같은 식을 조합하면 다음과 같은 **역전파** 식을 얻을 수 있게 됩니다.

$$\delta_j = h'(a_j) \sum_k w_{kj} \delta_k$$

위 식으로부터 어떤 특정 은닛 유닛의 $$\delta$$ 값은 네트워크상에서 다음 단계에 있는 유닛들의 $$\delta$$ 값들을 전파시킴으로써 구할 수 있다는 것을 알 수 있습니다. 또한 출력 유닛들의 $$\delta$$값에 대해서는 이미 알고 있기 때문에 위의 식을 재귀적으로 적용하면 모든 은닉유닛의 $$\delta$$ 값을 구할 수가 있습니다.


## 간단한 예시

- 아래의 설명은 <a href="https://ratsgo.github.io/deep%20learning/2017/05/14/backprop/">이기창님의 ratsgo blog</a>를 발췌한 것입니다.

우선 위에서 설명한 순전파(forward propagation)와 역전파(back propagation)의 계산과정을 그래프로 나타내어 보았습니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/propagation_01.png" | relative_url }}' alt='absolute'></center>
<br/>

위에서 설명한 **순전파(forward propagation)**는 위 계산그래프에서 계산을 왼쪽에서 오른쪽으로 진행하는 단계입니다. 위 그림 기준으로는 녹색 화살표가 됩니다. 입력값 x는 함수 f를 거쳐 y로 순전파되고 있는 점을 확인할 수 있습니다.

반대로 계산을 오른쪽에서 왼쪽으로 진행하는 단계를 **역전파(backward propagation)**라고 합니다. 빨간색 화살표가 역전파를 가리킵니다.

여기에서 $$\frac{\partial L}{\partial y}$$의 의미에 주목할 필요가 있습니다. 지금은 예시이기 때문에 노드를 하나만 그렸지만, 실제 뉴럴네트워크는 이러한 노드가 꽤 많은 큰 계산그래프입니다. 이 네트워크는 최종적으로는 정답과 비교한 뒤 **Loss**를 구합니다.

우리의 목적은 뉴럴네트워크의 오차를 줄이는 데 있기 때문에, 각 파라메터별로 Loss에 대한 그래디언트를 구한 뒤 그래디언트들이 향한 쪽으로 파라메터들을 업데이트합니다. $$\frac{\partial L}{\partial y}$$는 $$y$$에 대한 Loss의 변화량, 즉 Loss로부터 흘러들어온 그래디언트라고 이해하면 좋을 것 같습니다.

이제는 현재 입력값 $$x$$에 대한 Loss의 변화량, 즉 $$\frac{\partial L}{\partial x}$$ 를 구할 차례입니다. 이는 **미분 연쇄법칙(chain rule)**에 의해 다음과 같이 계산할 수 있습니다.

$$\frac { \partial L }{ \partial x } =\frac { \partial y }{ \partial x } \frac { \partial L }{ \partial y }$$

이미 설명드렸듯이 $$\frac{\partial L}{\partial y}$$는 Loss로부터 흘러들어온 그래디언트입니다. $$\frac{\partial L}{\partial x}$$는 현재 입력값에 대한 현재 연산결과의 변화량, 즉 **로컬 그래디언트(Local Gradient)** 입니다.

다시 말해 현재 입력값에 대한 Loss의 변화량은 Loss로부터 흘러들어온 그래디언트에 로컬 그래디언트를 곱해서 구한다는 이야기입니다. 이 그래디언트는 다시 앞쪽에 배치돼 있는 노드로 역전파됩니다.

### 덧셈 노드

덧셈 노드의 수식은 아래와 같습니다.

$$z=f(x,y)=x+y$$

덧셈 노드의 로컬 그래디언트는 아래와 같습니다.

$$\frac { \partial z }{ \partial x } =\frac { \partial (x+y) }{ \partial x } =1\\ \frac { \partial z }{ \partial y } =\frac { \partial (x+y) }{ \partial y } =1$$

덧셈 노드의 계산그래프는 아래와 같습니다. 현재 입력값에 대한 Loss의 변화량은 로컬 그래디언트에 흘러들어온 그래디언트를 각각 곱해주면 됩니다. 덧셈 노드의 역전파는 흘러들어온 그래디언트를 그대로 흘려보내는 걸 확인할 수 있습니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/propagation_02.png" | relative_url }}' alt='absolute'></center>
<br/>

### 곱셈 노드

곱셈 노드의 수식은 아래와 같습니다.

$$z=f(x,y)=xy$$

곱셈 노드의 로컬 그래디언트는 아래와 같습니다.

$$\frac { \partial z }{ \partial x } =\frac { \partial (xy) }{ \partial x } =y\\ \frac { \partial z }{ \partial y } =\frac { \partial (xy) }{ \partial y } =x$$

곱셈 노드의 계산그래프는 아래와 같습니다. 현재 입력값에 대한 Loss의 변화량은 로컬 그래디언트에 흘러들어온 그래디언트를 각각 곱해주면 됩니다. 곱셈 노드의 역전파는 순전파 때 입력 신호들을 서로 바꾼 값을 곱해서 하류로 흘려보내는 걸 확인할 수 있습니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/propagation_03.png" | relative_url }}' alt='absolute'></center>
<br/>

### ReLU 노드

**활성화함수(activation function)**로 사용되는 **ReLU**는 다음 식처럼 정의됩니다.

$$y=x\quad (x>0)\\ y=0\quad (x\le 0)$$

ReLU 노드의 로컬 그래디언트는 아래와 같습니다.

$$\frac { \partial y }{ \partial x } =1\quad (x>0)\\ \frac { \partial y }{ \partial x } =0\quad (x\le 0)$$

계산그래프는 아래와 같습니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/propagation_04.png" | relative_url }}' alt='absolute'></center>
<br/>
