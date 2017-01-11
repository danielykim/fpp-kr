## 7.7 지수 평활에 대한 혁신 상태 공간 모델
이 장의 나머지 부분에서는 지금까지 살펴본 지수 평활 기법에 깔린 통계적인 모델을 공부할 것이다. 표 7.8에 나타낸 지수 평활 기법은 점 예측을 생성하는 알고리즘이다. 이 절에서 다루는 통계 모델은 같은 점 예측과 예측 구간도 생성한다. 어떤 통계 모델은 전체 예측 분포를 만들어줄 수 있는 무작위적 데이터 생성 과정이다. 여기에서 소개할 일반적인 통계적인 틀은 5장에서 소개한 모델 선택 기준을 사용하는 플랫폼을 제공하여 모델을 객관적으로 선택할 수 있게 도와준다.

각 모델은 관측된 데이터를 기술하는 측정식과 아직 관측되지 않은 성분이나 상태(수준, 추세, 계절성)가 시간에 따라 어떻게 변하는지 기술하는 몇 가지 전이식으로 구성된다. 이러한 이유에서 "상태 공간 모델"이라고 부른다.

각 기법에 대해 두 가지 모델이 존재한다. 하나는 덧셈 오차, 다른 하나는 곱셈 오차를 고려한다. 같은 평활 매개변수 값을 사용했다면, 두 모델로 얻은 점 예측은 같다. 하지만, 다른 예측 구간을 생성할 것이다.

덧셈 오차와 곱셈 오차를 사용하는 모델을 구분하기위해(또한 기법에서 모델을 구분하기위해) 표 7.7의 분류에 문자 하나를 더 추가하였다.  (오차E, 추세T, 계절성S)에 대해 각 상태 공간 모델을 ETS(.,.,.)로 라벨을 붙였다. 이러한 라벨도 지수 평활이라는 개념으로 생각할 수 있다. 표 7.7에 있는 것과 같은 표기법을 사용하여, 가능한 각 성분은 다음과 같다. 오차E=$$\lbrace A, M\rbrace$$, 추세T=$$\lbrace N,A,A_{d},M,M_{d} \rbrace$$, 그리고 계절성=$$\lbrace N,A,M\rbrace$$이다. 따라서, 총 30개의 상태 공간 모델이 존재한다. 15개는 덧셈 오차, 15개는 곱셈 오차를 다룬다.

### ETS(A,N,N): 덧셈 오차를 고려하는 단순 지수 평활
7.1절에서 이야기한 것처럼, 단순 지수 평활의 오차 보정 형태는 다음과 같이 주어진다.

$$
l_{t} = l_{t-1} + \alpha e_{t},
$$

여기에서 $$e_{t}=y_{t}-l_{t-1}$$이고 $$\hat{y}_{t|t-1}=l_{t-1}$$이다. 따라서, $$e_{t}=y_{t}-\hat{y}_{t|t-1}$$은 한 단계 예측 오차를 나타내고 $$y_{t}=l_{t-1}+e_{t}$$로 쓸 수 있다.

혁신 상태 공간 모델로 만들기 위해, $$e_{t}$$에 대한 확률 분포를 정하는 작업이 필요하다. 덧셈 오차를 고려하는 어떠한 모델에 대해, 한 단계 예측 오차 $$e_{t}$$가 정규 분포를 따르는 평균이 0이고 분산이 $$\sigma^{2}$$인 백색 소음이라고 가정하자. 이렇게 가정한 것을 다음과 같이 $$e_{t}=\epsilon_{t} \sim \text{NID}(0,\sigma^{2}) $$ 간단하게 쓸 수 있다. NID는 "normally and independently distributed정규적이고 독립적으로 분포된"의 약자이다.

그러면 모델의 식을 다음과 같이 쓸 수 있다.

$$
\begin{align*}
y_{t} &= l_{t-1} + \epsilon_{t} \qquad      (7.4) \\
l_{t} &= l_{t-1} + \alpha \epsilon_{t}. \quad      (7.5)
\end{align*}
$$

(7.4)를 *측정*(또는 관측) 식으로 (7.5)를 *상태*(또는 전이) 식으로 부르자. 이렇게 오차의 통계적인 분포를 함께 고려한 두 식이 전체 통계 모델을 구성한다. 구체적으로, 이러한 요소가 단순 지수 평활에 깔린 혁신 상태 공간 모델을 구성한다.

