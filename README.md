# matlab_contest

## 주제 : HAPAGO, 얼굴 인식 알고리즘을 기반으로 유사한 하회탈 추정
### Ⅰ. Introduction
 ‘하회탈’은 인간의 모습을 본 뜬 한국의 전통적인 가면으로, 다양한 종류가 있다. 
 이 프로젝트는 각각의 하회탈을 분석하고, 그에 따라 사용자에게 유사한 하회탈을 추정하는 것을 목표로 한다.
 
### Ⅱ. Data and Method
#### 2.1 Overall Model Organization
본 프로젝트에서 고안한 '유사한 하회탈 추정 모델'은 다음과 같이 구성되어 있다

-------
이 부분에 전체적인 모델 구조도 추가 예정
-------
먼저, 입력 이미지 데이터에 대해 68 Face Landmarks를 이용하여 68개의 특징점을 구함. 

<img width="289" alt="image" src="https://github.com/mmmjii/2023_matlab_contest/assets/107604539/7d8a892b-e0de-431d-ac73-ba44b66b8da5">


이러한 특징점을 활용하여 턱의 각도, 눈의 폭과 길이의 비율 등 모델에서 사용되는 특징들의 값을 계산. 

특징값들을 기반으로 지도학습방법으로 학습된 머신러닝 모델이 유사한 하회탈을 추정함.

#### 2.2 data
데이터 출처 : https://www.kaggle.com/datasets/atulanandjha/lfwpeople

증명사진만을 사용한 데이터에서 눈코입이 다 보이는 사진까지 데이터 확장
- 정면사진만
    1차 : 정면 사진을 골라내는 코드 사용
    2차 : 얼굴 특징점을 사용하여 각도 측정 → 각도가 90도에 근접할수록 정면이라 판단(얼굴이 옆으로 돌아간 경우, 위 아래로 움직인 경우 모두 고려 가능 → 각도 분포에 근거해 90<= data <= 151 기준 설정 )

- 여성과 남성 데이터셋을 구분하지 않은 이유 → 각 성별의 특징이 하회탈의 특징에 큰 영향을 주지 않는다고 생각
- 각 연령대 별로 데이터 분포를 고려하지 않은 이유 → 인터넷에서 수집하는 사진들은 연령대 정보를 거의 포함하고 있지 않고 있음. 거의 영향력이 없다고 생각. 눈에 띄게 나이가 있어 보이는 인물(흰 머리, 주름)의 사진만 따로 모아 사용.(나이가 들수록 눈가가 쳐지기 때문에 이러한 요소가 하회탈 구분에 영향을 준다고 생각)

원자료 data : 1330개
증강후 data : 1330 x 11 = 14630개
데이터 증강 기법
1. 30도 ~ 30도 사이의 각도로 랜덤하게 회전 변환
2. 밝기 변화 (이미지의 밝기가 50%에서 90% 사이의 무작위 값으로 조정)
3. 밝기 변화 (이미지의 밝기가 1배에서 1.5배 사이의 무작위 값으로 변화 )
4. 채널 변화 (밝기 + 색감)
5. 가우시안 블러 처리 ( ‘(45, 45)’ 크기의 블러와 ‘0’ 표준편차 값 사용)

각 증강기법당 랜덤 이미지 2개 생성함. 그래서 한이미지를 증강했을경우에 추가로 10개의 이미지가 증강됨 


#### 2.3 method


#### 2.4 modeling
+ 2.4.1 RF
+ 2.4.2 SVM_POLY
+ 2.4.3 SVM_
### III. Results

### IV. Discussion

### V. Conclusion

### References
