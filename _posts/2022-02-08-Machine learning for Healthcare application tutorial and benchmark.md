---
layout: post
title: "의료업종 적용을 위한 기계학습을 사용한 이종 치료 효과 (HTE) 추정: 튜토리얼과 벤치마크"
author: "Seriouscoding"
---

학습을 위한 논문 번역이며, 잘못된 부분에 대한 피드백은 언제나 환영합니다.

**원제: AMachine learning for Healthcare application tutorial and benchmark** [post url](https://arxiv.org/abs/2109.12769)

**저자: Yaobin Ling, Pulakesh Upadhyaya, Luyao Chen, Xiaoqian Jiang, Yejin Kim**


______________________________________________________________


### 요약

**배경:** 표적 질환에 대한 신약 개발은, 시간과 비용이 많이 드는 작업이기에, 약물 용도 변경 (drug repurposing) 은 약물 개발 분야에서 인기있는 화두가 되었습니다. 많은 건강 강조 데이터를 사용할 수 있게 되면서, 데이터에 대한 많은 연구가 수행되었습니다. 실제 데이터는 훨씬 더 오류가 많이 껴있고, 희소하며, 많은 혼란 요소를 가지고 있습니다. 또한 많은 연구에서 '약물 효과는 인구 간에 이질적이라는 점을 보여주었습니다. 이질적 치료 효과(HTE) 추정에 대한 여러 고급 기계 학습 모델이 최근에 등장하였으며, 계량 경제학 및 기계 학습 커뮤니티에 적용되었습니다. 이러한 연구는 의학 및 약물 개발을 주요 응용 분야로 인정하고 있으나, 전통적인 HTE 방법을 의약 개발 분야에 적용하는데에 한계를 가지고 있습니다.

**목표 :** HTE 방법론을 의료 분야에 도입하고, 의료 행정 청구 데이터에 대한 벤치마크 실험을 통해, 의료 연구에 적용할 때 모델을 해석하고 평가하는 방법을 보여주고자 합니다.

**범위** : 통계, 기계 학습 및 경제학 커뮤니티 독자를 위해 HTE 방법론을 적용할 때, 대규모 claim 데이터 및 장기 진행성 질병의 예시와 몇 가지 실제 제약 조건을 보여주고자 합니다. 우리는 약물 또는 치료법의 효과를 연구하는 HTE 추정 의료 도구의 실행 가능성을 의료 및 약물 개발 독자들에게 제공하는 것을 목표로 합니다.
**기여** : 생물 의학, 정보학 커뮤니티의 광범위한 독자층에게 최신 HTE기술을 도입함으로써 기계 학습으을 사용한 인과 추론의 광범위한 선택(채택, adoption) 을 촉진할 것으로 예상합니다. 우리는 또한 개인화된 약물 효과를 위한 HTE의 이용 가능성을 제공할 것도 기대합니다.

**키워드**

튜토리얼, 이종 치료 효과(HTE), 기계 학습, 알츠하이머, 염증, 약물 효과, 표적 시험, meta-learner, 균형 학습, 딥러닝

Tutorial, Heterogeneous treatment effects, Machine learning, Alzheimer’s disease,
Inflammation, Drug effectiveness, Target trials, Meta-learners, Balanced learning, Deep learning

### Introduction

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fa6d6ab-973e-40ce-af5a-b28760af269a/Untitled.png)

**그림 1.** 약물 Montelukast 과 알츠하이머 병의 결과를 설명한 것. (a) Monteluast 가 알츠 하이머의 발병 및 진행을 줄이는데에 정말 치료 효과가 있었나요? (b) 무작위의 클리닉 시도(RCTs) 은 무작위 대조군 할당 (randomizing treatment assignment) 과 치료 효과의 평균화에 의해 그러한 치료 효과를 평가하는 사실상의 황금 기준입니다. 치료 효과 평가는 실제 세상 관찰 데이터에 대비하여 RCT를 시뮬레이션 하는 것입니다. (c) 치료 효과는 환자의 특성에 따라 다릅니다.

**Causal effect of treatment** Treatment의 인과적 영향
인과 분석은 특정  문제의 '근본' 원인을 식별하는 어떤 효과의 원인을 발견합니다. 무작위된 클리닉 시도(RCTs)들은 treatment 할당을 무작위화 하여 개입을의 인과 관계를 식별하는 황금 스탠다드 입니다.  그러나 이러한 CT 는 때때로 경제적, 윤리적 문제로 사용할 수 없거나 굉장히 비싼 비용을 요구합니다.

