---
layout: post
title: X-ray 및 기본 임상정보를 활용한 딥러닝기반 무릎 골 관절염 분류 연구
date: 2019-09-01 00:00:00 +0800
last_modified_at: 2020-02-28 00:00:00 +0800
tags: [knee-osteoarthritis, CNN, CAM, Inception-resnet-v2]
toc:  true
---

연세대학교 의과대학 생체공학협동과정 석사과정 연구 및 논문
{: .message }
## 개요

### 연구의 필요성
- 골 관절염은 무릎, 고관절, 손 또는 척추에 발생할 수 있는 질병이며, 특히 무릎의 골 관절염은 관절 사이의 간격이 좁아지고 통증을 수반함
- 노인 인구에서 가장 흔히 발생하는 만성 질환이며, 시기 적절한 치료를 하는것이 통증, 비용측면에서 효율적
- 지속적으로 환자수가 증가하고 있으며, 의료비용 또한 상당히 많이 발생함

### 연구의 목표
1. 골 관절염 발생 주요 원인 분석 (X-ray 영상 및 기본 임상정보)
2. X-ray 영상과 Convolutional Neural Network를 통해 무릎 골 관절염 위험도를 분류
3. 실제 임상적 특징을 반영하는지 확인
4. 기본 임상정보의 유무에 따른 분류 정확도 변화 확인

### 이론적 배경
- OARSI Score (Osteoarthritis Research Society International Score)
<dd>
  OARSI Score는 Osteoarthritis Research Society International(OARSI) 에서 권고하는 점수체계로서, 그림 4는 관절간격감소에 대한 정도를, 그림5는 골극의 형성 정도를 0(none)부터 3(Severe)까지 나타내는 점수
</dd>
![image](https://user-images.githubusercontent.com/82125326/138992468-96e6a4cf-c2bd-4d58-8001-b4d111276077.png)
![image](https://user-images.githubusercontent.com/82125326/138992471-fefa706f-c916-4486-aa18-bb5f20f6f984.png)

- Kellgren-Lawrence Grade (KL-Grade)
<dd>
  무릎 골 관절염의 심각도를 분류하는 점수체계로서, X-ray를 통한 무릎 골관절염의 심각도를 나타내는 수치이며 Grade 0(none)부터 Grade 4(Severe)까지 심각도를 구분하며, 높은 Grade일수록 무릎 골관절염이 악화된것으로 분류함
</dd>
![image](https://user-images.githubusercontent.com/82125326/138992346-d84ef06f-08d8-4be3-b9e4-bfbe24a73337.png)
![image](https://user-images.githubusercontent.com/82125326/138992358-906f9029-74bd-43dc-8b31-7342763c6966.png)

## 데이터 취득 
- OsteoArthritis Initiation (OAI) 
<dd>
  미국 국립 보건원 (National Institutes of Health, NIH)에서 운영하는 미국 국립 정신건강 연구소 데이터 아카이브 (National Institutes of Mental Health Data Archive, NIHM Data Archive)의 골관절염 이니셔티브 (OsteoArthritis Initiative, OAI) 데이터셋을 활용. OAI 데이터셋 취득을 위해서는 접근 권한 요청 및 인증과정이 존재함. **다기관에서 수집한 2004년~2014년까지 최초 스크리닝부터 108개월까지, 총 12회의 추적 정보가 존재하고, 기본적인 임상 정보, X-ray, MRI, Lab Test 결과 등의 다양한 데이터를 포함 하고 있으며, 45~79세의 남성, 여성 그리고 다양한 인종의 데이터를 포함하고 있음. 첫 스크리닝에서 총 4,796명의 환자가 존재하였으나, 차후 추적에서 참여 환자 수가 점차 감소**
</dd>

## 연구설계


## 전처리

