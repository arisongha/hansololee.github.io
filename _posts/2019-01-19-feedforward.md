---
title: 피드 포워드(feedforward) 네트워크 *
tags:
  - feedforward
  - 피드포워드

categories:
  - DeepLearning
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 김도형 박사님의 <a href="https://datascienceschool.net/">데이터사이언스스쿨</a>의 강의록과 도서로는 <a href="https://www.google.com/imgres?imgurl=http://t1.gstatic.com/images?q%3Dtbn:ANd9GcQTNaO1S8OepMrlVwqXRaZZrRA6r20i5YVs7W8DrmqUUFI4hMGu&imgrefurl=https://books.google.com/books/about/Pattern_Recognition_and_Machine_Learning.html?id%3DkOXDtAEACAAJ%26source%3Dkp_cover&h=1080&w=753&tbnid=RaJaTb74pCAENM:&q=%ED%8C%A8%ED%84%B4+%EC%9D%B8%EC%8B%9D%EA%B3%BC+%EA%B8%B0%EA%B3%84+%ED%95%99%EC%8A%B5&tbnh=160&tbnw=111&usg=AI4_-kRrLNV8X_BiAzeQJwy9KQJE9XHfGA&vet=12ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ..i&docid=b2dKjxvzbtRRzM&itg=1&hl=ko-KR&sa=X&ved=2ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ">비숍의 패턴인식과 머신러닝</a> 을 참고하였음을 미리 밝힙니다.

# 피드 포워드(feedforward) 네트워트

피드 포워드 네트워크 함수를 설명하기에 앞서 회귀와 분류의 선형모델을 살펴보고 가겠습니다.

$$y(x,w) = f\left( \sum_{j=1}^M w_j \phi_j(x) \right)$$

위의 수식과 같이 회귀와 분류의 선형모델은 비선형 기저함수 $$\phi_j(x)$$의 선형 결합을 바탕으로 하고 있습니다.

여기서 $$f$$는 분류의 경우는 비선형 활성화 함수고, 회귀의 경우는 항등함수 입니다. 우리의 목표는 이 모델을 확장시켜서 기저 함수 $$\phi_j(x)$$를 매개변수에 종속적이게 만들고 이 매개변수들이 계수 $${w_j}$$와 함께 훈련단계에서 조절되도록 하는 것입니다. 매개 변수적인 비선형 기저 함수를 만드는 데는 여러 방법이 있습니다. 뉴럴 네트워크는 위의 식의 형태를 그대로 따르는 기저 함수를 사용합니다. 각각의 기저 함수는 그 자체가 입력값의 선형 결합들에 대한 비선형 함수이며, 이 때 선형 결합에서의 계수들이 조절 가능한 매개변수입니다.

이를 바탕으로 기본적인 뉴럴 네트워크 모델을 만들어 보겠습니다. 첫번째로 입력변수 $$x_1,...,x_D$$에 대한 선형 결합을 $$M$$개 만들어 보겠습니다.

$$a_j = \sum_{i=1}^D w_{ji}^{(1)} x_i + w_{j0}^{(1)}$$

여기서 $$j=1,...,M$$이며, 위첨자 (1)은 해당 매개변수들이 네트워크의 첫 번째 계층에 해당한다는 것을 나타냅니다. 3장의 명명법에 따라 매개변수 $$w_{ji}^{(1)}$$을 **가중치**라 할 것이고 매개변수 $$w_{j0}^{(1)}$$을 편향(bias)이라 할 것입니다. $$a_j$$는 **활성도**라 한다. 각각의 선형 결합들은 미분 가능한 비선형 **활성화 함수** $$h$$에 의해 변환됩니다. 

$$z_j = h(a_j)$$



 