**Montelukast and Alzheimer's disease example**
병의 예방과 치료에 대한 약의 인과 효과에 대해 생각해보겠습니다. Montelukast 만성 염증에 대한 항염증제입니다. 알츠하이머  병은 뇌 세포가 사멸하는 진행성 신경 장애 입니다. 만성 염증은 알츠하이머 병과 관련이 있습니다. [1,2] 일부 연구자들은 Montekalust가 만성 말초 염증을 감소시킬 뿐 아니라, 알츠하이머병 병리의 다양한 측면을 매개하는  leukotiene 신호 전달 경로를 타겟팅 함으로써, 알츠하이머병 위험을 줄일 수 있다고 추측하고 있습니다. [4] 실제로 알츠 하이머 병에 대한 Montelukast 의 인과적 효과(여기서는 치료 효과) 를 조사하기 위한 phrase 2(2상) 무작위 임상 실험이 진행 중입니다(그림 1) [5]

**Simulating randomized clinical trials** 랜덤화된 RCTs
그러나, 1상 및 2상 임상 시험의 평균 비용은 미국에서 각각 400만 달러 및 1300만 달러이며[6], 알츠하이머 치료 시험의 실패율은 99%입니다.[7] 실제 시험에 앞서 무작위 임상 시험을 시뮬레이션 하는 것은 높은 신뢰도의 가설을 생성하여 실패율을 줄이는 비용 효율적이고 안전한 도구가 될 수 있습니다. Treatment effect 추정이나, Counterfactual 추론 혹은 인과 추론은 실제 관찰을 활용하고 treatment 효과를 소급 추론하는 것입니다. 실제 관찰 데이터를 사용한 treatement effect 추정은, 개인이 노출되지 않았을 때의 대안적 노출 시나리오 하에서  특정 treatment 에 노출된 개인의 결과를 추정하는 것을 목표로 하고 있습니다.

**Real-world data is biased** 실제 데이터는 편향되어있습니다.
treatment 효과를 식별하는 것은 데이터의 편향성 때문에 매우 어렵습니다. Montelukast 와 알츠 하이머 질환 간의 관련성을 계산하고, 통계학적으로 유의한 높은 연관성을 얻는다고 가정해보겠습니다. 여기서 보이는 높은 관련성은 알츠하이머 병에 대한 Montelukast 의 직접적인 예방 효과 때문이 아니라 만성 염증 감소에 대한 Montelukast 의 항염증 효과 때문일 수 있습니다. 우리는 treatment와 결과에 대한 노출을 유발하는 변수를 나타내기 위해 교란변수(confounding variables) 라는 용어를 사용합니다. Montelukast의 예시에서 만성염증(chronic inflammation)은 교란변수 입니다. treatment 가 결과에 영향을 미치지 않을 때, 결과에 영향을 미쳐 잘못된 결론을 내리도록 오도할 수 있는 편향의 주요 원인 중 하나입니다.

**Debias the data and estimate treatment effect**  데이터를 편향시키고 treatment 효과를 추정.
그래서, treatment effect 추정은 treatment 에 대한 노출에 영향을 미치는 조건의 분포를 조정하여, 이러한 편향을 줄이는 것을 목표로 합니다. 의약 분야의 관찰 데이터로부터 인과관계를 추정하기 위한 통계적 추론은 역병학(epidemiology) 및 생물 통계학의 대상 시험(target trial)으로 오랫동안 논의되어 왔습니다. Target trial 접근 방식은 무작위의 클리닉 시도를 에뮬레이트 하는 프레임워크를 제안합니다.[15, 17, 18] 이 프레임워크는 무작위 클리닉 시도(randomized clinical trials) 기준선 특징을 조정(control)하고, 결과에 대한 time zero(baseline)을 식별하고 일치(matching) 또는 가중치를 부여(weighting)하여 교란 요인(confounders)을 적용하면서 시뮬레이션 합니다. (그림 2) [17,18]. 많은 머신 러닝 예측 방법은, 연관성이 인과 관계를 의미하지 않는 다는 사실에도 불구하고, 연관성에 기반하여 개발되었습니다.

