---
layout: post
title: SAZAPALZA, WINEAT 외주 프로젝트
date: 2021-03-01 00:00 +0800
last_modified_at: 2021-09-30 00:00:00 +0800
tags: [Masterpiece, Inception-resnet-v2, SRGAN, API]
toc:  true
---
주식회사 캐시고 재직 시 담당한 SAZAPALZA(시계분류 서비스), WINEAT(와인분류 서비스) 외주 프로젝트
{: .message }

## 연구기획
뇌졸중은
- 4대중증(심혈관질환,뇌혈관질환,중증외상,심정지)에 해당하며,
- 혈관이 막히는 뇌경색과, 혈관이 터져 발생하는 뇌출혈이 있음. 
- 발병 시 빠르게 치료를 받는것이 예후에 큰 영향을 미침. 

이에따라, 연구의 목표를 
- 모바일, 웨어러블 디바이스를 통회 데자동화된 측정도구로 안면마비(F), 사지마비(A), 음성장애(S)의 객관적이고 정확한 측정 및 분석을 연구하고, 
- 뇌졸중 치료의 기준이 되는 진단지수인 NIHSS (National Institute of Health for Stroke Scale), CPSS (Cincinatti Pre-hospital Stroke Scale), MRC (Medial Research Council) 예측을 통하여 환자 혹은 잠재적 환자들이 제한된 시간 내에 적절한 치료를 받을 수 있도록 지원하는 모바일 뇌졸중 진단용 모니터링 시스템을 개발하는 것을 목표로 함.

## 데이터 수집용 안드로이드 Application 개발

### 신경학적 이상증상 테스트
뇌졸중의 신경학적 이상증상 데이터 취득을 위하여, 태블릿에 탑재할 데이터 수집용 Android Application을 개발하였으며,
데이터 수집에 필요한 테스트는 신경과 전문의와 협의하여 총 10가지를 선정 하였음.
- (Arm)Pronation Drift (20s)
- (Arm)Rolling (Front, back each 10s)
- (Arm)Finger to Nose (each 15s)
- (Arm)Rapid Alternative Move (each 10s)
- (Leg)Heel to Shin (each 10s)
- (Both)Motor weakness (Arms 10s, Legs 5s)
- (Leg)Tandem gait (Fixed Distance)
- (Leg)One Leg Jump (each 3 times)
- Speech (specific sentence 2times)
- Face (Record 10s)

### 피험자정보 기입 및 센서 연결
- 피험자 이니셜,번호, 성별을 기록하고,
- 아래 Left ARM, Right Arm, Left Leg, Right Leg 버튼을 클릭하여 아래의 Shimmer Sensor들과 Bluetooth 무선연결.
- 연결 후 Enter 버튼을 클릭하면 테스트 메뉴로 진입.


### 테스트 선택
- 필요한 테스트를 선택하여 진입

### Strength 관련 테스트
- Start 버튼을 누르게 되면, 정해진 타이머와 함께 센서를 통해 값이 기록되고, 타이머가 종료되면 자동으로 측정 종료.
- 우측 카메라는 올바르게 측정을 진행하였는지 확인하기 위해 영상을 기록.

### Speech Recording
![placeholder](https://user-images.githubusercontent.com/82125326/138886588-a7f8116d-068a-4c55-b98f-6285162ce3e5.png "Large example image"){: .align-center}
- 피험자가 “대한민국의 가을은 참으로 아름답다” 라는 뇌졸중 진단용 문장을 2회 말할 수 있게 유도하고 Record 버튼을 클릭.
- 피험자가 말을 끝내면 Stop 버튼으로 종료.
- 테스트에 필요한 문장은, 마찬가지로 신경과 전문의와 협의하여 선정.

### Face Video Recording
- 눈썹과 이마주름이 선명하게 나올 수 있도록 웃을 수 있게 유도하고, 10초간 동영상으로 Recording.
- 정면영상을 촬영하며, 이후 동영상을 확인하여 기계학습에 최적인 Frame들을 사용.

### 데이터 저장 구조
- Tablet 내부의 저장소에 저장을 하게 되며, 환자 별 및 측정시간 별 구분하여 해당 환자에 대한 지속적 Follow up이 가능.
- 위 사진과 같이 센서 값은 Csv파일로 저장되며 각 해당 테스트가 올바르게 진행되었는지에 대한 동영상 저장.
- 파일명은 각각 테스트 별, 사지의 부위별로 저장.

## 관련 논문 작성

### Speech (with Praat, WEKA) - EMBC2017
1. 10개의 환자 음성샘플(남성5, 여성5) , 30개의 비 환자 음성샘플(남성5, 여성25)을 활용함
2. Praat V6.0.28을 통해 shimmer, jitter, pitch, HNR 등의 Feature를 추출함
3. Information gain, wrapper subset evaluation 방법을 통해 영향이 큰 7가지의 feature만을 사용함
4. 선택된 feature들을 WEKA V3.8 의 Decision Tree, Random forest, Support Vector Machine을 통해 분류함
5. SVM with Polynomial kernal 모델이 Best로, ACC 97.5%, AUC 0.997을 달성함

### PD Test - EMBC2018
1. 여러 Strength Test중 PD(Pronation Drift) Test를 선정함
2. 2명의 환자, 3명의 비환자의 데이터 샘플을 활용함
3. bias를 제거하기위해 각 데이터의 mean값을 subtracting 하였음
4. 주요 feature는 센서의 X축, Y축, Z축의 가속도 값의 Euclidean norm 을 사용함
5. 이를통해 4가지의 feature를 생성함 (얼마나 동일한속도로 움직이는지, 양손이 조화롭게 움직이는지)
6. 비 환자의경우, 양 손이 비교적 동일한 속도로, 조화롭게 움직였으나, 환자의 경우 양손중 문제가 있는쪽이 비교적 느리고 조화적이지 못하였음
