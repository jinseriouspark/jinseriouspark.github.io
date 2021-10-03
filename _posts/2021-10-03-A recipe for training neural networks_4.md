---
layout: post
title: "A recipe for training neural networks 신경망 학습 가이드(3)"
author: "Seriouscoding"
---

오역이 있을시 말씀주시면 수정하겠습니다.

**원제:  recipe for training neural networks** [post url](http://karpathy.github.io/2019/04/25/recipe/)

**저자: Andrej Karpathy**


______________________________________________________________

3. Overfit
    
    이번 단계에서, 우리는 데이터셋에 대한 좋은 이해가 필요하며, 전체 학습 + 평가 파이프라인을 다룰 것입니다. 우리가 신뢰할 수 있는 metric 을 (재현 가능하게) 계산할 수 있는 어떤 모델이든 말입니다. 우리는 인풋-독립적인 베이스라인에 대한 성능, 몇 가지 바보같은 베이스라인의 성능 (이것보단 잘해야 함)을 가지고 있으며, 사람 평가(실제로 도달하고 싶은 목표) 에 대한 대략적인 감각도 있습니다. 이제 좋은 모델을 반복하기 위한 단계를 모두 갖췄습니다.