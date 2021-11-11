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

  OARSI Score는 Osteoarthritis Research Society International(OARSI) 에서 권고하는 점수체계로서, 그림 4는 관절간격감소에 대한 정도를, 그림5는 골극의 형성 정도를 0(none)부터 3(Severe)까지 나타내는 점수

![placeholder](https://user-images.githubusercontent.com/82125326/138992468-96e6a4cf-c2bd-4d58-8001-b4d111276077.png "Large example image"){: .align-center}
![placeholder](https://user-images.githubusercontent.com/82125326/138992471-fefa706f-c916-4486-aa18-bb5f20f6f984.png "Large example image"){: .align-center}

- Kellgren-Lawrence Grade (KL-Grade)

  무릎 골 관절염의 심각도를 분류하는 점수체계로서, X-ray를 통한 무릎 골관절염의 심각도를 나타내는 수치이며 Grade 0(none)부터 Grade 4(Severe)까지 심각도를 구분하며, 높은 Grade일수록 무릎 골관절염이 악화된것으로 분류함
  
![placeholder](https://user-images.githubusercontent.com/82125326/138992346-d84ef06f-08d8-4be3-b9e4-bfbe24a73337.png "Large example image"){: .align-center}
![placeholder](https://user-images.githubusercontent.com/82125326/138992358-906f9029-74bd-43dc-8b31-7342763c6966.png "Large example image"){: .align-center}

## 데이터 취득 
- OsteoArthritis Initiation (OAI) 

  미국 국립 보건원 (National Institutes of Health, NIH)에서 운영하는 미국 국립 정신건강 연구소 데이터 아카이브 (National Institutes of Mental Health Data Archive, NIHM Data Archive)의 골관절염 이니셔티브 (OsteoArthritis Initiative, OAI) 데이터셋을 활용. OAI 데이터셋 취득을 위해서는 접근 권한 요청 및 인증과정이 존재함. **다기관에서 수집한 2004년~2014년까지 최초 스크리닝부터 108개월까지, 총 12회의 추적 정보가 존재하고, 기본적인 임상 정보, X-ray, MRI, Lab Test 결과 등의 다양한 데이터를 포함 하고 있으며, 45~79세의 남성, 여성 그리고 다양한 인종의 데이터를 포함하고 있음. 첫 스크리닝에서 총 4,796명의 환자가 존재하였으나, 차후 추적에서 참여 환자 수가 점차 감소**

## 선행연구
- Feature Selection

  1. 무릎 골 관절염 진단에 사용되는 특징들 중 Correlation이 높은 특징들만 사용하여 Demension을 줄이기 위해 연구를 진행 
  2. 임상의의 조언에 따라 임상특징인 나이, 최고혈압, 최저혈압, BMI, 가족력, 성별을 채택
  3. 더불어 X-ray에서 얻을 수 있는 JSN(무릎관절간격감소정도), OS(골극), 경화증여부(SC), 석회증여부(CH), 낭종여부(CY), 연골마모정도(ATT)를 채택
  4. 카이제곱특성평가(Chi-Squared), 상관관계특성평가(Correlation), 정보획득 특성평가(Information Gain), 대칭불확실성 특성평가(Symmetric Uncertainty) 로 비교
  5. 이후 선정된 특징들을 통해 분류 정확도 비교 (CNN 없이, X-ray 특징 Label만을 통해)

- 결과

  1. 모든 방법에서 임상정보보다 X-ray에서 취득가능한 정보가 영향이 컸으며,
  2. 전반적으로 Lateral(몸의 중심에서 먼)부분 보다, Medial(중심쪽)의 이상징후가 영향이 컸으며,
  3. 모든방법에서 JSN이 최상위권에 위치하였으며,
  4. JSN 및 기본임상정보만으로도 Random-Forest 방법으로 82%의 Accuracy로 KL-Grade를 분류하였음
  5. 자세한 내용은 [논문](https://ir.ymlib.yonsei.ac.kr/handle/22282913/179009) 확인 바람

## 연구설계
- 선행연구 결과에 따라, CNN을 통해 X-ray에서 JSN정보를 추출하고, 임상정보를 더해 최종적으로 KL-Grade를 분류하는 구조를 설계

![placeholder](https://user-images.githubusercontent.com/82125326/141040509-6c45efb5-3c63-4190-9710-29e388ef85a6.png "Large example image"){: .align-center}

### Non clinical input
![placeholder](https://user-images.githubusercontent.com/82125326/141040350-2a83498b-255b-4a85-94f9-badfc5ed2a2a.png "Large example image"){: .align-center}

### Include Clinical input
![placeholder](https://user-images.githubusercontent.com/82125326/141040359-2a77d144-7c31-479a-b74c-4c33f1b7398c.png "Large example image"){: .align-center}


## 전처리

### 좌우 및 필요부분 분리
- 가로, 세로가 길이가 다른 데이터를 같게 만들기 위해, 가로, 세로 중 부족한부분을 Zero padding 진행 

![placeholder](https://user-images.githubusercontent.com/82125326/141220621-edd284cb-9209-44ec-9896-034d71c8d870.png "Large example image"){: .align-center}
- 일부 데이터가 손실되지 않게 위해 각각 [0,w/2+w/10], [w-w/2-w/10]로 cropping

![placeholder](https://user-images.githubusercontent.com/82125326/141220712-db70aedf-fe10-4f7b-9255-f6585616bfab.png "Large example image"){: .align-center}
- 실제 필요한 부분의 이미지는 무릎의 간격 부분이므로, 1차적으로 U-net으로 간격부분을 Segmentation 진행하였음

![placeholder](https://user-images.githubusercontent.com/82125326/141220889-ade86cea-af40-4598-8812-24ff997382f0.png  "Large example image"){: .align-center}
- Segmentation은 잘 이루어졌으나 주위 골격의 크기를 비교해야 하므로 segmentation이미지의 중심부를 기준으로 상 150pix, 하 150pix, 좌 300pix, 우 300pix을 cropping 하여 가로 600pix, 세로 300pix의 이미지를 생성

![placeholder](https://user-images.githubusercontent.com/82125326/141220982-5f4ef58b-0323-4a95-8740-fed0934e03bf.png "Large example image"){: .align-center}
- 이후 좌/우 무릎의 이미지에 따라 Medial JSN (JSNM), Lateral JSN(JSNL)로 이미지 분리


### Contrast 전처리
- 이미지 별 Contrast가 다양하여, 다음과 같이 Contrast Equalization을 진행 하였음
![placeholder](https://user-images.githubusercontent.com/82125326/141221268-0568b1c2-cdf2-4ada-b0a1-0883ada5b572.png "Large example image"){: .align-center}

## 결과
### Non clinical input
![placeholder](https://user-images.githubusercontent.com/82125326/141040752-d7020f3b-2781-4e94-8cd6-1a7495e85233.png "Large example image"){: .align-center}

### Icnlude Clinical input
![placeholder](https://user-images.githubusercontent.com/82125326/141040768-89df59de-8d48-400a-ab66-128d1c278a0f.png "Large example image"){: .align-center}