**Treatment effects vary Treatment** 효과가 다양함
모든 알츠하이머병이 동일하지 않습니다. 알츠하이머 병은 다양한 임상 증상 (예를 들어 신경염, 혈관/대사 위험 인자, 신화 스트레스, 단백질 잘못접힘, 신경 전달물질) 를 발생시키는 다양한 병리를 가지고 있습니다. 알츠하이머 발병과 진행에 대한 Monelukast 의 treatment effect는 환자의 병리현상에 따라 다양합니다. 신경염(neuroinflammation)이 있는 환자는 신경염이 없는 환자보다 항염증(anti-flammatory) treatment의 혜택을 더 많이 받을 수 있습니다.[2] 뇌에 아밀로이드 플라크가 있는 환자는 아밀로이드가 없는 환자보다 아밀로이드 감소 치료의 혜택을 받을 가능성이 큽니다. [19] Montelukast 의 평균 치료 효과를 추정하는 것은 각 환자가 가지고 있는 이 이질적인 병리를 무시합니다. 환자의 이질성을 고려하여 개인화된 치료를 가능하게 하기 위해서는, 각 개인 또는 환자의 유사한 하위 그룹에 대한 치료 효과를 주장하는 것이 매우 중요하며, 이를 이질적 치료 효과(HTE) 라고 합니다. 조건부 평균 치료 효과는 종종 이종 치료 효과 또는 개별 치료 화가와 혼용하여 사용됩니다.

**Use machine learning to represent nuisance** 기계학습을 사용하여 nuisance를 표현
대부분의 경우, 치료 효과에서 어떤 특징이 이러한 이질성을 만드는 지는 알려져 있지 않습니다. 연구자들은 treatment effect 가 다른 것들과 구별되는 일련의 특징들을 식별하기 위해 특징 공간을 탐색할 필요가 있습니다. 기능 공간(feature space)에서, feature 들의 조합은 기하급수적으로 증가하고 있습니다. (예를들어, 10개의 이진 feature는 2^10 개의 조합을 의미합니다) features 의 고차원을 주장하기 위해, Chemozhukov et al.[20] 은 첫번째로 반-모수적 (semi-parametric) nuisance 를 임의의 지도 머신 러닝 모델로 대체할 것을 제안합니다. 이를 더블 머신러닝(Double machine learning) 이라고 부릅니다. 이는 임의의 머신러닝 모델을 사용하여, 개별적인 문맥의 nuisance 를 학습하고, 잠재적 결과와 편향을 줄이는 데에 사용됩니다. 그 후, 통계 및 증강된 역-성향 가중치(augmented inverse propensity weighting) (또는 이중 robust 학습), tree-based learning, meta learners, balanced representation learning 과 같은 것을 사용하여, HTE를 추정하려는 여러 노력들이 있었습니다.

**이번 튜토리얼에서**, 우리는

1. HTE를 추정하기 위한 최근 조사와, 더 나은 모델링 선택을 위한 비교를 할 것입니다
2. 알츠하이머 병에 대한 구체적인 예시와 함께 6천 만명의 환자 기록이 있는 전국 청구 데이터에 HTE 방법을 적용하여 HTE 방법의 타당성을 테스트할 것입니다.
3. 의료 관리 데이터에서 HTE 방법의 잠재력, 과제 및 현재 제한 사항에 대해 논의합니다.

**Target readers and scope** 대상 독자 및 범위
우리의 튜토리얼과 벤치마크는 특히 의료 관리 데이터를 사용하는 중계적 생물의학 관점에서 작성되었습니다. 방법론에 대한 이론적 비교 및 벤치마크는 [27, 28] 을 참조하십시요. 계량 경제학적 관점은 튜토리얼 논문[29]를 참조하세요. 임상 약리학 관점에 대해서는 튜토리얼 논문[30] 을 참조하세요. 이 튜토리얼 문서는 econml[31], causalml[32], Kunzel et al[24], Bica et al[30] 을 포함한 다양한 소스의 표기법과 네이밍 전략을 적용하였습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26215432-cc83-4395-8396-e7353d8bbc20/Untitled.png)

**그림2** 평균 치료 효과 추정(a,b) 무작위 임상 실험과 실제 관찰 비교. (a) RCT의 피험자는 치료군과 미치료군에 무작위로 할당되므로, 유사한 분포를 따릅니다. (b) 실제 데이터의 피험자는 치료군과 대조군에 무작위로 할당되지 않습니다. 교란 요인(confounders)에 의해 일부 환자는 치료를 받을 확률이 매우 높을 수 있습니다.


