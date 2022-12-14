## 7.1 전체 구조
- 인접하는 계층의 모든 뉴런과 연결되어 있는 연결을 Fully-Connected(Affine 계층)이라 한다.

## 7.2 합성곱 계층
- CNN에서는 패딩, 스트라이드 등과 같은 용어가 등장 한다.

## 7.2.1 완전 연결 계층의 문제점
- 완전 연결 계층에서는 인접하는 계층의 뉴런이 모두 연결 되고 출력의 수는 임의로 정할 수 있었다.
- 데이터의 형상이 무시 된다 라는 문제점이 있다.
- 완전 연결 계층은 공강적 정보가 무시 되고 입력 데이터를 등등한 차원으로 취급하여 형상에 담긴 정보를 살릴 수 없다.
- 합성곱 계층은 형상을 유지 한다.
- 합성곱 계층의 입력 데이터를 입력 특징맵, 출력 데이터를 출력 특징맵 이라고 한다.

## 7.2.2 합성곱 연산
- ...

## 7.2.3 패딩
- 합성 곱 연산을 수행하기 전에 입력 데이터 주변을 특정값으로 채운다.
- 주로 출력 크기를 조정할 목적으로 사용한다. 따라서 컨볼루션 연산을 거쳐도 출력 데이터 크기가 입력 크기와 동일하게 유지 될 수 있다.

## 7.2.4 스트라이드
- 스트라이드를 통해 다음 컨볼루션 연산할 위치를 정한다.

`O = (H + 2P - F) / S + 1`
- O : 결과 사이즈
- H : 입력 사이즈
- P : 패딩 크기
- F : 필터 크기
- S : 스트라이드

## 7.2.5 3차원 데이터의 합성곱 연산
- 입력데이터와 필터의 연산을 채널마다 수행하고 그 결과를 **더해서** 하나의 출력을 얻는다.

## 7.2.6 블록으로 생각 하기
![image](https://velog.velcdn.com/images%2Fkimkihoon0515%2Fpost%2Fdf23763c-394a-42f9-9dcc-5988e1447496%2Fimage.png)

- 입력데이터의 채널과 필터의 채널은 같아야 한다.
- Numpy나 Mat에서는 (H,W,C)로 표시하나 Tensor에서는 (C,H,W)로 표현 한다.

![image](https://velog.velcdn.com/images%2Fkimkihoon0515%2Fpost%2F7986391a-df12-40d2-b6be-7e8ee5611e4c%2Fimage.png)

- FN개의 필터를 사용하면 FN개의 맵이 생거 (FN, OH, OW)인 블록이 완성 된다.

#### 1X1 Convolution
- 채널의 수를 줄이고 싶을 때 사용한다.
- CX1X1 필터를 사용하면 채널이 1개가 된다.
- 이런 필터가 N개 있다면 출력 채널은 N개의 채널이 된다.


## 7.2.7 배치 처리
- 신경망 처리는 입력데이터를 한덩어리로 묶어 배치로 처리 한다.
- 각 계층을 흐르는 데이터의 차원을 하나 늘려 4차원 데이터로 저장 한다.
![image](https://velog.velcdn.com/images%2Fkimkihoon0515%2Fpost%2F0d98c989-6759-4bfd-adde-356f8821e408%2Fimage.png)

- 4차원 데이터가 하나 흐를 때 마다 N개에 대한 합성곱 연산이 이뤄진다. 즉, N회 분의 처리를 한번에 수행 한다.

## 7.3 풀링 계층
- 가로,세로 방향의 공간을 줄이는 연산이다.
- 필터 만큼 크기를 하나의 원소로 집약한다.

## 7.3.1 풀링 계층의 특징

#### 학습 해야할 매개 변수가 없다.
- 합성곱 계층과 다르게 학습 해야할 매개 변수가 없다. 

#### 채널 수가 변하지 않는다.
- 입력 데이터의 채널 수 그대로 출력 데이터로 내보낸다.

#### 입력의 변화에 영향을 적게 받는다.
- 입력 데이터가 변해도 풀링 결과는 잘 변하지 않는다.

## 7.6 CNN 시각화 하기

## 7.6.1 1번째 층의 가중치 시각화 하기
![image](https://velog.velcdn.com/post-images%2Fdscwinterstudy%2Fb0d98ab0-4199-11ea-bed1-737062fffe57%2Ffig-7-24.png)
- 학습 전 필터는 무작위로 초기화 되고 있어 규칙성이 없다.
- 학습 후 필터는 규칙성 있는 이미지가 된다. 즉, 점차 특징을 뽑아내는 필터가 된다.

## 7.6.2 층 깊이에 따른 추출 정보 변화
- 계층이 깊어질 수록 추출되는 정보(정확히는 강하게 반응하는 뉴런)는 더 추상화된다.
![image](https://velog.velcdn.com/post-images%2Fdscwinterstudy%2Fc9235880-4199-11ea-9479-ad3ff669a3cd%2Ffig-7-26.png)
- 1층은 에지와 블롭, 3층은 텍스쳐, 5층은 사물의 일부, 마지막은 사물의 클래스에 뉴런이 반응 한다.
- 뉴런이 반응 하는 대상이 단순한 모양에서 고급 정보로 변화해 간다, 사물의 의미를 이해하도록 변화하는 것이다.

## 7.7 대표적인 CNN

## 7.7.1 LeNet
- 합성곱 계층과 풀링 계층만 반복하고 마지막으로 완전열결계층을 거치면서 출력한다.
- LeNet은 시그모이드 함수를 사용하는 데 반해, 현재는 주로 ReLU를 사용한다.
- LeNet은 서브샘플링을 하여 중간 데이터의 크기를 줄이지만 현재는 맥스 풀링이 주류 이다.

## 7.7.4 AlexNet
- 활성화 함수를 ReLU를 이용한다.
- LRN이라는 국소적 정규화를 실시하는 계층을 이용한다.
- 드롭 아웃을 사용한다.