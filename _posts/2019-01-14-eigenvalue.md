
# 고유값, 고유벡터

어떤 벡터에 선형변환의 역할로 행렬을 곱하였을 때, 곱한 이 후에도 벡터 본래의 방향을 잃지 않았을 때 그 벡터를 **고유벡터**라고 합니다. 이때 크기는 달라질 수 있는데 그 달라진 크기를 나타내는 비율을 **고유값**이라고 합니다.

$$Av - \lambda v = (A - \lambda I) v = 0$$

위 식을 만족하는 실수 $$\lambda$$ 를 **고유값**, 벡터 $$v$$를 **고유벡터**라고 합니다.



# 특성방정식

고유값과 고유벡터는 $$A - \lambda I$$ 의 행렬식이 0이 되도록 하는 **특성방정식**의 해를 구하면 됩니다.

$$\det \left( A - \lambda I \right) = 0$$

예를 들어 행렬 $$A$$,

$$ A=
\begin{bmatrix}
4 & -5 \\
2 & -3
\end{bmatrix} $$

의 특성방정식은

$$\begin{eqnarray}
\det \left( A - \lambda I \right)
&=&
\det
\left(
\begin{bmatrix}
4 & -5 \\
2 & -3
\end{bmatrix}
-
\begin{bmatrix}
\lambda & 0 \\
0 & \lambda
\end{bmatrix}
\right)
\\
&=&
\det
\begin{bmatrix}
4 - \lambda & -5 \\
2 & -3 -\lambda
\end{bmatrix}
\\
&=& (4 - \lambda)(-3 -\lambda) + 10 \\
&=& \lambda^2 - \lambda - 2 = 0
\end{eqnarray}$$

그러므로 고유값은 -1, 2가 됩니다.

여기서 고유값이 2 일때 고유벡터를 구해보면

$$ \begin{bmatrix}
4 -2 & -5 \\
2 & -3 -2
\end{bmatrix}
\begin{bmatrix}
v_1  \\
v_2
\end{bmatrix}
=
\begin{bmatrix}
0  \\
0
\end{bmatrix}$$

이므로 고유벡터 $$v$$는
$$v =
\begin{bmatrix}
5  \\
2
\end{bmatrix}$$
라는 것을 알 수 있습니다.


어떤 벡터  $$v$$가 고유벡터가 되면 이 벡터에 실수를 곱한 벡터 $$cv$$ ,즉 $$v$$ 와 방향이 같은 벡터는 모두 고유벡터가 됩니다. 보통 고유벡터를 표시할때 길이가 1인 단위벡터가 되도록 정규화하여 표시합니다.

$$\dfrac{v}{\|v\|}$$

그러므로
$$
v=
\begin{bmatrix}
\dfrac{{5}}{\sqrt{29}}  \\
\dfrac{{2}}{\sqrt{29}}
\end{bmatrix}
$$
로 표현할 수 있습니다. 
