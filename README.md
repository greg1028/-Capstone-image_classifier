# VTIP (Vehicle Type Identification Program) <br/>전북대학교 2022 겨울학기 Capstone 프로젝트

**주/야간 자동차 종류 식별 모델** **_VTIP_** 입니다.  
사진 속 차량들을 각 차종들로 분류하는 인공지능 모델입니다.  

<center><img src="https://user-images.githubusercontent.com/83526669/208496856-fb471cdd-e29e-4f63-9e93-30b2d31cb486.png" width="500" height="500"></center>

파일을 업로드 하신 후, 실행하기 버튼을 누르시면 사진 속 차량들을 검출하고,   
각 클래스별로 분류하여 폴더에 저장합니다.  

[데모보기]()
https://youtu.be/-WyWCNJS92g

**CNN** 모델 구축 후 **Resnet50**, **Densnet121**, **Xception**, **MobileNetV2** 를 학습,  
fine tuning 했고, 총 5개 모델을 **averaging** 하여 **ensemble** 했습니다.  
Bus, Freight, Hatchback, Sedan, SUV, Truck, Van의 **7개 classes** 를 가집니다.
  
총 443,335개의 차량 이미지를 통해  
Training_set(354,669장), Validation_set(29,552장), Test_set(59,114장)을 구성했고  
이를 통해 모델들을 학습 및 검증하였습니다.    

