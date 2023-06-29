# Diagnosis_dead_skin_cell

## Abstract
촬영한 사진을 딥러닝 모델로 학습시켜 병의 유무와
중증도를 판단하는 기술은 기존 의료기기보다 성능,
효율, 그리고 질적인 측면을 높일 수 있는 특징이 있다. 

본 논문에서는 딥러닝 모델을 기반으로 사람 두피의 미세각질 질환 심각도를 자동으로 분류하는 방법을
제안한다. 

AI HUB에서 제공하는 두피 미세각질 데이터셋을 전처리하여 사용하였고, 
환자의 두피의 미세각질 상태를 양호, 경증, 중증도, 중증까지 총 4단계로 나누어 분류할 수 있는 모델을 설계한 뒤 학습하였다.

학습 모델로는 IRSVRC에서 1등 모델을 응용한
Xception모델을 사용하였고 훈련 결과 정확도(Accuracy) 95..48%, 정밀도(Precision) 91.08%, 
재현율(Recall) 90.82% 의 성능을 보이는 것을 확인하였다


### I. 서론
딥러닝을 활용한 의료 진단 기술은 IT 인프라의 발전과 인공지능 기술 발전에 힘입어 매우 높은 
정확도와 범용성을 가진 기술로 발전하였다.

최근 여성뿐만 아니라 남성의 두피와 모발에 대한
관심이 커지고 있다. 

두피의 모발에 대한 문제는 단순히 치료의 대상으로 보는 수준을 넘어 건강한 두피,
매력적인 머릿결을 갖기 위한 능동적인 노력이 점차
활성화 되고 관련 시장도 성장추세에 있다. 

이에 따라서 두피 건강을 쉽고 빠르게 진단할 수 있는 프로그램 모델이 필요한 상황이다.

본 논문은 사람 두피의 상태 심각도를 진단하기 위해 AI HUB에서 제공하는 두피 미세각질 데이터셋을 사용하여 두피의 상태를 양호, 경증, 중증도, 
중증으로 나누어 분류하는 훈련 과정과 결과에 대한 분석에
초점을 맞춰 기술한다. 

두피 미세각질에 대한 샘플 이미지는 아래와 같다.

<img src="https://github.com/heeManLee/Diagnosis_dead_skin_cell/assets/90524127/9009458c-6065-455b-8eae-92013ba7160a"  width="400" height="200">

그림 1. 두피 미세각질 샘플 이미지
(위쪽 좌측부터 양호, 경증, 중증도, 중증)

### II. 관련 연구
학습된 AI를 활용하여 병을 진단하기 위한 연구는
활발하게 진행되고 있다. 

그 중 대표적인 연구로 이희도외 3인이 진행하였던 『딥러닝 기법을 이용한 미숙
아 망막병증 분류 시스템』이 있다. 

이 연구에서는
37주 미만의 미숙아에게서 실명을 유발할 수 있는 미
숙아망막병증(Retinopathy of Prematurity, ROP)을 
진단하는 연구를 진행하였다. 

고가의 장비대신 스마트폰으로 촬영한 미숙아의 안저 사진을 딥러닝 기법에 
활용하여 분류기를 통해 진단한 결과 99% 이상의 정확도를 보였다. 

이 연구에서도 이미지 분류 모델 중
Xception 모델을 사용하여 훈련하여 높은 수준의 정확
도를 보였다.

### III. 시스템 개발 환경
#### 3.1 Inception 모델
<img src="https://github.com/heeManLee/Diagnosis_dead_skin_cell/assets/90524127/8e8ed8a6-f1e5-4967-b4ee-2e1c772ae4d9"  width="400" height="200">

Inception model은 GoogLeNet이라고도 많이 알려진 모델이다. 

딥러닝 모델의 성능을 향상시키는 가장
간단한 방법은 훈련하는 모델의 깊이를 깊게 하는것이다.

하지만 이렇게 하면 과적합(Overfiting)이 발생하기
쉽고 계산에 필요한 자원들이 크게 증가되는 문제점이
있다. 

이러한 문제점들을 해결하기 위해 Optimal
Local Construction을 찾고 이걸 공간적으로 반복하는
동시에 연산해야 하는 노드들을 1x1 Filter로 줄여서
계산량이 많은 합성곱(Convolution)을 계산하자는 생각
에서 Inception model이 탄생하였다. 

기존의 local에 있는 이미지들은 1x1 filter를 사용하고 멀리 떨어진 correlation에 있는 다른 이미지들은 dimension
reduction의 역할을 1x1 convolution이 먼저 하고 이후
에 3x3, 5x5 filter로 나타내서 계산 자원의 활용을 개
선시키고 계산상의 어려움에 빠지지 않으며 단계 수를
늘릴 수 있게 되었다.

#### 3.2 Xception 모델
<img src="https://github.com/heeManLee/Diagnosis_dead_skin_cell/assets/90524127/cb0c3c30-9b9a-4012-8a9f-d819b170ed06"  width="400" height="200">

Inception module에서는 1x1 convolution을 통해
cross-channel correlation을 살펴보고, 입력 데이터를
원래의 공간보다 작은 3, 4개의 별도 공간에 mapping
한 다음에 3D 공간의 모든 spatial correlation을 3x3,
5x5 convolution에 mapping한다. 

Xception 모델에서는 위의 그림처럼 1x1 convolution으로 재구성하고
output channel이 겹치지 않는 부분에 대한 spatial
convolution 이 오는 형태로 재구성된다. 

