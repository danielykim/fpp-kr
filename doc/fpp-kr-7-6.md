## 7.6 지수 평활 기법 분류
우리가 지금까지 살펴본 것이 지수 평활 기법의 전부가 아니다. 추세와 계절적인 성분의 조합을 고려해보면, 15개의 지수 평활 기법이 가능하고 이것을 표 7.7에 나타내었다. 각 기법은 '추세'와 '계절적인' 성분의 종류를 나타내는 문자의 쌍으로 표시하였다. 예를 들면, (A, M)은 덧셈 추세와 곱셈 계절성을 사용하는 기법을, (M, N)은 곱셉 추세와 계절성이 없는 기법을 나타낸다.

|  |  | 계절적인 성분 |  |
|--|---|--------------|--|
| 추세 성분 | N (없음) | A (덧셈) | M (곱셈) |
| N (없음) | (N,N) | (N,A) | (N,M) |
| A (덧셈) | (A,N) | (A,A) | (A,M) |
| Ad (덧셈 감쇠) | (Ad,N) | (Ad,A) | (Ad,M) |
| M (곱셈) | (M,N) | (M,A) | (M,M) |
| Md (곱셈 감쇠) | (Md,N) | (Md,A) | (Md,M) |

몇몇은 이미 살펴본 것이다.

|       |   |                              |
|-------|---|------------------------------|
| (N,N) | = | simple exponential smoothing |
| (A,N) | = | Holts linear method |
| (M,N) | = | Exponential trend method |
| (Ad,N) | = | additive damped trend method |
| (Md,N) | = | multiplicative damped trend method |
| (A,A) | = | additive Holt-Winters method |
| (A,M) | = | multiplicative Holt-Winters method |
| (Ad,M) | = | Holt-Winters damped method |

이러한 분류는 Pegels가 1969년에 제안한 것이다. 이후에 Gardner가 1985년에 덧셈 감쇠 추세 기법을, Taylor가 2003년에 곱셈 감쇠 추세 기법을 추가하여 확장되었다.

표 7.8은 모든 가능한 15개의 지수 평활 기법에 대한 재귀식을 나타낸 것이다. 표의 각 칸에는 $$h$$ 단계 앞 예측식과 평활식이 있다. 표 7.9에 가장 흔히 사용하는 지수 평활 기법 몇 가지에 대해 초기값을 선택하는 전략을 나타냈다. 이러한 전략을 직접 사용하는 것을 추천하진 않는다. 대신에, 이러한 값은 최적화 과정을 시작할 때 유용하다.


![](https://www.otexts.org/sites/default/files/resize/fpp/images/Table7-8-570x295.png)


표 7.8: 재귀적 계산과 점 예측에 대한 식. 각 경우에, $$ l_{t} $$는 시간 $$t$$에서 시계열의 수준을, $$ b_{t} $$는 시간 $$ t $$에서 기울기를, $$ s_{t} $$는 시간 $$ t $$에서 시계열의 계절적인 성분을, $$ m $$은 한 연도 안에 계절의 갯수를 나타낸다. $$ \alpha, \beta^{*}, \gamma $$는 평활 매개변수이다. $$ \phi_{h}=\phi+\phi^{2}+\cdots+\phi^{h} $$와 $$ h_{m}^{+} = \lfloor (h-1) \mod m \rfloor + 1$$이다.


| 기법 | 초기값 |
|------|--------|
| (N,N) | $$l_{0}=y_{1}$$ |
| (A,N)  (Ad,N) | $$l_{0}=y_{1}, b_{0}=y_{2}-y_{1}$$ |
| (M,N)  (Md,N) | $$l_{0}=y_{1}, b_{0}=y_{2}/y_{1}$$ |
| (A,A)  (Ad,A) | $$l_{0}=\frac{1}{m}(y_{1}+\cdots+y_{m}) $$ |
| | $$ b_{0}=\frac{1}{m}[\frac{y_{m+1}-y_{1}}{m}+\cdots+\frac{y_{m+m}-y_{m}}{m}] $$ |
| | $$s_{0}=y_{m}-l_{0}, \ s_{-1}=y_{m-1}-l_{0}, \ \cdots, \ s_{-m+1}=y_{1}-l_{0}$$ |
| (A,M)  (Ad,M) | $$l_{0}=\frac{1}{m}(y_{1}+\cdots+y_{m}) $$ |
| | $$ b_{0}=\frac{1}{m}[\frac{y_{m+1}-y_{1}}{m}+\cdots+\frac{y_{m+m}-y_{m}}{m}] $$ |
| | $$s_{0}=y_{m}/l_{0}, \ s_{-1}=y_{m-1}/l_{0}, \ \cdots, \ s_{-m+1}=y_{1}/l_{0}$$ |
