# VTIP (Vehicle Type Identification Program)
주/야간 자동차 종류 식별 모델 VTIP입니다.  
사진 속 차량들을 각 차종들로 분류하는 모델입니다.  

CNN 모델 구축 후 Resnet50, Densnet121, Xception, MobileNetV2를 학습, fine tuning 했고, 
총 5개 모델을 averaging 하여 ensemble 했습니다.  
Bus, Freight, Hatchback, Sedan, SUV, Truck, Van의 7개 classes를 가집니다.
  
총 443,335개의 차량 이미지를 통해  
Training_set(354,669장), Validation_set(29,552장), Test_set(59,114장)을 구성했고  
이를 통해 모델들을 학습 및 검증하였습니다.    
<br/><br/>
  
## 목차
1. [프로젝트 수행 과정](#프로젝트-수행-과정)
2. [성능](#성능)
3. [데모](#데모)
4. [참고자료](#참고자료-reference)
5. [구성](#구성)
6. [코드 실행 필요 조건](#코드-실행-필요-조건)
7. [코드 실행 방법](#코드-실행-방법)
8. [VTIP 사용 방법](#VTIP-사용-방법)
9. [라이센스](#라이센스)
10. [기여 방법](#기여-방법)
<br/><br/>
## 프로젝트 수행 과정
**_1. 인식 대상 자동차 선정_**
- Bus, Freight, Hatchback, Sedan, SUV, Truck, Van로 classes 선정
- 본 프로젝트와 같은 데이터를 통해 차종을 분류하는 모델을 만든 사례가 있어 비교/평가에 용이하다고 판단.
["CNN 알고리즘 기반 2 단계 차종 분류 모델."](#참고자료-reference)  
<br/>
**_2. RAW 학습 데이터 구축 및 전처리_**
- [Aihub](https://aihub.or.kr/)의 자동차 차종/연식/번호판 인식용 영상 데이터 선정 (231.15 GB)
- 직사각형으로 크롭된 차량 이미지 파일만 추출 (7.33 GB, 443,335장)
- 7개의 classes로 디렉토리 구성 (Bus, Freight, Hatchback, Sedan, SUV, Truck, Van)
- 모델 학습에 적합하도록 이미지 전처리 코드 작성 Traning 80%, Vaildation 7%, Test 13% (354,669장, 29,552장, 59,114장)  
    <br/>
**_3. CNN, Resnet50, Densnet121, Xception, MobileNetV2 학습 및 fine tuning, ensemble_**
-  [tensorflow.keras.application](https://www.tensorflow.org/api_docs/python/tf/keras/applications/xception/Xception)를 이용한 모델 생성
-  각 모델의 input pixels 에 맞게 scale (input preprocessing)
-  Training_set과 Validation_set을 이용한 모델들 학습
-  CNN을 제외한 나머지 모델 fine tuning (모델 정확도 향상)
-  Test_set을 이용한 모델 검증
-  5개의 models를 ensemble한 최종 차종 식별 모델 생성  
    <br/>
**_4. 웹 인터페이스를 통한 서비스 제공_**
- html을 이용한 web 인터페이스 생성
- 사진 입력시 yolo를 이용한 차량 검출, 최종 차종 식별 모델을 이용한 차종 식별 
  <br/><br/>
  
## 성능
(59,114장의 이미지를 이용한 test 결과)
|        |CNN|ResNet50|Xception|Densenet121|MobileNetV2|VTIP(ensemble)|
|---|---|---|---|---|---|---|
|accuracy|0.9701|0.9835|0.9726|0.9870|0.9757| |
|loss    |0.1869|0.1097|0.1126|0.0596|0.0981| |
  <br/><br/>
## 데모
시연 유튜브 링크


<br/><br/>
## 참고자료 reference
[CNN]
[Resnet50]
[Densnet121]
[Xception]  
[MobileNetV2]

ensemble
- [everage ensemble with models](https://stackoverflow.com/questions/67647843/is-there-a-way-to-ensemble-two-keras-h5-models-trained-for-same-classes)
- [change layer's name](https://datascience.stackexchange.com/questions/40886/how-to-change-the-names-of-the-layers-of-deep-learning-in-keras)  
  
  
[stackoverflow](https://stackoverflow.com)  
[datascience.stackexchange](https://datascience.stackexchange.com)  

□ 이용, et al. "자동-레이블링 기반 영상 학습데이터 제작 시스템." 한국콘텐츠학회논문지 21.6 (2021): 701-715.
<br/><br/>
□ Hedeya, Mohamed A., Ahmad H. Eid, and Rehab F. Abdel-Kader. "A super-learner
  ensemble of deep networks for vehicle-type classification." IEEE Access 8 (2020):
  98266-98280.<br/><br/>
□ 김한겸, et al. "CNN 알고리즘 기반 2 단계 차종 분류 모델." 한국정보처리학회 학술대회논문집 28.2 (2021): 791-794.  
<br/><br/>
## 구성
해당 파일이 어떠한 역할을 하는 파일인지를 간단히 설명 전반적인 맥락을 파악하기 위한 용도  
  <br/><br/>
## 코드 실행 필요 조건
모델을 학습하기 위해서 자신의 GPU/CPU 버전에 맞는 package 들이 필요합니다.
[GPU/CPU 호환 버전 확인하기](https://www.tensorflow.org/install/source_windows#tested_build_configurations)
- tensorflow_gpu : gpu를 사용하지 않는다면 tensorflow를 설치해도 되지만, CPU를 사용하므로 매우 느립니다.
- cuDNN
- CUDA
- cudatoolkit
  <br/><br/>
## 코드 실행 방법
작성한 코드를 어떻게 실행해야 하는지에 대한 가이드라인이다. Usage Example을 함께 작성하면 좋다.  
<br/><br/>
## VTIP 사용 방법
<br/><br/>
## 라이센스
[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)  
<br/><br/>
## 기여 방법
1. 본 프로젝트를 포크합니다.
2. 본인의 깃허브에 복제된 repository를 clone 하세요.
3. 새로 브랜치를 만들어 작업을 완료한 후 push 해주세요.
4. develop브랜치로 PR을 보내주세요.
>커밋 말머리에 어떤 수정이 있었는지 명시해주세요.
>> (feat) : 새로운 기능에 대한 커밋  
>> (fix) : 버그 수정에 대한 커밋  
>> (build) : 빌드 관련 파일 수정에 대한 커밋  
>> (chore) : 그 외 자잘한 수정에 대한 커밋  
>> (ci) : ci관련 설정 수정에 대한 커밋  
>> (docs) : 문서 수정에 대한 커밋  
>> (style) : 코드 스타일 혹은 포맷 등에 관한 커밋  
>> (refactor) : 코드 리팩토링에 대한 커밋  
>> (test) : 테스트 코드 수정에 대한 커밋
