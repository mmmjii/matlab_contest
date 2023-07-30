# matlab_contest

## 주제 : HAPAGO, 얼굴 인식 알고리즘을 기반으로 유사한 하회탈 추정
### Ⅰ. Introduction
 인공지능은 현재 의료 분야를 포함하여 다양한 분야에서의 적용과 연구가 활발하게 이루어지고 있습니다. 그러나 문화 체험과 관련해서는 인공지능의 활용은 아직 많이 알려져 있지 않습니다. 특히, 우리 문화의 한 부분인 하회탈을 바탕으로 인공지능을 활용하는 아이디어는 아직까지 거의 언급되지 않은 것으로 확인됩니다.
 
 이러한 상황을 바탕으로, 한국 전통 문화 체험에 인공지능을 융합하고자 합니다. 하회탈은 경상북도 안동시 풀천면 하회마을에서 전통 놀이와 지배 계급에 대한 비판을 담아 나무 가면으로 만들어지는 예술적인 유산입니다. 이러한 하회탈을 활용하여, 사용자들에게 자신과 가장 닮은 하회탈을 추정하는 프로그램을 개발하고자 합니다.
 
 우리의 프로젝트는 다양한 인공지능 기술과 이미지 처리 알고리즘을 활용하여, 사용자가 제공하는 사진을 분석하고 유사한 하회탈을 찾아내는 시스템을 구축하는 것을 목표로 합니다. 이를 통해 우리는 인공지능 기술을 우리의 문화와 접목함으로써, 전통적이고 독특한 문화를 현대에 맞게 재해석하고 새로운 형태의 문화 체험을 제공할 수 있을 것으로 기대합니다.


### Ⅱ. Data and Method
#### 2.1 Overall Model Organization
본 프로젝트에서 고안한 '유사한 하회탈 추정 모델'은 다음과 같이 구성되어 있다
![4dZRKsIgPJ4m0GIV8HrIamEzxESP5fD1GT6gRufRK8e6Si9s-sH5VFayZ9oNvwNgLASxzJbIBI2bw0yIeaEHilJf6zFb33gwdlTpX7JgO6IpdYgnC-3A--VW-_cb](https://github.com/mmmjii/2023_matlab_contest/assets/107604539/d63e1889-4de1-4a3e-b828-1e8926e02eb3)

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
Dlib의 68 Landmark 알고리즘을 사용하여 얼굴 이미지 당 68개의 특징점을 얻었습니다. 이 68개의 랜드마크 지점은 얼굴, 눈, 코, 그리고 입 주변의 테두리에 위치합니다. 우리는 이 68개의 특징점 위치를 기반으로 ##가지 특징을 만들었습니다. 각 특징은 두 개의 랜드마크를 연결하는 두 선 사이의 각도의 비율 또는 코사인 값을 나타냅니다. 이 특징들의 계산 공식은 ###에 나와 있습니다.

F1과 F2는 각 눈의 높이를 해당 눈의 너비로 나눈 비율을 의미합니다. F3과 F4는 왼쪽 눈과 오른쪽 눈의 끝점을 연결하는 선과 코의 상단과 하단 끝점을 연결하는 선 사이의 각도의 코사인 값을 의미합니다. F7과 F8은 한 폭을 다른 폭으로 나눈 비율입니다. F7은 광대뼈를 지나는 폭을 인줄기를 지나는 폭으로 나눈 비율을 나타냅니다. F8은 인줄기를 지나는 폭을 턱을 지나는 폭으로 나눈 비율을 나타냅니다. F9는 얼굴의 하단 높이를 가운데 높이로 나눈 비율을 의미합니다. F10은 광대뼈를 지나가는 폭을 가운데 높이와 하단 높이의 길이로 나눈 비율을 나타냅니다.

#### 2.4 modeling
+ 2.4.1 RF
+ 2.4.2 SVM_POLY
+ 2.4.3 SVM_RBF
+ 2.4.4 CNN
### III. Results

### IV. Discussion

### V. Conclusion

### References