본 프로젝트를 참고하여 재현하고 싶으신분들 께서는
[프로젝트 수행 과정](#프로젝트-수행-과정) 을 따라 전체적인 흐름을 이해하시고,  
공부가 필요한 부분은 [참고자료](#참고자료-reference)의 링크를 통해 무리없이 학습하실 수 있습니다.  
추가로 [구성](#구성), [코드 실행 필요 조건](#코드-실행-필요-조건), [코드 실행 방법](#코드-실행-방법)등을 확인하세요.

[DataSet, 저장된 모델들](https://drive.google.com/drive/folders/15ozD4DQ5JLCrXg6_t5TWyCMI_YWAGdJz)

<br/><br/>
  
## 목차
1. [프로젝트 수행 과정](#프로젝트-수행-과정)
2. [성능](#성능)
3. [데모](#데모)
4. [참고자료](#참고자료-reference)
5. [구성](#구성)
6. [코드 실행 필요 조건](#코드-실행-필요-조건)
7. [라이센스](#라이센스)
8. [기여 방법](#기여-방법)
9. [회고](#회고)
<br/><br/>
## 프로젝트 수행 과정
**_1. 인식 대상 자동차 선정_**
- Bus, Freight, Hatchback, Sedan, SUV, Truck, Van로 classes 선정
- 본 프로젝트와 같은 데이터를 통해 차종을 분류하는 모델을 만든 사례가 있어 비교/평가에 용이하다고 판단.
["CNN 알고리즘 기반 2 단계 차종 분류 모델."](#참고자료-reference)  

**_2. RAW 학습 데이터 구축 및 전처리_**
- [Aihub](https://aihub.or.kr/)의 자동차 차종/연식/번호판 인식용 영상 데이터 선정 (231.15 GB)
- 직사각형으로 크롭된 차량 이미지 파일만 추출 (7.33 GB, 443,335장)
- 7개의 classes로 디렉토리 구성 (Bus, Freight, Hatchback, Sedan, SUV, Truck, Van)
- 모델 학습에 적합하도록 이미지 전처리 코드 작성  Traning 80%, Vaildation 7%, Test 13% (354,669장, 29,552장, 59,114장)  
   
**_3. CNN, Resnet50, Densnet121, Xception, MobileNetV2 학습 및 fine tuning, ensemble_**
-  [tensorflow.keras.application](https://www.tensorflow.org/api_docs/python/tf/keras/applications)를 이용한 모델 생성
-  각 모델의 input pixels 에 맞게 scale (input preprocessing)
-  Training_set과 Validation_set을 이용한 모델들 학습
-  CNN을 제외한 나머지 모델 fine tuning (모델 정확도 향상)
-  Test_set을 이용한 모델 검증
-  5개의 models를 averaging-ensemble한 최종 차종 식별 모델 생성  
    
**_4. 사용자가 입력할 영상이미지(이미지 데이터)에서 차량객체 인식_**
- 사진 입력시 yolov3를 이용한 차량 검출, 최종 차종 식별 모델을 이용한 차종 식별
- yolov3에서 검출된 image를 model의 input size에 맞게 resize하는 과정에서 image가 많이 변형되어 예측 성능이 심하게 낮아지는 문제가 있다.추후에 해결할 예정이다.
 
**_5. 인터페이스를 통한 서비스 제공_**
- tkinter, PyQt5를 통한 GUI 인터페이스로 프로그램을 직관적으로 실행 가능하도록 하였다.
- QFileDialog를 통해 파일 업로드를 수행하며, 불러온 파일은 높이 210px로 확인할 수 있도록 하였다.

  <br/><br/>
  
## 성능
(59,114장의 이미지를 이용한 test 결과)
|        |CNN|ResNet50|Xception| ** Densenet121 ** |MobileNetV2|VTIP(ensemble)|
|---|---|---|---|---|---|---|
|accuracy|0.9701|0.9835|0.9726|0.9870|0.9757|0.9259|
|loss    |0.1869|0.1097|0.1126|0.0596|0.0981|0.8395|


<br/><br/>
## 데모
[시연 유튜브 링크](https://youtu.be/-WyWCNJS92g)


<br/><br/>
## 참고자료 reference
CNN [tensorflow document](https://www.tensorflow.org/tutorials/images/cnn)
- [CNN이란?https://youngq.tistory.com/40](https://youngq.tistory.com/40)  

<br/>

Resnet50 [tensorflow document](https://www.tensorflow.org/api_docs/python/tf/keras/applications/resnet50/ResNet50)
- [Resnet50구조 https://velog.io/@ssulee0206/ResNet50](https://velog.io/@ssulee0206/ResNet50)  

<br/>

Densnet121 [tensorflow document](https://www.tensorflow.org/api_docs/python/tf/keras/applications/densenet/DenseNet121) 
- [Densenet121구조 https://csm-kr.tistory.com/10](https://csm-kr.tistory.com/10)  

<br/>

Xception [tensorflow document](https://www.tensorflow.org/api_docs/python/tf/keras/applications/xception/Xception) 
- [Xception구조 https://wikidocs.net/122179](https://wikidocs.net/122179)  

<br/>

MobileNetV2 [tensorflow document](https://www.tensorflow.org/api_docs/python/tf/keras/applications/mobilenet_v2/MobileNetV2) 
- [MobileNetV2구조 https://gaussian37.github.io/dl-concept-mobilenet_v2/](https://gaussian37.github.io/dl-concept-mobilenet_v2/)  

<br/>

[ensemble 이란?](https://velog.io/@hyesoup/%EC%95%99%EC%83%81%EB%B8%94-Ensemble-%EC%9D%B4%EB%9E%80)
- [everage ensemble with models](https://stackoverflow.com/questions/67647843/is-there-a-way-to-ensemble-two-keras-h5-models-trained-for-same-classes)
- [change layer's name](https://datascience.stackexchange.com/questions/40886/how-to-change-the-names-of-the-layers-of-deep-learning-in-keras)  
  

[DataSet, 저장된 모델들](https://drive.google.com/drive/folders/15ozD4DQ5JLCrXg6_t5TWyCMI_YWAGdJz)  

[stackoverflow](https://stackoverflow.com)  
[datascience.stackexchange](https://datascience.stackexchange.com)  
[keras documentation](https://keras.io/)  
[tensorflow](https://www.tensorflow.org)  
<br/><br/>

□ 이용, et al. "자동-레이블링 기반 영상 학습데이터 제작 시스템." 한국콘텐츠학회논문지 21.6 (2021): 701-715.
<br/><br/>
□ Hedeya, Mohamed A., Ahmad H. Eid, and Rehab F. Abdel-Kader. "A super-learner
  ensemble of deep networks for vehicle-type classification." IEEE Access 8 (2020):
  98266-98280.<br/><br/>
□ 김한겸, et al. "CNN 알고리즘 기반 2 단계 차종 분류 모델." 한국정보처리학회 학술대회논문집 28.2 (2021): 791-794.  
<br/><br/>
## 구성
(code)models 모델을 import, training, fine tuning 하기 위한 코드이다.  
(image)TestData 코드를 실행시킬 수 있도록 만든 sample image data set이다.  
[DataSet, 저장된 모델들](https://drive.google.com/drive/folders/15ozD4DQ5JLCrXg6_t5TWyCMI_YWAGdJz)실제로 훈련에 사용된 DataSet과 저장된 모델들이 준비되어 있다.    
VTIP_exe.ipynb VTIP 실행파일을 만드는 코드이다.

  <br/><br/>
## 코드 실행 필요 조건
모델을 학습하기 위해서 자신의 GPU/CPU 버전에 맞는 package 들이 필요합니다.
[GPU/CPU 호환 버전 확인하기](https://www.tensorflow.org/install/source_windows#tested_build_configurations)
- tensorflow_gpu : gpu를 사용하지 않는다면 tensorflow를 설치해도 되지만, CPU를 사용하므로 매우 느립니다.
- cuDNN
- CUDA
- cudatoolkit
- anaconda3
- jupyter notebook
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
<br/><br/>
## 회고