이렇게 되면
input 데이터가 오면 1x1 convolution을 거치고 모든
channel을 분리시켜 output channel당 3x3 convolution
을 해주게 된다. 

이렇게 함으로써 channel과 spatial에
대한 두 방향을 완전히 분리할 수 있다. 

이런 형태의
합성곱층은 일반적으로 더 나은성능을 보여주는 것으로 나타나 있다.

#### 3.3 데이터 분석 및 전처리
본 논문에서 사용한 데이터셋은 AI HUB에서 제공하는 ‘유형별 두피 이미지 데이터셋’이다. 

이 데이터셋은 6개 유형의 두피 이미지를 4개 단계의 양호도로 구분해 놓았다. 

이번 연구에서 사용하는 데이터는 6개
유형의 두피 이미지 중에서 ‘미세각질’ 데이터이다.

이미지 전처리 작업은 Xception 모델의 권장 입력에
맞게 이미지 사이즈를 가로, 세로를 각각 224 픽셀로
변환하였고, 그래디언트 포화(Gradient Saturation)를
방지하기 위해 이미지의 픽셀 별 데이터 크기(Scale)를
–1과 1사이 분포로 정규화(Normalization)하였다.

두피각질에 따른 양호도로는 0.양호, 1.경증, 2.중증도, 3.중증이 있으며 
각각 4435개, 534개, 5486개, 2284개의 데이터 사진을 가지고 있다.

모델 학습을 위해 데이터를 훈련(Train), 검증(Validation), 테스트(Test) 셋을 7:2:1의 비율로 분할하였다. 

분할된 데이터셋의 개수는 훈련 셋 8919개, 테스트 셋 2548개, 검증 셋 1272개다.

훈련 데이터가 많지 않으므로 훈련 데이터에 데이터
증강기법을 적용하여 데이터의 개수를 늘려주었다. 

미세각질의 색상(hue), 채도(Saturation), 밝기(Brightness)등을 변경한 이미지는 
미세각질의 심각도를 식별하기 어려운 것으로 판단되었다. 

따라서 원본 이미지의 훈련 셋의 이미지는 각각 상하 대칭, 좌우
대칭, 90도 대칭을 적용하였다. 

증강기법을 적용한 훈련 데이터는 원본을 포함해 총 (8,919 +　8,919*3) = 35,676개이다.

### IV. 구현 및 학습 결과
#### 4.1 모델링
데이터를 전처리하고 모델을 구현하는 환경은 전적으로 Google Colab을 사용하였으며 런타임 유형으로
GPU를 선택하여 훈련을 가속하였다. 

이미지 분류 모델은 Xception 모델을 사용하되, Imagenet에서 제공하는 미리 학습된 가중치를 가져와 전이학습을 적용하였다.

모델의 Optimizer는 Adam, 학습 속도는 0.001로 설정하였다. 

손실함수는 라벨이 원-핫 인코딩(one-hot
encoding) 되어 있으므로 Catergorical-Crossentropy로 설정하였다. 

Xception 모델의 출력층은 1000개이므로 기존 모델의 출력층을 제외하고 미세각질의 4단계 심각도를 분류하는 
4개의 unit을 가진 Dense층을 추가하였다.

새롭게 추가한 출력층은 분류 작업을 위해 활성화함수로 Softmax를 설정하였다.

#### 4.2 실험결과
<img src="https://github.com/heeManLee/Diagnosis_dead_skin_cell/assets/90524127/2d193687-625b-41ea-b3b0-f83863032fed"  width="400" height="200">

그림 4. '두피 미세각질' Train accuracy

<img src="https://github.com/heeManLee/Diagnosis_dead_skin_cell/assets/90524127/6394eef7-2e7c-4530-a1e0-18e67ee1ed6d"  width="400" height="200">

 4.1에서 모델링한 결과를 가지고 모델을 학습시키고 
검증한 결과는 그림 4, 5와 같다. 

정확도(Accuracy)와 손실(loss)의 변화도를 잘 보여주기 위
해 Epoch은 100으로 조절하여 훈련을 진행하였다. 

메모리 크기의 제약으로 인해 batch size는 16으로 설정
하였다. 

그림 5와 같이 훈련 결과 train set에서는 95%의 정확도를 보이는 것을 확인하였고 
test_accuracy와 valid_accuracy에서의 81%로 준수하게 나왔음을 확인할 수 있었다. 

손실도는 train set에서 0.25대로 매우 낮게 나왔고 
test_loss와 valid_loss에서 1.2577, 1.1852로 1에 가깝게 나온 것
을 확인할 수 있다. 

### V. 결론 및 향후 연구 방향
본 연구에서는 AI-HUB에서 제공하는 두피 이미지
사진을 딥러닝 모델을 사용해 두피의 미세각질 상태를
분류하는 연구를 진행하였으며, 이미지 분류 모델로써
Xception 모델을 사용하였다. 

다른 모델에 비해 훈련할
파라미터의 수가 적은 Xception 모델 방식을 사용하였다. 

테스트 셋으로 모델의 성능을 검증한 결과 정확도(Accuracy)는 95.48%, 정밀도(Precision)은 91.08%,
재현율(Recall)은 90.82% 로 나타났다.

향후 연구로는 다른 이미지 분류 모델을 적용해보거
나 다른 이미지 증강 방식을 사용하거나 optimizer를
변경해보는 등 여러 가지 파라미터의 값을 변경하여
연구를 진행하고자 한다.

이 논문은 한국연구재단의 이공분야기초연구(기본연
구)의 지원을 받아 수행된 연구임(NRF-2019R1F1A
1063128).
