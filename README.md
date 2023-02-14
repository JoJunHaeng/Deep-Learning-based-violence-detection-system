## 딥러닝 기반의 폭력 행동 감지 시스템
![image](https://user-images.githubusercontent.com/93234544/218736560-18c3c2c1-09ce-4aad-860e-5b38a5e166d4.png)
-아동학대란 보호자를 포함한 성인이 아동의 건강 또는 복지를 해하거나 정상적인 발달을 할 수 없게 하는 신체적, 정신적, 성적 폭력행위와 가혹행위를 하는 것을 의미함.
-2020년 경북 문경시, 원아 9명에 대해 음식을 억지로 먹이거나 멍이 들도록 잡아 당기는 등의 가혹행위 발생
-아동 학대 신고 건수는 2014년에는 총 17,782건이었으나 2018년 36,416건으로 2배 이상 증가하였으며 특히 아동학대 의심 사례의 신고건수가 가장 증가한 것으로 알려지고 있음

* #### CCTV 동영상에서 폭력 행동 및 아동 학대 상황 발생에 대해서 정상/위협/폭력을 정의하고 이를 deep learning을 이용하여 감지하기 위한 기술 개발 프로젝트

## 프로젝트 담당 업무
* 관련 연구 조사
* 데이터 수집 및 라벨링 로직 개발
* 딥러닝 model 개발

## 개발 기간
* 2021년 06월 ~ 2022년 12월

## Version
* python3.9, Tensorflow 2.5
* C#, WPF

## 1. dataset
![image](https://user-images.githubusercontent.com/93234544/218737026-640474b3-14ac-427a-87ad-b709500b06b1.png)

* AI hub에서 제공하는 폭력 행위가 있는 동영상 965건 수집 및 아동 학대 감지를 위한 별도의 data 39건 수집
* Data set의 모든 video에 대해서 frame 추출 후 rgb 영상 및 optical flow 영상으로 Data set 구축
* 1개 video에 대해서 rgb, optical flow로 2종류의 데이터로 구성
* 데이터 셋의 label은 정상/위협/폭력 상황으로 정의

label|폭력 Video(train set)|아동 학대 Video(train set)
---|---|---|
정상|314개(230개)|13개(3개)|
위협|340개(238개)|13개(3개)|
폭력|311개(227개)|13개(3개)|
합계|965개(695개)|39개(10개)|

# 2. Model
![image](https://user-images.githubusercontent.com/93234544/206909360-066241e5-c53c-4200-8cbe-14a9dbc01f75.png)
* 시간 축으로 연속인 video data에 대한 feature extract를 수행하기 위해 3D conv 연산을 해야하나, 3D conv 구현 시 model의 parameter 수가 과도하게 증가하는 문제가 발생
* 이를 해결하기 위해 pseudo 3d conv를 이용하여 Resnet 구현
* pseudo 3D conv와 3D conv는 동일한 결과를 도출함
***
![image](https://user-images.githubusercontent.com/93234544/206909109-ddabe3d9-ee43-4604-af2c-9c99dd0b3af1.png)
* 동일한 조의 CNN model을 2개의 stream으로 구성하여 각각 rgb영상과 optical flow 영상을 학습
* 각각의 CNN에서 연산된 feature map을 global average pooling하여 별도의 MLP에서 classification 수행

# 3. result 
* #### 폭력 감지 model 학습 결과
모델|Accuracy
---|---|
rgb stream(resnet50)|0.435|
optical flow stream(resnet50)|0.461|
rgb+optical flow stream(resnet50)|0.482|
rgb stream(resnet101)|0.415|
optical flow stream(resnet101)|0.492|
rgb+optical flow stream(resnet101)|0.508|
rgb stream(PCA + resnet101)|0.456|
optical flow stream(PCA + resnet101)|0.440|
rgb+optical flow stream(PCA + resnet101)|0.554|
* 데이터를 model에 입력 시 기존의 image resize 대신 PCA를 활용하여 imput size를 조절함
 *** 
* #### 아동 학대 학습 결과(transfer learning)
모델|Accuracy
---|---|
rgb stream(resnet101)|0.556|
optical flow stream(resnet101)|0.444|
rgb+optical flow stream(resnet101)|0.778|
* Model 학습 조건별 accuracy에서 pretrained model을 이용하여 transfer learning 하였을 때의 결과
*** 

* #### 폭력 상황 감지 예시
![image](https://user-images.githubusercontent.com/93234544/206907597-4dd7f7b9-c60d-48c1-8502-5596563b8419.png)
![image](https://user-images.githubusercontent.com/93234544/206907613-570347d3-11bd-473a-a437-0d917768c013.png)

 ![image](https://user-images.githubusercontent.com/93234544/206910423-b1904e03-3cd3-4158-89ed-d09e23ac98ff.png)

  
  
