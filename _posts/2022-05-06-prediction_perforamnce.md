---
layout: post
title: "주식 매매 모델의 성능 평가"
date: May 6, 2022
categories: analysis
---

이전에 소개한 [코스피 예측 모델]({{ site.baseurl }}{% post_url 2022-05-03-kospi-prediction_perforamnce %})을 사용하여 다수 종목들의 한달 뒤 주가를 예측하고 그중 상승률 상위 종목들을 매수하는 모델(이하 GBM)에 대하여 무작위 매매 모델(Random)과 성능을 비교한다.
- Random은 한달 뒤 개별 종목들의 주가가 현재가 대비 무작위로 변화한다고 예측 (각 종목들의 변화율은 서로 다른 정규분포를 가정).
- 예측 성능에 대한 평가 지표는 과거 17년간 주가에 대하여 예측값과 실제값의 일일 상관 계수(Information Coefficient), 한달 뒤 주가의 상승/하락 여부 예측에 대한 정밀도와 재현율(precision & recall).
- 모의투자(backtesting)를 시행하여 각 모델의 수익률을 비교하고, 베이즈 추론(Bayesian inference)을 통해 GBM은 Random보다 통계적으로 유의미하게 우수한가를 검토.

#### Information Coefficient
일일 IC의 평균은 Random의 경우 예상대로 0에 가깝고, GBM은 그보다 높은 IC 수준을 보이지만 월별 편차가 크다.

![ic_comparison]({{ site.baseurl }}/assets/2022-05-06-ic_comparisonv2.png)

#### Precision & Recall
GBM은 주가 상승 대부분을 포착하지 못했지만(낮은 재현율), 일단 상승을 예측한 경우에는 실제로 상승한 경우가 90%를 상회(높은 정밀도). Random의 정밀도는 50%를 하회.

| | GBM | Random |
| :-: | :-: | :-: |
| recall of rise | 0.009 | 0.386 |
| precision of rise | 0.934 | 0.489 |

#### Backtesting
다음의 규칙에 따라 주식을 매매한다고 가정하여 두 모델의 과거 17년 동안의 성과를 비교.
- 한달 뒤 상승을 예측한 종목에 대하여 종목당 예산 한도 내 최대 수량만큼 매수한 후, 기준치 이상 상승시 전량 매도하되 한달 안에는 무조건 매도.
- 보유중인 종목은 추가 매수하지 않고 반드시 매도 후 예측 결과에 따라 신규 매수 시행.
- 보유 종목수는 일정 한도로 제한

GBM은 상대적으로 매우 적은 매수 횟수(적은 투자액)로 인하여 Random 보다 수익 액수는 작을 수 있지만 수익률은 월등히 크다.

| | GBM | Random |
| :-: | :-: | :-: |
| number of buy | 77 | 3573 |
| win rate | 0.88 | 0.48 |
| return (simple rate) | 49% | 2.5% |

매매당 수익률은 GBM이 더 우수한 것으로 보인다.

![return_histogram]({{ site.baseurl }}/assets/2022-05-06-ic_return_hist_v3.png)

베이즈 추론을 통해 매매당 수익률을 비교해보면 GBM의 평균 및 변동성 대비 수익률 모두 Random 보다 유의미하게 큰 것을 확인할 수 있다. (베이즈 추론 참고: [Bayesian estimation supersedes the t-test], John Kruschke, Journal of Experimental Psychology, 2012.)

![return_bayesian]({{ site.baseurl }}/assets/2022-05-06-bayesian_returns2_all_220507_v3.png)

[Bayesian estimation supersedes the t-test]: https://psycnet.apa.org/record/2012-18082-001 "Bayesian estimation supersedes the t-test"



