---
layout: post
title: "코스피 예측 모델의 성능 평가"
date: May 3, 2022
categories: analysis
---

<script>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}
  .table {
    width: 100%
    }
</script>
    

한달 뒤 코스피를 예측하는 그래디언트 부스팅 모델(GBM)의 성능을 평가하기 위하여 무작위 예측 모델(Random)과 비교.
- Random은 한달 뒤 코스피 지수가 현재 대비 무작위로 변화한다고 예측 (변화율은 정규분포를 가정).
- 예측 성능에 대한 평가 지표는 과거 17년간 코스피 지수에 대하여 예측값과 실제값의 월단위 상관 계수(Information Coefficient), 한달 뒤 코스피의 상승/하락 여부 예측에 대한 정밀도(precision)와 재현율(recall).
- 각 모델의 예측 결과로 모의투자(backtesting)를 시행하여 수익률을 비교하고, 베이즈 추론을 통해 GBM은 Random보다 통계적으로 유의미하게 우수한가를 검토.

#### Information Coefficient
월단위 IC의 평균은 Random의 경우 예상대로 0에 가깝고, GBM은 그보다 높은 IC 수준을 보이지만 월별로 큰 편차를 보인다.

![ic_comparison]({{ site.baseurl }}/assets/2022-05-03-ic_comparison2.png)

#### Precision & Recall
GBM은 코스피 상승의 대부분을 포착하지 못했지만(낮은 재현율), 일단 상승을 예측한 경우에는 실제로 상승한 경우가 Random에 비해 많았다(높은 정밀도). 참고로 Random의 정밀도가 50%를 넘는데 역사적으로 코스피가 상승 추세였기 때문에 상승/하락의 예측 확률이 동일하다면 50%를 넘는 예측 정밀도를 보일것.

| | GBM | Random |
| :-: | :-: | :-: |
| recall of rise | 0.121 | 0.483 |
| precision of rise | 0.702 | 0.590 |

#### Backtesting
예측 결과를 기반으로 다음의 원칙에 따라 코스피 지수를 매매한다고 가정하여 두 모델의 과거 17년 동안의 성과를 비교.
- 한달 뒤 상승을 예측하면 1주를 매수한 후, 기준치 이상 상승시 매도하되 한달 안에는 무조건 매도.
- 보유중에는 추가 매수하지 않고 반드시 매도 후 신규 매수 시행.

GBM은 Random에 비해 매우 적은 매수 횟수와 절반 미만의 수익률을 보이지만 수익이 발생한 매매 비율은 조금 더 높다.
GBM은 Random에 비해 수익이 발생한 매매 비율이 높지만, 상대적으로 매우 적은 매수 횟수로 최종 수익률은 더 낮다.

| | GBM | Random |
| :-: | :-: | :-: |
| number of buy | 67 | 623 |
| win rate | 0.66 | 0.54 |
| return (simple rate) | 107% | 247% |

GBM의 

Bayesian estimation supersedes the t-test, John Kruschke, Journal of Experimental Psychology, 2012.

![return_bayesian]({{ site.baseurl }}/assets/2022-05-03-bayesian_returns2_kodex_220502.png)