"혁신"이라는 단어는 이러한 종류의 설명에서 모든 식이 같은 무작위 오차 과정 $$ \epsilon_{t} $$을 사용한다는 사실에서 유래한다. 같은 이유에서, 이렇게 식을 세우는 과정을 여기에서 다루지 않는 오차의 다중 원천과는 반대로 "오차의 단일 원천"으로 부르기도 한다.

측정 식은 관측과 미관측 상태 사이의 관계를 보여준다. 이 경우에 관측값 $$ y_{y} $$는 수준 $$ l_{t-1} $$, 예측 가능한 부분$$ y_{t} $$, 무작위 오차 $$ \epsilon_{t} $$, 예측 불가능한 부분 $$ y_{t} $$의 선형 함수이다. 다른 혁신 상태 공간 모델에 대해서, 이러한 관계는 비선형적일 수 있다.

전이 식은 시간에 따른 상태의 변화를 나타낸다. 평활 매개변수 $$ \alpha $$의 영향은 이전에 언급한 기법에서 살펴본 것과 같다. 예를 들면, $$ \alpha $$는 연속적인 수준 변화의 정도를 조절한다. $$ \alpha $$ 값이 높으면, 수준이 더 급격하게 변한다. $$ \alpha $$ 값이 작으면, 더 매끄럽게 변한다. 가장 낮은 극한인 $$ \alpha=0 $$에서 시계열의 수준은 시간에 따라 변하지 않는다. 또 다른 극한인 $$ \alpha=1 $$에서는 무작위 걷기 모델 $$ y_{t}=y_{t-1}+\epsilon_{t} $$과 같게 된다.


### ETS(M,N,N): 곱셈 오차를 고려하는 단순 지수 평활
같은 방식으로, 한 단계 무작위 오차를 상대적인 오차로 기술하여 곱셈 오차를 고려하는 모델을 세울 수 있다.

$$
\epsilon_{t} = \frac{ y_{t} - \hat{y}_{t \vert t-1} }{ \hat{y}_{t \vert t-1} }
$$

여기에서 $$ \epsilon_{t} \sim \text{NID}(0, \sigma^{2} ) $$이다. $$ \hat{y}_{t \vert t-1}=l_{t-1} $$을 대입하면, $$ y_{t} = l_{t-1} + l_{t-1} \epsilon_{t} $$와 $$ \epsilon_{t} = y_{t} - \hat{y}_{t \vert t-1} = l_{t-1} \epsilon_{t} $$을 얻을 수 있다.

그 다음, 상태 공간 모델의 곱셈 형태를 다음과 같이 쓸 수 있다.

$$
\begin{align*}
y_{t} & = l_{t-1}(1+\epsilon_{t}) \\
l_{t} & = l_{t-1}(1+\alpha \epsilon_{t}).
\end{align*}
$$

### ETS(A,A,N): 덧셈 오차를 고려하는 Holt의 선형 기법
이 모델에서, 한 단계 예측 오차는 $$ \epsilon_{t} = y_{t} - l_{t-1} - b_{t-1} \sim \text{NID}(0,\sigma^{2}) $$로 주어진다고 가정한다. 이것을 Holt의 선형 기법에 대한 오차 보정식에 대입하면 다음과 같은 식을 얻는다.

$$
\begin{align*}
y_{t} & = l_{t-1} + b_{t-1} + \epsilon_{t} \\
l_{t} & = l_{t-1} + b_{t-1} + \alpha \epsilon_{t} \\
b_{t} & = b_{t-1} + \beta \epsilon_{t}
\end{align*}
$$

간결하게 쓰기 위해  여기에서 $$ \beta = \alpha \beta^{*} $$로 두었다.


### ETS(M,A,N): 곱셈 오차를 고려하는 Holt의 선형 기법
한 단계 예측 오차를 다음과 같이 쓰고

$$
\epsilon_{t} = \frac{ y_{y} - (l_{t-1} + b_{t-1}) }{ (l_{t-1} + b_{t-1}) }
$$

위에서 사용한 접근 방식을 다시 사용하면, 곱셈 오차를 고려하는 Holt의 선형 기법에 깔린 혁신 상태 공간 모델을 다음과 같이 쓸 수 있다.

