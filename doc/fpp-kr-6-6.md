## 6.6 분해법으로 예측하기
시계열 데이터를 연구하거나 시간적인 변화를 살펴볼 때 분해가 주로 유용하긴 하지만, 예측에도 사용할 수 있다.

덧셈 분해를 가정하면, 분해한 시계열을 다음과 같이 쓸 수 있다.

$$ 
y_{t} = \hat{S}_{t} + \hat{A}_{t},
$$ 

여기에서 $$ \hat{A}_{t} = \hat{T}_{t} + \hat{E}_{t} $$는 계절적으로 조절된 성분이다. 곱셈 분해를 사용한다면, 다음과 같이 쓸 수 있다.

$$ 
y_{t} = \hat{S}_{t} \hat{A}_{t},
$$ 

여기에서 $$ \hat{A}_{t} = \hat{T}_{t}\hat{E}_{t} $$ 이다.

분해한 시계열을 예측하기 위해서, 계절적인 성분 $$ \hat{S}_{t} $$와 계절적으로 조절된 성분 $$ \hat{A}_{t} $$를 각각 예측하겠다. 계절적인 성분이 변하지 않거나 엄청나게 느리게 변하는 상황을 보통 가정하고, 추정한 성분의 마지막 연도를 이용하여 예측한다. 다르게 말하면, 계절적인 성분에 단순 계절적인 방법을 사용할 수 있다.

계절적으로 조절된 성분을 예측하기 위해서는, 비-계절적인 예측 기법이 사용된다. 예를 들면, 점점 이동하는 임의 보행자 모델, Holt의 방법(다음 장에서 이야기 할), 아니면 비-계절적 ARIMA 모델(8장에서 이야기 할)을 사용할 수 있다.

### 예제 6.3 전자 장비 생산
![](https://www.otexts.org/sites/default/files/resize/fpp/images/elecequip4-570x333.png)

그림 6.11: 전자 장비 지수 데이터의 STL 분해로 얻은 계절적으로 조절된 데이터를 단순 예측한 것.

```R
fit <- stl(elecequip, t.window=15, s.window="periodic", robust=TRUE)
eeadj <- seasadj(fit)
plot(naive(eeadj), xlab="New orders index",
  main="Naive forecasts of seasonally adjusted data")
```

그림 6.11은 계절적으로 조절된 전자 장비 지수 데이터의 단순 예측이다. 이 예측값은 계절적인 성분의 계절적인 단순 예측을 더하여 "재-계절화"된 것이다. 원본 데이터의 결과 예측은 그림 6.12와 같다. 이 그래프에서 나타낸 예측 구간은 점 예측 방법으로 구성한 것이다. 즉, 계절적으로 조절된 데이터에 대한 에측 구간의 상한과 하한은 계절적인 성분의 예측을 더하여 "재-계절화"되었다. 계절적인 성분에서 불확실성이 계절적으로 조절된 데이터의 불확실성보다 무시해도 될 만큼 훨씬 작기 때문에 이렇게 해도 합리적이다.

![](https://www.otexts.org/sites/default/files/resize/fpp/images/elecequip5-570x333.png)

그림 6.12: 데이터를 STL 분해하여, 계절적으로 조절된 데이터의 단순 에측과 계절적인 성분의 계절적인 단순 예측에 근거하여 전자 장비 지수 데이터를 예측한 것.

```R
fcast <- forecast(fit, method="naive")
plot(fcast, ylab="New orders index")
```

[원문 보기](https://www.otexts.org/fpp/6/6)