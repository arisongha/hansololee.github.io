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

