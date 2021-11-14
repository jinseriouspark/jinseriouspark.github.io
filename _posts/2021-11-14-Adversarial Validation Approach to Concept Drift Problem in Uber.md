---
layout: post
title: "Uber의 유저 타겟팅 자동화 시스템의 Concept drift 문제를 해결하기 위한 적대적 검증 접근"
author: "Seriouscoding"
---

논문 학습 후 요약한 내용이며, 잘못된 부분에 대한 피드백은 언제나 환영합니다.

**원제: Adversarial Validation Approach to Concept Drift Problem in
User Targeting Automation Systems at Uber** [post url](https://arxiv.org/abs/2004.03045)

**저자: Jing Pan, Vincent Pham, Mohan Dorairaj, Huigang Chen, Jeong-Yoon Lee**


______________________________________________________________


    
1) 어떤 문제를 풀려고 하였나? 
    
    학습/ 검증 데이터를 가지고 학습 후, 테스트데이터에 적용하고자 할 때, 테스트데이터와 분포가 달라  concept drift 하는 경우가 존재하였음
    
2) 어떤 솔루션을 제시했나?
    
    메인 결과
    
        1) 자동 피쳐 선택
    
        - train - test 의 auc  가 0.5 이상일 경우, 그 구분을 잘하게 하는 피쳐를 삭제하고 다시 auc 계산, auc 가 0.5 이하가 되면 중단
        - 그 구분을 잘하게 하는 피쳐 삭제 기준은 MDI(mean decrease impurty) 나 상위 x% 등으로 threshold 지정
    
        2) 검증 데이터 선택
    
        - train, test 셋에 학습시키고 각 데이터셋에 대한 ㅣpropensity score 를 계산한 후, SMD 점수를 체크
        - SMD 점수는 (E[X_match] - E[X_test]) / (sqrt(1/2var[X_match] - var[X_test]))
    
        3) 역 propensity score 가중치 적용
    
        - w = P_propensity / (1 - P_propensity)
        - 위의 가중치를 학습데이터셋에 적용하여 가중치 적용된 학습을 가능하게 함
    
    솔루션 장점
    
        - concept drift 를 낮출 수 있도록 train, valid 데이터로 학습이 가능, 모델이 더 robust 해짐
    
    솔루션 단점
    
        - 따로 등장하지 않음

3) 기존의 다른 솔루션은 무엇인가?

    과거 솔루션은 따로 안내되지 않았으나, 차용한 개념 2가지를 소개

    - GAN
        - 2개의 뉴럴넷으로 구성 : generator, discriminator
            - generator 역할 : 새로운 데이터를 학습셋과 비슷하게 만듦
            - discriminator 역할 : 오리지널 데이터와 새롭게 생성기가 만든 데이터 간에 구분
        - adversarial validation 도 2개로 구성 : target variable 예측하는 학습기, train - test 분류기
    - propensity score (in Causal inference)
        - propensity score 를 주요 예측 모델에 사용하여, test set bias 를 낮추고자 함

4) 남은 부분 이해를 위한 추가 자료

    - 추가 생각해볼 것
        - concept drift 를 완화하기 위한 검증 프로세스의 하이퍼 파라미터 최적화 설계 방식
            - 벡테스트를 진행
        - IPW의 노이즈를 감소시키기 위해 피쳐를 조합하는 방법을 찾는 게 필요함
        - adversarial detection / estimation 방법이 전체 진행에서 중요