$$
\begin{align*}
y_{t} & = (l_{t-1} + b_{t-1})( 1 + \epsilon_{t} ) \\
l_{t} & = (l_{t-1} + b_{t-1})( 1 + \alpha \epsilon_{t} )\\
b_{t} & = b_{t-1} + \beta ( l_{t-1} + b_{t-1} )\epsilon_{t}
\end{align*}
$$

여기에서 다시한번 $$ \beta = \alpha \beta^{*} $$ 와 $$ \epsilon_{t} =\text{NID}(0, \sigma^{2} ) $$로 두었다.


### 다른 ETS 모델
비슷한 방식으로, 표 7.8의 지수 평활 모델 각각에 대한 혁신 상태 공간 모델을 쓸 수 있다. 표 7.10에 ETS 체계에서 모든 모델에 대한 식을 나타내었다.

![](https://www.otexts.org/sites/default/files/resize/fpp/images/Table7-10-570x639.png)

표 7.10: ETS 체계의 각 모델에 대한 상태 공간 식.


### ETS 모델 측정하기
"가능성likelihood"을 최대화하는 것은 오차 제곱 합을 최소화하여 매개변수를 측정하는 것의 한 가지 대안이다. 가능성likelihood은 특정한 모델에서 일어나는 데이터의 확률이다. 그래서 큰 가능성은 좋은 모델과 관련 있다. 덧셈 오차 모델에 대해, 가능성을 최대화하면 오차 제곱 합을 최소화하여 얻은 결과와 같은 결과를 얻을 수 있다. 하지만, 곱셈 오차 모델에 대해서는 결과가 서로 다르다. 이 절에서는 평활 매개변수 $$ \alpha, \beta, \gamma, \phi $$와 초기 상태 $$ l_{0}, b_{0}, s_{0}, s_{-1}, \cdots, s_{-m+1} $$을 가능성을 최대화하여 구할 것이다.

평활 매개변수가 가질 수 있는 가능한 값은 제한되어있다. 전통적으로 매개변수 값은 식이 가중평균으로 해석될 수 있도록 0과 1사이 범위로 제한된다. 즉, $$ 0 < \alpha, \beta^{*}, \gamma^{*}, \phi < 1 $$이다. 상태 공간 모델에 대해, $$ \beta = \alpha \beta^{*} $$와 $$ \gamma = (1-\alpha)\gamma^{*} $$로 두었다. 그래서 전통적인 제한조건을 $$ 0 < \alpha < 1, 0 < \beta < \alpha, 0 < \gamma < 1-\alpha $$로 바뀐다. 실제적인 상황에서는 수치적인 어려움을 막기 위해 보통은 감쇠 매개변수 $$ \phi $$에도 제한을 둔다. 흔히 사용하는 제한 조건의 범위는 $$ 0.8 < \phi < 0.98 $$이다.

상태 공간 모델의 수학적인 특성을 고려하여 매개변수를 또 다른 방식으로 해석할 수 있다. 그러면, 매개변수는 현재 예측에 대해 계속되는 효과를 갖는 먼 미래 관측을 방지하도록 제한된다. 이로부터 매개변수에 대한 보통의 범위보다 덜 제한적인(하지만 항상 그렇지는 않은) 몇 가지 *허용성admissibility* 제한조건이 나온다. 예를 들면, ETS(A,N,N) 모델에 대해, 매개변수 범위는 보통 $$ 0 < \alpha < 1 $$이지만, 허용 범위는 $$ 0 < \alpha < 2 $$이다. ETS(A,A,N) 모델에 대해, 매개변수 범위는 보통 $$ 0 < \alpha < 1 $$과 $$ 0 < \beta < 2 $$이지만, 허용 범위는 $$ 0 < \alpha < 2 $$와 $$ 0 < \beta < 4 - 2\alpha $$이다.

### 모델 선택
ETS 통계 체계의 한 가지 큰 장점은 모델을 선택할 때 정보 기준information criteria을 사용할 수 있다는 것이다. 주어진 시계열에 대해 30가지 ETS 모델 중에서 가장 적절한 것을 결정하는데 5.3절에서 소개한 $$\text{AIC}, \text{AIC}_{c}, \text{BIC}$$을 사용할 수 있다.

ETS 모델에 대해, Akaike의 정보 기준(AIC)은 다음과 같이 정의된다.

$$
\text{AIC} = -2 \log (L) + 2k,
$$
    
여기에서 $$ L $$은 모델의 가능성likelihood이고 $$ k $$는 잔여 분산을 포함하여 추정된 매개변수와 초기 상태의 전체 수이다. 적은 표본에 대해 보정된 AIC는 다음과 같이 정의되고,

$$
\text{AIC}_{c} = \text{AIC} + \frac{ 2k (k + 1) }{ T - k -1 },
$$

Bayesian 정보 기준은

$$
\text{BIC} = \text{AIC} + k\left[ \log(T) - 2 \right].
$$

이다.

(오차E, 추세T, 계절성S)의 몇 가지 조합으로부터 수치적인 어려움이 생길 수 있다. 구체적으로, 그런 불안정성을 일으키는 모델은 다음과 같다. ETS(M,M,A), ETS(M, M$$_{d}$$, A), ETS(A, N, M), ETS(A,A,M), ETS(A, A$$_{d}$$, M), ETS(A, M, N), ETS(A, M, A), ETS(A, M, M), ETS(A, M$$_{d}$$, N), ETS(A, M$$_{d}$$, A) 그리고 ETS(A, M$$_{d}$$, M)이다. 모델을 선택할 때 보통 이러한 특정 조합은 고려하지 않는다.

곱셈 오차를 고려하는 모델은 데이터가 분명하게 양수일 때 유용하지만, 데이터에 0이나 음수가 있으면 수치적으로 안정하지 않다. 그래서 곱셈 오차 모델은 시계열이 양수가 분명하게 양수가 아닐 때 사용하지 않는다. 이러한 경우에는 여섯 가지 덧셈 모델을 적용해야할 것이다.


### R에서 ets() 함수
forecast 패키지의 `ets()` 함수를 사용하여 R에서 모델을 측정할 수 있다. 아래의 R 코드는 이 함수에서 지원하는 모든 가능한 입력값과 디폴트 값을 보여준다. 시계열만 넣고, 다른 입력값을 디폴트 값으로 두면, 자동으로 적절한 모델이 선택된다. 아래에 각 입력값 설명이 있다.

```R
ets(y, model="ZZZ", damped=NULL, alpha=NULL, beta=NULL,
    gamma=NULL, phi=NULL, additive.only=FALSE, lambda=NULL,
    lower=c(rep(0.0001,3), 0.8), upper=c(rep(0.9999,3),0.98),
    opt.crit=c("lik","amse","mse","sigma","mae"), nmse=3,
    bounds=c("both","usual","admissible"),
    ic=c("aicc","aic","bic"), restrict=TRUE)
```

`y`

예측할 시계열

`model`

ETS 분류와 표기법을 사용하여 모델을 나타낸 3개의 문자 코드. 가능한 입력은 다음과 같다. "N"은 없다는 것을, "A"는 덧셈, "M"은 곱셈, 또는 "Z"는 자동으로 선택하는 것을 의미한다. 각 입력값을 "Z"로 두면 정보 기준에 따라 성분을 선택한다. 디폴트 값 ZZZ는 모든 성분을 정보 기준에 따라 선택하는 것을 의미한다.

`damped`

만약에 `damped=TRUE`이면, 감쇠 추세 ( $$ A_{d} $$와 $$ M_{d} $$중에서 하나를 )를 사용한다. 만약에 `damped=FALSE`이면, 비-감쇠 추세를 사용한다. 만약에 `damped=NULL`이면(디폴트), 정보 기준에 따라 감쇠와 비-감쇠 추세 중에서 하나를 선택한다.

`alpha`, `beta`, `gamma`, `phi`
평활 매개변수 값은 이러한 입력으로 정할 수 있다. 만약에 `NULL`로 정하면(각각의 디폴트 설정), 매개변수를 추정한다.

`additive.only`
`additive.only=TRUE`이면, 덧셈 성분을 고려하는 모델만 고려한다. 그렇지 않으면, 모든 모델을 고려한다.

` lambda`
Box-Cox 변환 매개변수. `lambda=NULL`이면(디폴트 값), 이 매개변수는 무시된다. 그렇지 않으면, 모델을 추정하기 전에 시계열이 변환될 것이다. `lambda`가 `NULL`이 아닐 때는, `additive.only`는 `TRUE`로 정해진다.

`opt.crit`
추정에서 사용할 최적화 기준. 디폴트 설정은 `opt.crit=lik`일 때, 최대 가능성 추정maximum likelihood estimation이다.

`bounds`
이것으로 매개변수에서 사용할 제한조건을 정할 수 있다. 전통적인 제한조건을 `bounds="usual"`을 사용하여 정하고, 허용되는 제한조건은 `bounds="admissible"`을 사용하여 정한다. 디폴트(`bounds="both"`)에는 두 제한조건 모두 만족시키기 위한 매개변수가 필요하다.

`ic`
모델을 선택할 때 사용할 정보 기준이고 디폴트 값은 `aicc`이다.

`restrict`
먄약에 `restrict=TRUE`(디폴트)이면, 모델을 선택할 때 수치적으로 어려운 모델을 고려하지 않는다.


### ETS 모델로 예측하기
$$ t=T+1, T+2, \cdots, T+h $$와 $$ t>T $$에 대한 식을 반복하고 $$ t>T $$에 대해 모두 $$ \epsilon_{t}=0 $$로 두어 점 예측을 얻을 수 있다. 예를 들면, ETS(M,A,N) 모델에 대해,

$$
y_{T+1} = ( l_{T} + b_{T} ) ( 1 + \epsilon_{T+1} ).
$$

따라서

$$
\hat{y}_{T+1 \vert T} = l_{T}+b_{T}.
$$

비슷하게,

$$
\begin{align*} 
y_{T+2} &= (\ell_{T+1} + b_{T+1})(1 + \varepsilon_{T+1})\\ 
&= \left[(\ell_T + b_T)(1+ \alpha\varepsilon_{T+1}) + b_T + \beta (\ell_T + b_T)\varepsilon_{T+1}\right] ( 1 + \varepsilon_{T+1}).
\end{align*}
$$

따라서,

$$
\begin{align*} 
\pred{y}{T+2}{T}&= \ell_{T}+2b_{T}, 
\end{align*}
$$

이다. 이러한 예측은 Holt의 선형 기법과 ETS(A,A,N) 모델로 예측한 것과 같다. (같은 매개변수를 사용했다고 가정하면) 기법으로 얻은 점 예측과 이 기법을 이루는 두 모델로 얻은 점 예측은 동일하다.

이 모델의 큰 장점은 기법을 사용하여 할 수 없었던 예측 구간을 구하는 일도 할 수 있다는 것이다. 덧셈 기법과 곱셈 기법의 예측 구간은 서로 다르다.

몇 가지 모델에 대해, 예측 구간을 구할 수 있도록 하는 정확한 식이 존재한다. 모든 모델에 대해 작동하는 더 일반적인 접근 방식은 상태의 마지막 추정에 대해 미래 표본 경로를 모사하는 것과 이렇게 모사한 미래 경로의 백분위수로 예측 구간을 구하는 것이다. R에서 forecast 패키지의 `forecast` 함수를 이용하여 이러한 작업을 할 수 있다. 아래의 R 코드는 ETS 모델을 적용했을 때, 이 함수의 모든 가능한 입력을 나타낸다. 각 입력을 살펴보자.

```R
forecast(object, h=ifelse(object$m>1, 2*object$m, 10),
    level=c(80,95), fan=FALSE, simulate=FALSE, bootstrap=FALSE,
    npaths=5000, PI=TRUE, lambda=object$lambda, ...)
```

`object`

`ets()` 함수로부터 넘겨받는 객체


`h`

예측 수평선 -- 예측할 기간 갯수

`level`

예측 구간에 대한 신뢰도

`fan`

`fan=TRUE`이면, `level=seq(50,99,by=1)`이다. fan 그래프로 나타낼 때 적절하다.

`simulate`

`simulate=TRUE`이면, 대수적 식을 사용하는 대신 모사하여 예측 구간을 생성한다. `simulate=FALSE`이고 대수적 식을 사용할 수 없는 특정 모델이 존재하는 경우에 모사를 사용한다.

`bootstrap`
`bootstrap=TRUE`이고 `simulate=TRUE`이면, 모사한 예측 구간에서 정규 분포 오차 대신 재-표본 오차를 사용한다.

`npaths`
모사 예측 구간을 구할 때 표본 경로의 수.

`PI`
`PI=TRUE`이면, 예측 구간을 생성한다. 그렇지 않으면, 예측값만 계산한다. `PI=FALSE`이면, `level`, `fan`, `simulate`, `bootstrap`과 `npaths` 전부가 무시된다.

`lambda`
Box-Cox 변환 매개변수. `lambda=NULL`이면 무시된다. 그렇지 않으면, 역 Box-Cox 변환을 통해 예측이 역-변환된다.


### 예제 7.1 원유 생산 예제(재검토)
그림 7.8은 ETS(A,N,N) 모델로 추정한 점 예측과 예측 구간을 나타낸다. 매개변수 $$ \alpha $$와 초기 수준 $$ l_{0} $$에 대한 추정치는 각각 0.89와 447.89이다. 이는 이전에 단순 지수 평활 기법으로 추정한 값과 같다(예제 7.1의 시작부분을 보라). 그래프는 예측 구간을 생성하는 것이 중요하다는 것을 보여준다. 여기 나타낸 구간은 상대적으로 넓어서, 큰 불확실성을 고려하지 않고 점 예측을 해석하면 오해를 불러일으킬 수 있다.

![](https://www.otexts.org/sites/default/files/resize/fpp/images/fig_7_Oil-570x333.png)

그림 7.8: ETS(A,N,N) 모델로 얻은 점 예측과 80%과 95% 예측 구간.

R code
```R
oildata <- window(oil,start=1996,end=2007)
fit <- ets(oildata, model="ANN")
plot(forecast(fit, h=3), ylab="Oil (millions of tones)")
fit$par
```

R output
```R
  alpha       l
  0.892 447.489
```

### 예제 7.4 호주 국제선 항공편 승객의 숙박일 수
이제 2011년부터 2012년 사이의 호주 국제선 항공편 승객의 숙박일 수 예측에 ETS 통계 체계를 적용해보자. `ets()` 함수를 가지고 AICc를 최소화하여 모델을 선택하자.

R code
```R
vndata <- window(austourists, start=2005)
fit <- ets(vndata)
summary(fit)
```

R output
```R
ETS(M,A,M) 

Call:
 ets(y = vndata) 

  Smoothing parameters:
    alpha = 0.4504 
    beta  = 4e-04 
    gamma = 0.0046 

  Initial states:
    l = 32.4349 
    b = 0.6533 
    s=1.0275 0.9463 0.7613 1.2648

  sigma:  0.0332

    AIC    AICc     BIC 
105.972 115.572 115.397 

Training set error measures:
                    ME    RMSE    MAE       MPE    MAPE     MASE      ACF1
Training set -0.081646 1.33328 1.0647 -0.221243 2.65469 0.374361 -0.089119
```

선택된 모델은 ETS(M,A,M)이다.

$$
\begin{align*}
y_{t} &= (\ell_{t-1} + b_{t-1})s_{t-m}(1 + \varepsilon_t)\\
\ell_t &= (\ell_{t-1} + b_{t-1})(1 + \alpha \varepsilon_t)\\
b_t &=b_{t-1} + \beta(\ell_{t-1} + b_{t_1})\varepsilon_t\\
s_t &=  s_{t-m}(1+ \gamma \varepsilon_t).
\end{align*}
$$

추정된 매개변수는 $$ \alpha=0.4504, \beta=0.0004, \gamma=0.0046 $$이다. 출력값은 $$ l_{0}, b_{0}, s_{0}, s_{-1}, s_{-2}, s_{-3} $$에 대한 초기값 추정치이다. 이 모델에 대한 표본 내 RMSE는 표 7.5와 7.6에 각각 나타낸 덧셈과 곱셈 계절성 Holt-Winters 기법보다 약간 작다. 그림 7.9는 시간에 따른 상태를 나타내고 그림 7.10은 모델로부터 얻은 점 예측과 95%와 80% 예측을 나타낸다. 구간이 원유 생산 예제의 예측 구간에 비해 훨씬 더 좁아졌다.

![](https://www.otexts.org/sites/default/files/resize/fpp/images/fig_7_MAMstates-570x456.png)

그림 7.9: 추정된 상태를 시간에 따라 그림으로 나타낸 것.

R code
```R
plot(fit)
```

![](https://www.otexts.org/sites/default/files/resize/fpp/images/fig_7_MAMforecasts-570x356.png)

그림 7.10: ETS(M,A,M) 모델로 호주 국제선 항공편 승객의 숙박일 수를 예측한 것.

```R
plot(forecast(fit,h=8),
  ylab="International visitor night in Australia (millions)")
```

