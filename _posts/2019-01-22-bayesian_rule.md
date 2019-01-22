---
title: 베이즈 정리(bayesian_rule) *
tags:
  - bayesian_rule
  - bayes
  - 베이즈
  - 베이즈 정리

categories:
  - DeepLearning
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 김도형 박사님의 <a href="https://datascienceschool.net/">데이터사이언스스쿨</a>의 강의록과 <a href="https://ratsgo.github.io/">이기창님의 ratsgo's blog</a>, 그리고 도서로는 <a href="https://www.google.com/imgres?imgurl=http://t1.gstatic.com/images?q%3Dtbn:ANd9GcQTNaO1S8OepMrlVwqXRaZZrRA6r20i5YVs7W8DrmqUUFI4hMGu&imgrefurl=https://books.google.com/books/about/Pattern_Recognition_and_Machine_Learning.html?id%3DkOXDtAEACAAJ%26source%3Dkp_cover&h=1080&w=753&tbnid=RaJaTb74pCAENM:&q=%ED%8C%A8%ED%84%B4+%EC%9D%B8%EC%8B%9D%EA%B3%BC+%EA%B8%B0%EA%B3%84+%ED%95%99%EC%8A%B5&tbnh=160&tbnw=111&usg=AI4_-kRrLNV8X_BiAzeQJwy9KQJE9XHfGA&vet=12ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ..i&docid=b2dKjxvzbtRRzM&itg=1&hl=ko-KR&sa=X&ved=2ahUKEwiWvLeM4PHfAhXHw7wKHWt9AOIQ_B0wCXoECAYQEQ">비숍의 패턴인식과 머신러닝</a> 을 참고하였음을 미리 밝힙니다.

# 베이즈 정리(bayesian_rule)

일반적으로 사건 $$A_1,A_2,A_3$$가 서로 배반이고 $$A_1,A_2,A_3$$의 합집합이 표본공간과 같으면 사건 $$A_1,A_2,A_3$$는 표본공간 $$S$$의 분할이라고 정의합니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/bayes_01.png" | relative_url }}' alt='absolute'></center>
<br/>

$$P(B)=P({ A }_{ 1 }\cap B)+P({ A }_{ 2 }\cap B)+P({ A }_{ 3 }\cap B)$$

$$P(B)$$를 조건부확률의 정의를 이용해 다시 쓰면 아래와 같습니다. 이를 **베이즈 법칙** 이라고 합니다.

$$P(B)=P({ A }_{ 1 })P(B|{ A }_{ 1 })+P({ A }_{ 2 })P(B|{ A }_{ 2 })+P({ A }_{ 3 })P(B|{ A }_{ 3 })=\sum _{ i=1 }^{ 3 }{ P({ A }_{ i })P(B|{ A }_{ i }) }$$

보통 $$P({A}_{1}),P({A}_{2}),P({A}_{3})$$는 미리 알고 있다는 의미의 **사전확률**로 불립니다. $$P(B$$|$${A}_{1})$$,$$P(B$$|$${A}_{2})$$,$$P(B$$|$${A}_{3})$$는 **우도(likelyhood probability)**라 부릅니다.

사건 $$B$$가 $$A_1$$에 기인했을 조건부확률은 아래와 같이 구할 수 있습니다.

$$% <![CDATA[
\begin{align*}
P({ A }_{ 1 }|B)&=\frac { P({ A }_{ 1 })P(B|{ A }_{ 1 }) }{ P(B) } \\
&=\frac { P({ A }_{ 1 })P(B|{ A }_{ 1 }) }{ P({ A }_{ 1 })P(B|{ A }_{ 1 })+P({ A }_{ 2 })P(B|{ A }_{ 2 })+P({ A }_{ 3 })P(B|{ A }_{ 3 }) }
\end{align*} %]]>$$

$$P({A}_{1}$$|$$B)$$는 사건 B를 관측한 후에 그 원인이 되는 사건 $$A$$의 확률을 따졌다는 의미의 **사후확률**로 정의됩니다. 사후확률은 사건 $$B$$의 정보가 더해진, 사전확률의 업데이트 버전 정도라고 생각하면 좋을 것 같습니다.

## 검사시약 예시
