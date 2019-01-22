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

보통 $$P({A}_{1}),P({A}_{2}),P({A}_{3})$$는 미리 알고 있다는 의미의 **사전확률**로 불립니다. 그리고

$$P(B|A_1),P(B|A_2),P(B|A_3)$$
는 **우도(likelyhood probability)**라 부릅니다.

사건 $$B$$가 $$A_1$$에 기인했을 조건부확률은 아래와 같이 구할 수 있습니다.

$$% <![CDATA[
\begin{align*}
P({ A }_{ 1 }|B)&=\frac { P({ A }_{ 1 })P(B|{ A }_{ 1 }) }{ P(B) } \\
&=\frac { P({ A }_{ 1 })P(B|{ A }_{ 1 }) }{ P({ A }_{ 1 })P(B|{ A }_{ 1 })+P({ A }_{ 2 })P(B|{ A }_{ 2 })+P({ A }_{ 3 })P(B|{ A }_{ 3 }) }
\end{align*} %]]>$$
<br/>

$$P(A_1|B)$$
는 사건 B를 관측한 후에 그 원인이 되는 사건 $$A$$의 확률을 따졌다는 의미의 **사후확률**로 정의됩니다. 사후확률은 사건 $$B$$의 정보가 더해진, 사전확률의 업데이트 버전 정도라고 생각하면 좋을 것 같습니다.

## 검사시약 예시

베이즈 정리를 이용하여 문제를 풀어보겠습니다.

제약사에서 환자가 특정한 병에 걸린지 확인할 수 있는 시약을 만들었습니다. 그 병에 걸린 환자에게 시약을 테스트한 결과 99%의 양성 반응을 보였습니다. 병에 걸린지 확인되지 않은 환자가 이 시약을 테스트한 결과 양성 반응을 보였다면 이 환자가 그 병에 걸려 있을 확률은 몇 %일까요?

이 문제를 확률론의 용어로 다시 정리해보겠습니다.

우선 환자가 실제로 병에 걸린 경우를 사건 $$D$$라고 합니다. 그러면 병에 걸려있지 않은 경우는 $$D^C$$가 됩니다. 또 시약 테스트에서 양성 반응을 보이는 경우를 사건 $$S$$라고 하면 음성 반응을 보이는 경우는 사건 $$S^C$$ 입니다.

현재 주어진 확률 값은 병에 걸린 환자에게 시약을 테스트하였을때 야야성 반응을 보이는 확률입니다. 병에 걸렸다는 것은 추가된 조건 혹은 정보이므로 이 확률은

$$P(S|D)$$로 표기할 수 있습니다.

- 사건

    - 병에 걸리는 경우: 사건  D
    - 양성 반응을 보이는 경우: 사건  S
    - 병에 걸린 사람이 양성 반응을 보이는 경우: 조건부 사건  S|D
    - 양성 반응을 보이는 사람이 병에 걸려 있을 경우: 조건부 사건  D|S

- 문제
$$P(S|D)=0.99$$
가 주어졌을 때,  
$$P(D|S)$$
를 구하시오.

베이즈 정리에서

$$P(D|S) = \dfrac{P(S|D)P(D)}{P(S)}$$

임을 알고 있다. 그러나 이 식에서 우리가 알고 있는 것은

$$P(S|D)$$
뿐이고, $$P(D)$$나 $$P(S)$$는 모르기 때문에

$$P(D|S)$$
를 현재로서는 구할 수 없습니다.

추가 조사를 통해 필요한 정보를 다음과 같이 입수하였다고 하자.

- 이 병은 전체 인구 중 걸린 사람이 0.2% 인 희귀병이다.
- 이 병에 걸리지 않은 사람에게 시약 검사를 했을 때, 양성 반응, 즉 잘못된 결과(False Positive)가 나타난 확률이 5% 이다.

이를 확률론적 용어로 바꾸면 다음과 같다.

$$P(D) = 0.002$$
$$P(S|D^C) = 0.05$$

베이즈 정리를 사용하면

$$
\begin{eqnarray}
P(D|S)
&=& \dfrac{P(S|D)P(D)}{P(S)} \\
&=& \dfrac{P(S|D)P(D)}{P(S,D) + P(S,D^C)} \\
&=& \dfrac{P(S|D)P(D)}{P(S|D)P(D) + P(S|D^C)P(D^C)} \\
&=& \dfrac{P(S|D)P(D)}{P(S|D)P(D) + P(S|D^C)(1-P(D))} \\
&=& \dfrac{0.99 \cdot 0.002}{0.99 \cdot 0.002 + 0.05 \cdot (1 - 0.002)} \\
&=& 0.038
\end{eqnarray}
$$

즉, 시약 반응에서 양성 반응을 보이는 사람이 실제로 병에 걸려 있을 확률은 약 3.8% 에 불과합니다.
