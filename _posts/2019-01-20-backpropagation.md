---
title: 역전파(backpropagation) *
tags:
  - backpropagation
  - 역전파

categories:
  - DeepLearning
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 김도형 박사님의 <a href="https://datascienceschool.net/">데이터사이언스스쿨</a>의 강의록과 <a href="https://ratsgo.github.io/">이기창님의 ratsgo's blog</a>, 그리고 도서로는 <a href="https://www.google.com/imgres?imgurl=http://t1.gstatic.com/images?q%3Dtbn:ANd9GcQTNaO1S8OepMrlVwqXRaZZrRA6r20i5YVs7W8DrmqUUFI4hMGu&imgrefurl=https://books.google.com/books/about/Pattern_Recognition_and_Machine_Learning.html?id%3DkOXDtAEACAAJ%26source%3Dkp_cover&h=1080&w=753&tbnid=RaJaTb74pCAENM:&q=%ED%8C%A8%ED%84%B4+%EC%9D%B8%EC%8B%9D%EA%B3%BC+%EA%B8%B0%EA%B3%84+%ED%95%99%EC%8A%B5&tbnh=160&tbnw=111&usg=AI4_-kRrLNV8X_BiAzeQJwy9KQJE9XHfGA&vet=12ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ..i&docid=b2dKjxvzbtRRzM&itg=1&hl=ko-KR&sa=X&ved=2ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ">비숍의 패턴인식과 머신러닝</a> 을 참고하였음을 미리 밝힙니다.

# 역전파(backpropagation)
