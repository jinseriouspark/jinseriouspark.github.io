---
layout: post
title: "A recipe for training neural networks 신경망 학습 가이드(3)"
author: "Seriouscoding"
---

오역이 있을시 말씀주시면 수정하겠습니다.

**원제:  recipe for training neural networks** [post url](http://karpathy.github.io/2019/04/25/recipe/)

**저자: Andrej Karpathy**


______________________________________________________________

2. Set up the end-to-end training/evaluation skeleton + get dumb baseline

이제 우리의 데이터를 이해했으므로, super fancy Multi-scale ASPP FPN ResNet (엄청 멋진 멀티 스케일 ASPP FPN ResNet)에 접근해서 멋진 모델 학습을 시작할 수 있을까요? 확실히 아닙니다. 아직 고난의 행군 중입니다. 우리의 다음 단계는 전체 학습 + 얼개 평가를 위한 셋팅이고, 몇개의 연속된 실험을 통한 그 정확성에 신뢰를 얻는 것입니다. 이 단계에서는 몇가지 단순한 모델에서 최고를 뽑는데, 이 모델들은 어떤 짓을 하더라도 망칠 가능성이 없는 것들입니다. 예를들어 선형 분류기나 매우 작은 ConvNet 같은 것 말입니다. 우리는 이를 학습하고, 오차나 다른 모든 메트릭(예: 정확도) 를 시각화 하고, 모델 예측을 수행하는 과정에서 명백한 가설을 사용하여 일련의 절제 실험을 수행하려고 합니다. 

이 단계에서의 tip 과 trick 들은 다음과 같습니다. 

- fix random seed 랜덤 시드 고정

    언제나 고정된 랜덤 시드를 사용하는 것은 여러분들이 코드를 2회 사용할 때 동일한 결과가 나오게 하는 것을 보장합니다. 이는 변동을 주는 요소를 제거해주고, 여러분들이 미치지 않게끔 도와줍니다. (keep you sane)

- simplify 단순화

    멋지지만 불필요한 것들(any unnecessary fanciness)을 사용하지 못한다는 것을 확실히 하세요. 예를 들어서, 이번 단계에서 데이터 증강을 시도하지 마십시오 (turn off any data augmentation). 데이터 증강은 정규화 전략이며, 이는 이후에 사용할 것입니다. 지금은 그냥 몇가지 멍청한 버그를 소개하는 기회일 뿐입니다. 

- add significant digits to your eval eval에 유효한 숫자 추가

    테스트 오차 그림을 그릴 때, 전체 (큰) 테스트셋에 대한 평가를 실행하게 됩니다. 그때, 각 배치마다의 테스트 손실을 그리지 말고, Tensorboard의 smoothing에만 의존하지 마세요. 우리는 정확성을 추구하며, 제정신을 유지하기 위해 시간을 기꺼이 포기해야 합니다.

- verify loss @ init 초기화 시 loss 확인

    정확한 loss value 에서 여러분의 loss 가 시작했다는 것을 꼭 확인하세요. 예를들어, 만약 여러분의 최종 레이어를 정확하게 초기화 했다면, 여러분은 초기화 과정에서 소프트맥스에 대해 `log(1/n_classes)` 를 계산해야 합니다. 이 동일한 기본값은 L2 회귀, Huber losses 에서도 도출할 수 있습니다.

- init well 잘 초기화하기

