# 세션 2: 딥 러닝의 진화와 고급 신경망 아키텍처

이 세션에서는 기초적인 신경망 모델에서 현대의 딥 러닝 아키텍처로의 흥미로운 여정을 탐험합니다. 우리는 제프리 힌튼의 중요한 공헌, 합성곱 신경망(CNNs)의 발전, 그리고 혁신적인 트랜스포머 모델의 도입에 초점을 맞출 것입니다. 이러한 진화는 인공지능을 혁명적으로 변화시켜, 기계가 한때는 불가능하다고 여겨졌던 방식으로 복잡한 데이터를 처리하고 이해할 수 있게 만들었습니다.

## 딥 러닝의 부상

### 깊은 네트워크 훈련의 도전과제

1990년대와 2000년대 초, 연구자들은 깊은 신경망(DNNs)을 훈련시키는 데 상당한 어려움을 겪었습니다. 가장 두드러진 문제 중 하나는 **기울기 소실 문제**였습니다.

#### 기울기 소실 문제 설명

긴 사람 체인을 통해 메시지를 전달하려고 한다고 상상해 보세요. 메시지가 전달되면서 왜곡되고, 끝에 도달할 때쯤이면 알아볼 수 없게 될 수 있습니다. 마찬가지로, 깊은 신경망에서 더 많은 층을 추가할수록, 가중치를 업데이트하는 데 사용되는 기울기(우리의 "메시지"와 같은)가 네트워크를 통해 역방향으로 전파될 때 극도로 작아지거나(또는 "소실") 됩니다. 이로 인해 초기 층들이 효과적으로 학습하기 어려워집니다. 의미 있는 피드백을 거의 받지 못하기 때문입니다.

**기울기 소실 문제(Vanishing Gradient Problem)**는 **심층 신경망(Deep Neural Networks, DNNs)**에서 발생하는 중요한 문제 중 하나로, 특히 역전파(backpropagation) 알고리즘을 사용할 때 주로 나타납니다. 이 문제는 딥러닝 모델의 훈련을 방해하고 성능을 저하시킬 수 있기 때문에, 이를 이해하고 해결하는 것이 깊은 신경망 학습의 중요한 부분입니다.

**1. 기울기 소실 문제란?**
기울기 소실 문제는 **역전파 과정**에서 **기울기(gradient)**가 점점 작아져서, 결국에는 가중치가 거의 업데이트되지 않는 현상을 말합니다. 네트워크가 깊어질수록, 즉 **층(layer)**의 수가 많아질수록, 입력층에서 가까운 하위 층들의 가중치 업데이트가 제대로 이루어지지 않게 됩니다.

- 역전파를 통해 오차가 출력층에서부터 입력층으로 전파될 때, 오차의 크기는 네트워크의 층을 거치면서 점점 더 작아지거나, 거의 0에 가까워질 수 있습니다.
- 결과적으로 초기 층의 가중치가 거의 조정되지 않으며, 이는 초기 층이 제대로 학습되지 않는 원인이 됩니다. 신경망의 많은 층들이 학습되지 않으면서 모델의 성능이 저하됩니다.

**2. 기울기 소실 문제의 원인**

기울기 소실 문제의 근본 원인은 **활성화 함수**와 **체인 룰(Chain Rule)**을 통한 역전파 방식에 있습니다.

**2.1 체인 룰을 통한 기울기 계산**

- **역전파**에서 각 가중치의 변화는 **오차 함수의 미분**을 통해 계산됩니다.
- 네트워크의 가중치는 체인 룰을 사용하여, 각 층의 활성화 함수에 대한 미분값을 연쇄적으로 곱해 계산됩니다.
- 만약 층이 많고, 각 층에서 미분 값이 0과 1 사이의 소수라면, 이 값들이 계속 곱해지면서 **기하급수적으로 감소**하게 되어, 결국 거의 0에 가까운 값이 되어버립니다. 즉, **깊은 층에서는 오차 신호가 사라지게 됩니다**.

**2.2 활성화 함수의 영향**

- **시그모이드(Sigmoid)와 Tanh 활성화 함수**:
  - 시그모이드 활성화 함수는 입력 값을 [0, 1] 사이로, Tanh는 [-1, 1] 사이로 제한합니다.
  - 이 함수들의 **기울기**는 대부분의 입력 영역에서 **작은 값**을 가집니다. 역전파에서 이를 반복적으로 곱하면 기울기가 점점 더 작아지며, 결국 사라지게 됩니다.
  - 예를 들어, 시그모이드 함수의 그래프를 보면 입력이 매우 큰 양수나 음수일 경우 **포화 상태(saturated state)**에 도달하게 되어, 기울기(미분 값)가 거의 0에 가까워집니다. 이로 인해 역전파를 통한 가중치 업데이트가 제대로 이루어지지 않게 됩니다.

**3. 기울기 소실 문제의 영향**

- **학습 정체**: 초기 층의 기울기가 거의 0에 가까워지면서 가중치가 조정되지 않기 때문에, 초기 층의 **특징 학습**이 멈추게 되고, 결과적으로 신경망 전체의 학습이 정체됩니다.
- **오랜 학습 시간**: 깊은 신경망을 학습하는 데 **매우 긴 시간이 필요**하며, 학습 속도가 극도로 느려질 수 있습니다.
- **성능 저하**: 제대로 훈련되지 않은 초기 층들 때문에 모델이 입력 데이터의 복잡한 특징을 효과적으로 학습하지 못하여 **성능이 저하**됩니다.

**4. 기울기 소실 문제 해결 방법**
기울기 소실 문제를 해결하기 위해 여러 방법이 제안되었습니다. 주요 방법들은 다음과 같습니다.

**4.1 새로운 활성화 함수 도입**

- **ReLU(Rectified Linear Unit)**:

  - ReLU는 입력이 양수일 때는 그대로 출력하고, 음수일 때는 0으로 변환합니다.
  - **미분 값이 1 또는 0**이기 때문에, 기울기가 층을 거쳐 감소하지 않으며, 기울기 소실 문제를 완화합니다.
  - 하지만 ReLU 역시 **죽은 ReLU(dying ReLU)** 문제를 가질 수 있는데, 이는 뉴런이 계속 음수를 받아서 미분 값이 0이 되어 더 이상 학습되지 않는 현상입니다. 이를 해결하기 위해 **Leaky ReLU**나 **Parametric ReLU** 등의 변형이 사용되기도 합니다.

- **Leaky ReLU**:
  - Leaky ReLU는 입력이 음수일 때도 작은 양의 기울기를 주어, 뉴런이 완전히 죽지 않도록 합니다. 이 방식은 ReLU에서 발생하는 죽은 뉴런 문제를 어느 정도 해결합니다.

**4.2 가중치 초기화 기법 개선**

- **Xavier 초기화 (Xavier Initialization)**:

  - 가중치를 초기화할 때, 가중치의 분포를 신경망의 **입력과 출력 뉴런의 개수**에 따라 설정하여, 각 층의 출력 값이 균형 있게 유지되도록 합니다.
  - 이를 통해 기울기가 소실되지 않고 네트워크 전체에 걸쳐 잘 전파될 수 있습니다.

- **He 초기화 (He Initialization)**:
  - ReLU 활성화 함수와 잘 맞도록 가중치를 초기화하는 방법으로, 가중치를 더 큰 분포에서 초기화하여 층 간의 기울기 소실을 줄이는 방식입니다.
  - ReLU와 같은 비선형 활성화 함수와 함께 사용될 때 효과적입니다.

**4.3 네트워크 구조 변경**

- **잔차 연결 (Residual Connections)**:

  - **ResNet**(Residual Networks)과 같은 모델에서는 **잔차 연결**을 사용하여, 네트워크의 한 층의 출력을 몇 층 건너뛰어 다음 층에 직접 전달합니다.
  - 이는 기울기가 층을 거치면서 감소하지 않고, 직접 전파될 수 있도록 도와 기울기 소실 문제를 크게 완화합니다.
  - 이러한 접근은 네트워크가 **훨씬 깊어지더라도 안정적인 학습**을 가능하게 합니다.

- **Batch Normalization**:
  - **배치 정규화(Batch Normalization)**는 각 미니 배치에서 출력 값을 정규화하여, 데이터가 네트워크를 통과할 때 **분포의 변화(covariate shift)**를 줄여줍니다.
  - 이를 통해 학습을 안정화하고, 기울기가 소실되는 문제를 완화하여 보다 빠르고 효과적인 학습을 지원합니다.

**5. 기울기 소실 문제의 예시와 직관적 이해**

- **긴 체인의 메시지 전달**: 기울기 소실 문제를 쉽게 이해하기 위해 **긴 체인을 통한 메시지 전달**에 비유할 수 있습니다. 체인의 처음 사람에게서 시작된 메시지가 여러 사람을 거쳐 마지막 사람에게 전달될 때, 각 사람의 전달 과정에서 조금씩 메시지가 왜곡되고 점점 작아져, 마지막에는 원래의 메시지를 거의 알아볼 수 없게 됩니다. 이는 깊은 신경망에서 역전파가 이루어질 때 기울기가 점점 감소하여 거의 소실되는 것과 유사합니다.
- **깊은 빛깔이 흐려지는 그림**: 한 층 한 층을 거치면서 그림이 점점 흐려지는 것을 생각해볼 수 있습니다. 이 과정에서 초기의 명확한 디테일은 점점 사라져버리고, 결국에는 아무런 유용한 정보를 전달하지 못하게 되는 것처럼, 기울기가 점점 약해지며 학습되지 않는 현상이 발생합니다.

**요약**

- **기울기 소실 문제**는 딥러닝에서 **깊은 신경망의 초기 층이 제대로 학습되지 않는 현상**을 의미합니다. 이는 특히 시그모이드나 Tanh 활성화 함수를 사용할 때, 층이 깊어질수록 기울기가 거의 0에 가까워져 가중치 업데이트가 이루어지지 않기 때문에 발생합니다.
- **해결 방법**으로는 **ReLU**와 같은 활성화 함수의 사용, **가중치 초기화 기법 개선**, **잔차 연결**을 통한 기울기 전달 개선 등이 있습니다.
- 이러한 기술들은 딥러닝 모델이 **더 깊고 복잡한 네트워크**를 학습할 수 있도록 하여, 현대의 인공지능 기술 발전에 중요한 기여를 하고 있습니다.

### 제한 볼츠만 머신(RBMs)을 통한 돌파구

2006년, 제프리 힌튼은 **제한 볼츠만 머신(RBMs)**과 **계층별 사전 훈련**을 사용하여 이러한 도전과제를 해결하는 새로운 접근법을 소개했습니다.

#### 계층별 사전 훈련

이 기술은 입력 층에서 시작하여 출력 층으로 이동하면서 네트워크를 한 층씩 훈련시키는 것을 포함합니다. 각 층은 초기에 RBM으로 훈련되어 입력을 재구성하는 법을 학습합니다. 이 사전 훈련 단계 후, 전체 네트워크는 역전파를 사용하여 미세 조정됩니다.

이것은 마치 고층 건물을 짓는 것과 같습니다: 모든 층을 동시에 건설하려고 하지 않고, 다음 층으로 이동하기 전에 각 층을 건설하고 안정화시킵니다. 이 접근법은 네트워크의 가중치를 더 나은 상태로 초기화하는 데 도움을 주어, 후속 미세 조정 과정을 더 효과적으로 만듭니다.

## 제프리 힌튼의 신경망에 대한 주요 공헌

종종 "딥 러닝의 대부"로 불리는 제프리 힌튼은 신경망과 인공지능 분야를 형성한 여러 혁신적인 공헌을 했습니다.

### 1. 역전파 (1986)

1986년, 힌튼은 데이비드 루멜하트와 로널드 윌리엄스와 함께 다층 신경망을 훈련시키기 위한 **역전파 알고리즘**을 소개했습니다.

![역전파 이미지](figs/Hinton-papers-1.jpg)
![역전파 이미지](figs/Hinton-papers-2.jpg)

#### 역전파의 작동 방식

복잡한 투석기의 조준을 조정하여 목표물을 맞추려고 한다고 상상해 보세요. 발사를 하고, 얼마나 빗나갔는지 확인한 다음, 그에 따라 기계의 각 부분을 조정합니다. 역전파도 비슷하게 작동합니다:

1. 네트워크가 예측을 합니다.
2. 오차(예측과 실제 출력 간의 차이)가 계산됩니다.
3. 이 오차는 네트워크를 통해 역방향으로 전파됩니다.
4. 각 가중치는 오차에 대한 기여도에 따라 조정됩니다.

이 과정을 통해 네트워크는 자신의 실수로부터 학습하고 점진적으로 성능을 향상시킬 수 있습니다. 마치 각 시도마다 투석기를 미세 조정하는 것과 비슷합니다.

**역전파(Backpropagation)**는 신경망을 훈련하는 데 필수적인 알고리즘으로, 네트워크의 가중치와 편향을 조정해 **오차를 최소화**하는 과정입니다. 역전파는 네트워크가 **예측 오류**를 기반으로 학습하도록 하여 점점 더 정확한 예측을 할 수 있도록 만듭니다. 이 알고리즘은 **경사하강법(Gradient Descent)**을 통해 가중치를 조정하는데, 이를 단계적으로 설명드리겠습니다.

##### **1. 역전파의 개요**

- **목적**: 네트워크의 예측 값과 실제 값 간의 차이를 줄이기 위해 각 연결 가중치를 업데이트합니다.
- **기본 원리**: 역전파는 네트워크의 **출력층에서 입력층으로** 오류를 전파하여 각 가중치의 변화량을 계산하고, 이를 사용해 가중치를 수정합니다.

##### **2. 역전파의 주요 단계**

역전파는 크게 **순방향 전달(Forward Pass)**, **오차 계산(Error Calculation)**, **오차 역전파(Backward Pass)**의 세 단계로 구성됩니다.

**단계 1: 순방향 전달 (Forward Pass)**

- 입력 데이터를 네트워크에 전달하여 **출력 값**을 계산합니다.
- 각 층의 노드(뉴런)는 **활성화 함수**를 거쳐 출력을 생성하고, 이를 다음 층으로 전달합니다.
- 이 과정을 통해 **예측 값**이 생성됩니다.

**단계 2: 오차 계산 (Error Calculation)**

- 출력층에서의 **예측 값과 실제 값**의 차이를 계산하여 **오차(Error)**를 구합니다.
- 일반적으로 사용되는 오차 함수는 **평균 제곱 오차(MSE, Mean Squared Error)**로, 다음과 같이 정의됩니다:
  $$
  E = \frac{1}{2} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
  $$
  여기서 $ y_i $는 실제 값이고, $ \hat{y}\_i $는 예측 값입니다. 이 오차는 네트워크가 현재 학습 상황에서 얼마나 잘못되었는지를 나타냅니다.

**단계 3: 오차 역전파 (Backward Pass)**

**오차를 각 가중치에 대해 미분**하여, 가중치를 얼마나 조정해야 하는지를 계산하는 과정입니다. 역전파는 이 오차를 뒤로 전파하여 가중치 업데이트에 필요한 **기울기(Gradient)**를 계산합니다.

- **오차의 미분을 통한 가중치 조정**:
  - 역전파의 핵심은 오차 함수 $E$를 각 가중치 $w$에 대해 미분하여, 가중치가 오차에 미치는 영향을 계산하는 것입니다. 이를 통해 각 가중치가 얼마나 조정되어야 오차가 줄어드는지 알 수 있습니다.
  - 이때, **체인 룰(Chain Rule)**이 사용됩니다. 체인 룰은 복합 함수의 미분을 계산하는 방법으로, 역전파에서 오차가 각 가중치에 미치는 영향을 계층적으로 계산할 수 있도록 합니다.

**예시: 단일 뉴런의 역전파**

단일 뉴런에서 역전파 과정을 간단히 설명하면 다음과 같습니다:

1. **출력 오차 계산**:

   - 출력 노드에서 오차를 계산합니다.
     $$
     \delta_{\text{output}} = \frac{\partial E}{\partial a} \cdot f'(z)
     $$
     여기서:
   - $ a $는 출력값입니다.
   - $ z $는 가중합(input weighted sum)이며, $ f'(z) $는 활성화 함수의 미분입니다.

2. **가중치 변화량 계산**:
   - 각 가중치에 대해 오차 기울기를 계산합니다.
     $$
     \Delta w_i = -\eta \cdot \delta_{\text{output}} \cdot x_i
     $$
     여기서 $ \eta $는 **학습률(learning rate)**이고, $ x_i $는 입력 값입니다.

**다층 신경망에서의 역전파**

다층 신경망의 경우, 오차는 출력층에서 시작하여 **모든 은닉층(hidden layers)**을 거쳐 입력층 방향으로 전파됩니다.

- **출력층에서의 오차 계산**:
  - 출력층의 각 노드에 대해 오차 기울기를 계산합니다.
- **은닉층으로의 전파**:
  - 출력층에서 계산된 오차는 **체인 룰**을 사용하여 이전 층의 노드들로 전달됩니다. 이를 통해 은닉층의 각 가중치가 출력 오차에 미치는 영향을 계산할 수 있습니다.
  - 은닉층의 노드 $ j $에 대한 오차 기울기는 다음과 같이 계산됩니다:
    $$
    \delta_j = f'(z_j) \sum_{k} \delta_k w_{jk}
    $$
    여기서:
    - $ f'(z_j) $는 은닉층 노드의 활성화 함수의 미분입니다.
    - $ \delta_k $는 다음 층의 오차 기울기입니다.
    - $ w\_{jk} $는 은닉층 노드와 다음 층 노드 간의 가중치입니다.

##### **3. 가중치 업데이트**

- 역전파를 통해 계산된 기울기를 사용하여, 각 가중치를 다음과 같이 업데이트합니다:
  $$
  w_{ij} = w_{ij} - \eta \cdot \delta_j \cdot x_i
  $$
  여기서 $ \eta $는 학습률입니다. 학습률은 모델이 얼마나 빠르게 학습할지를 결정하며, 지나치게 크면 학습이 불안정해지고 너무 작으면 학습 속도가 느려질 수 있습니다.

##### **4. 활성화 함수와 역전파**

역전파는 **활성화 함수**의 선택에 크게 영향을 받습니다. 일반적으로 사용되는 활성화 함수와 그 역할은 다음과 같습니다:

- **시그모이드(Sigmoid)**: 출력값을 [0, 1]로 제한해줍니다. 그러나 기울기 소실 문제(vanishing gradient problem)를 유발할 수 있습니다.
- **ReLU(Rectified Linear Unit)**: 양수 값을 그대로 반환하고 음수 값을 0으로 변환합니다. **기울기 소실 문제**를 해결하는 데 도움이 되지만, 일부 노드가 학습 중 **죽는 현상(dying ReLU)**을 겪을 수 있습니다.
- **Tanh**: 시그모이드보다 넓은 출력 범위([-1, 1])를 가지며, 기울기 소실 문제가 덜하지만 여전히 존재합니다.

##### **역전파의 주요 개념 요약**

- **체인 룰**을 통해 오차를 각 층에 걸쳐 전파하고, 각 가중치에 대한 오차 기울기를 계산합니다.
- **순방향 전달**로 예측 값을 얻고, **역방향 전달**로 오차를 각 가중치에 반영합니다.
- 학습률을 사용해 가중치를 조정하여, 네트워크가 점차적으로 오차를 줄여 나갑니다.

##### **역전파의 직관적 이해**

역전파는 다음과 같이 생각할 수 있습니다:

- **투석기 조정 비유**: 역전파는 마치 투석기의 각 부분을 조정해 목표물을 정확히 맞추는 과정과 유사합니다. 발사 후 결과를 보고 그에 따라 조정하면서, 점점 목표에 가까워지는 방식입니다.
- **피드백을 통한 학습**: 역전파는 사람이 실수를 통해 학습하는 방식과 비슷합니다. 실수를 통해 피드백을 받고, 그 피드백을 이용해 다음 번에는 더 나은 결과를 얻도록 행동을 조정하는 과정입니다.

**역전파는 딥 러닝의 핵심 학습 메커니즘**으로, 네트워크가 스스로 학습하고 더 나은 예측을 할 수 있도록 하는 필수적인 알고리즘입니다. 이를 통해 다층 신경망이 매우 복잡한 데이터의 패턴을 학습하고, 다양한 AI 응용 분야에서 뛰어난 성능을 발휘할 수 있게 됩니다.

### 2. 깊은 신뢰 신경망 (DBNs) (2006)

2006년, 힌튼은 기울기 소실 문제를 극복하기 위해 계층별 사전 훈련을 사용하는 **깊은 신뢰 신경망(DBNs)**을 소개했습니다.

![깊은 신뢰 신경망 이미지](figs/Hinton-papers-3.jpg)

#### DBNs 이해하기

DBNs를 각 블록이 RBM인 빌딩 블록 타워로 생각해 보세요. 네트워크는 아래에서 위로 구축되며, 각 층은 아래 층에서 받은 데이터를 표현하는 법을 학습합니다. 이 접근법을 통해 네트워크는 상위 층으로 올라갈수록 점점 더 추상적인 특징을 학습할 수 있습니다.

예를 들어, 이미지 인식에서:

- 첫 번째 층은 가장자리를 감지하는 법을 배울 수 있습니다.
- 두 번째 층은 이러한 가장자리를 조합하여 간단한 형태를 인식할 수 있습니다.
- 더 높은 층은 얼굴이나 물체와 같은 더 복잡한 패턴을 인식할 수 있습니다.

이러한 계층적 학습은 DBNs를 복잡한 패턴 인식 작업에 특히 효과적으로 만듭니다.

### 3. RBMs를 이용한 차원 축소 (2006)

또한 2006년에 힌튼과 루슬란 살라후트디노프는 RBMs를 **차원 축소**에 사용하는 방법을 보여주었습니다.

![차원 축소 이미지](figs/Hinton-papers-4.jpg)

#### 차원 축소 설명

도시의 크고 상세한 지도가 있지만 작은 종이에 표현해야 한다고 상상해 보세요. 가장 중요한 특징만 유지하면서 단순화해야 할 것입니다. 이것이 본질적으로 차원 축소가 데이터에 대해 하는 일입니다.

RBMs는 고차원 데이터(예: 이미지)를 가장 중요한 특징을 포착하는 저차원 표현으로 압축하는 법을 학습할 수 있습니다. 이는 후속 처리를 더 효율적으로 만들고 데이터 압축, 특징 추출, 심지어 새로운 데이터 샘플 생성과 같은 작업에 도움이 될 수 있습니다.

### 4. AlexNet (2012)

2012년, 힌튼은 학생인 알렉스 크리제프스키와 일야 서츠케버와 함께 ImageNet 대회에서 우승하고 컴퓨터 비전을 혁명화한 합성곱 신경망인 AlexNet을 개발했습니다.

![AlexNet 이미지](figs/Hinton-papers-5.jpg)

#### AlexNet의 영향

AlexNet은 여러 가지 이유로 게임 체인저였습니다:

1. **깊은 아키텍처**: 여러 합성곱 층을 사용하여 이미지에서 복잡한 특징을 학습할 수 있었습니다.
2. **GPU 가속**: GPU 컴퓨팅을 활용하여 이전보다 훨씬 더 큰 네트워크를 훈련시킬 수 있었습니다.
3. **새로운 기술**: ReLU 활성화와 드롭아웃과 같은 기술을 도입했는데, 이는 지금 많은 신경망에서 표준이 되었습니다.

AlexNet이 ImageNet 대회에서 성공(오류율을 26%에서 15.3%로 줄임)한 것은 컴퓨터 비전에서 딥 러닝 혁명의 시작을 알렸습니다.

### 5. 시각적 대조 학습 (2020)

2020년, 힌튼은 시각적 작업을 위한 **대조 학습**의 발전에 기여했습니다.

![시각적 대조 학습 이미지](figs/Hinton-papers-6.jpg)

#### 대조 학습 이해하기

대조 학습은 아이에게 물체를 인식하도록 가르치는 것과 비슷합니다. 이미지 쌍을 보여주고 "이것들이 같은 물체인가요?"라고 묻는 것과 같습니다. 시간이 지남에 따라 아이(또는 이 경우 신경망)는 다른 물체를 구별하는 핵심 특징을 식별하는 법을 배웁니다.

기계 학습의 맥락에서:

1. 네트워크에 같은 이미지의 다른 증강(회전이나 색상 변경 등)이 보여집니다.
2. 이러한 증강이 같은 기본 이미지를 나타낸다는 것을 인식하는 법을 배웁니다.
3. 이 과정은 네트워크가 라벨이 붙은 데이터 없이도 강건한 시각적 표현을 학습하는 데 도움을 줍니다.

이 접근법은 라벨이 붙은 데이터가 부족하거나 얻기 비싼 시나리오에서 특히 유용했습니다.

## 합성곱 신경망 (CNNs)

힌튼의 연구가 딥 러닝을 부활시킨 반면, 얀 르쿤과 다른 연구자들은 **합성곱 신경망(CNNs)**이라 불리는 특별한 종류의 신경망을 발전시켰습니다.

### CNNs의 작동 방식

큰 벽화를 보고 있다고 상상해 보세요. 전체 이미지를 한 번에 받아들이지 않고, 눈이 이미지를 가로질러 스캔하면서 다른 부분에 초점을 맞춥니다. CNNs도 비슷하게 작동합니다:

1. **합성곱 층**: 이는 슬라이딩 윈도우처럼 작동하여 이미지를 스캔하고 가장자리, 질감, 형태와 같은 특징을 감지합니다.
2. **풀링 층**: 특정 영역에서 감지된 특징을 요약하여 네트워크가 위치의 작은 변화에 더 강건해지도록 합니다.
3. **완전 연결 층**: 합성곱 층과 풀링 층이 학습한 고수준 특징을 가져와 최종 분류에 사용합니다.

이 아키텍처는 CNNs를 이미지 분류, 물체 감지, 심지어 얼굴 인식과 같은 작업에 특히 효과적으로 만듭니다.

## 순환 신경망과 LSTMs

텍스트나 시계열과 같은 순차 데이터를 처리하기 위해 연구자들은 **순환 신경망(RNNs)**과 그 더 발전된 변형인 **장단기 메모리(LSTM) 네트워크**를 개발했습니다.

### RNNs와 LSTMs 이해하기

RNN을 기억을 가진 네트워크로 생각해 보세요. 문장과 같은 시퀀스를 처리할 때, 현재 입력뿐만 아니라 이전에 본 것도 고려합니다. 하지만 기본 RNNs는 장기 의존성을 다루는 데 어려움을 겪습니다.

Sepp Hochreiter와 Jürgen Schmidhuber가 도입한 LSTMs는 더 정교한 메모리 셀을 사용하여 이 문제를 해결합니다. 정보를 선택적으로 기억하거나 잊을 수 있어, 장기 의존성을 더 효과적으로 포착할 수 있습니다.

예를 들어, 언어 번역에서 LSTM은 주어가 많은 단어로 동사와 분리되어 있더라도 문장의 주어를 기억할 수 있어, 번역의 문법적 정확성을 보장합니다.

## 트랜스포머 네트워크

2017년, Vaswani 등은 **트랜스포머 아키텍처**를 소개했는데, 이는 많은 최첨단 언어 모델의 근간이 되었습니다.

### 트랜스포머의 작동 방식

트랜스포머는 **자기 주의(self-attention)**라는 메커니즘을 사용하여 각 요소를 처리할 때 입력의 다른 부분의 중요성을 가중치로 부여할 수 있습니다.

파티에서 여러 대화를 따라가려고 하는 상황을 상상해 보세요. 당신에게 관련 있는 것에 기반하여 다른 화자들에게 주의를 기울입니다. 트랜스포머도 비슷하게 작동합니다:

1. 문장의 각 단어에 대해, 트랜스포머는 다른 모든 단어에 얼마나 주의를 기울일지 계산합니다.
2. 이를 통해 단어들 사이의 복잡한 관계를 포착할 수 있으며, 문장 내에서 멀리 떨어져 있더라도 가능합니다.
3. RNNs와 달리, 트랜스포머는 모든 단어를 병렬로 처리할 수 있어 훈련 속도가 훨씬 빠릅니다.

트랜스포머는 기계 번역, 텍스트 생성, 질문 답변과 같은 NLP 작업을 혁신했습니다. BERT와 GPT와 같은 모델들은 트랜스포머 아키텍처를 기반으로 하며, 언어 이해와 생성에 새로운 기준을 세웠습니다.

## CNNs와 트랜스포머 비교

**합성곱 신경망(CNNs)**과 **트랜스포머(Transformers)**는 인공지능 분야에서 가장 널리 사용되는 두 가지 신경망 아키텍처입니다. CNNs는 **컴퓨터 비전**에 특화된 신경망으로, 이미지와 비디오 데이터를 처리하는 데 뛰어난 성능을 보입니다. 반면, 트랜스포머는 주로 **자연어 처리(NLP)** 분야에서 두각을 나타냈지만, 최근에는 **컴퓨터 비전** 분야에도 적용되고 있습니다. 이 두 아키텍처를 비교하기 위해 주요 특성과 장단점을 설명하겠습니다.

### **1. 합성곱 신경망 (CNNs)**

CNNs는 이미지 데이터를 다루는 데 매우 효과적입니다. 이는 **합성곱 연산**을 통해 이미지의 국소적인 패턴을 추출하고, 그 정보를 바탕으로 특징을 학습합니다.

- **구조**:

  - **합성곱 층 (Convolutional Layers)**: 입력 이미지를 작은 영역(커널 또는 필터)으로 나누고, 이 작은 영역의 **합성곱 연산**을 통해 특징 맵(feature map)을 생성합니다.
  - **풀링 층 (Pooling Layers)**: 특징 맵의 차원을 줄여 연산량을 줄이고, 주요 특징만을 남겨 네트워크의 성능을 높입니다.
  - **완전 연결 층 (Fully Connected Layers)**: 마지막 단계에서 분류나 회귀 작업을 수행하기 위해, 추출된 특징을 결합하여 최종 출력을 만듭니다.

- **장점**:

  - **지역 패턴 탐지(Local Pattern Detection)**: 합성곱 연산은 이미지의 국소적인 패턴(예: 가장자리, 텍스처 등)을 매우 효과적으로 감지합니다.
  - **공간 불변성(Translation Invariance)**: CNNs는 필터를 통해 같은 패턴을 이미지의 어디에서든 동일하게 인식할 수 있습니다. 이는 **물체의 위치 변화**에 강건한 특징입니다.
  - **효율적 특징 추출**: 이미지의 국소적 특징을 추출하는 데 최적화되어 있어 **컴퓨터 비전** 문제에 특히 적합합니다.

- **단점**:
  - **장거리 의존성 학습의 어려움**: CNN은 이미지나 데이터의 국소적인 패턴을 주로 학습하기 때문에, **장거리 의존성(Long-Range Dependency)**을 포착하는 데 한계가 있습니다. 예를 들어, 이미지의 멀리 떨어진 두 물체 간의 관계를 학습하기 어렵습니다.
  - **고정된 커널 크기**: 필터 크기가 고정되어 있어, 다양한 스케일의 패턴을 학습하는 데 제한이 있습니다.

### **2. 트랜스포머 (Transformers)**

트랜스포머는 NLP에서 출발하여 **자기 주의(Self-Attention)** 메커니즘을 통해 데이터를 처리합니다. 입력 시퀀스의 모든 요소가 서로 주의(attend)할 수 있는 구조 덕분에, 각 요소 간의 복잡한 관계를 효율적으로 학습할 수 있습니다.

- **구조**:

  - **자기 주의(Self-Attention) 메커니즘**: 입력의 각 요소가 다른 모든 요소와의 관계를 학습할 수 있도록 함으로써 **문맥적 정보**를 효율적으로 포착합니다.
  - **인코더-디코더 구조**: 주로 기계 번역 같은 시퀀스-투-시퀀스 작업에서 사용됩니다. 하지만, NLP에서의 BERT나 GPT처럼 인코더 또는 디코더 하나만을 사용하여 특정 작업에 특화되기도 합니다.
  - **병렬 처리 가능성**: 트랜스포머는 시퀀스 전체를 동시에 처리할 수 있어, RNN과 달리 병렬화가 가능합니다.

- **장점**:

  - **장거리 의존성 학습**: 자기 주의 메커니즘을 통해 트랜스포머는 멀리 떨어진 요소들 사이의 관계를 잘 포착할 수 있습니다. 이는 **텍스트 내에서 장기 의존성(Long-Term Dependencies)**을 학습하는 데 매우 유리합니다.
  - **병렬 처리 효율성**: 모든 요소를 동시에 처리할 수 있어, 긴 시퀀스를 처리할 때 훨씬 빠르게 학습할 수 있습니다.
  - **유연성**: 트랜스포머는 텍스트뿐 아니라, **컴퓨터 비전, 음성 인식, 시계열 데이터** 등 여러 분야에 확장 가능하며, 각기 다른 도메인에서도 효과적으로 적용되고 있습니다.

- **단점**:
  - **고비용 계산**: 자기 주의 메커니즘은 **메모리 사용량과 계산 비용**이 매우 크며, 시퀀스 길이가 길어질수록 비용이 급격히 증가합니다.
  - **복잡성**: CNN에 비해 모델이 더 복잡하고 많은 계산 자원이 필요하여, 대규모 학습에 적합한 인프라가 요구됩니다.

### **3. CNNs vs Transformers: 주요 차이점 및 비교**

| **특성**             | **CNNs**                                      | **Transformers**                                      |
| -------------------- | --------------------------------------------- | ----------------------------------------------------- |
| **데이터 유형**      | 이미지, 비디오 등 공간적 구조가 중요한 데이터 | 텍스트, 이미지, 시계열 데이터 등 순차적/관계적 데이터 |
| **특징 학습 방식**   | 국소적 특징 학습 (지역 필터 사용)             | 전체적인 문맥 학습 (자기 주의)                        |
| **처리 방식**        | 순차적 특징 추출 (필터 이동)                  | 병렬 처리 (자기 주의)                                 |
| **장거리 관계 학습** | 어려움 (국소적 학습에 특화)                   | 우수 (자기 주의를 통한 관계 학습)                     |
| **적용 분야**        | 컴퓨터 비전 (물체 인식, 이미지 분류)          | NLP (기계 번역, 텍스트 생성), 컴퓨터 비전(최근)       |
| **계산 효율성**      | 비교적 적은 계산 자원 필요                    | 많은 계산 자원 필요, 시퀀스 길이에 따른 복잡도 증가   |

### **4. 트랜스포머의 CNN 대체 시도**

최근 연구에서는 트랜스포머를 **컴퓨터 비전**에 적용하려는 다양한 시도가 이어지고 있습니다. 대표적으로 **Vision Transformer (ViT)**는 이미지를 패치로 나누고, 각 패치를 시퀀스로 간주하여 트랜스포머를 적용합니다.

- **CNN과의 비교**:
  - **ViT**는 기존 CNN과 달리, 이미지 전체를 나누어 **자기 주의**를 통해 각 패치 간의 관계를 학습합니다. 이를 통해 이미지의 장거리 의존성을 더 잘 포착할 수 있으며, 이는 특히 복잡한 이미지 분류 문제에서 유리합니다.
  - 그러나 **데이터 효율성** 측면에서는 CNN이 여전히 강점을 지니고 있습니다. ViT와 같은 트랜스포머 기반 모델은 **매우 큰 데이터셋**을 필요로 하며, 데이터가 부족할 경우 CNN보다 성능이 저하될 수 있습니다.

### **5. 결론**

- **CNNs**는 이미지 처리에서 뛰어난 성능을 보이며, 국소적인 패턴을 잘 학습하여 **공간 불변성**을 갖는 특성으로 컴퓨터 비전 작업에 적합합니다.
- **트랜스포머**는 텍스트 내 **장거리 의존성**을 학습하고, 병렬 처리가 가능해 NLP 작업에서 널리 사용되고 있습니다. 최근에는 컴퓨터 비전에도 성공적으로 적용되어, 장거리 의존성을 포착하는 능력을 활용한 모델들이 등장하고 있습니다.

CNNs와 트랜스포머는 서로 다른 강점을 가지고 있으며, 많은 경우 특정 문제에 따라 이 두 가지 모델을 결합하거나 번갈아 사용하는 것이 효과적일 수 있습니다. 예를 들어, 컴퓨터 비전의 일부 문제에서는 CNN으로 초기 특징을 추출한 뒤 트랜스포머로 장거리 관계를 학습하는 방식이 사용될 수 있습니다.

## 응용 및 미래 방향

신경망 아키텍처의 발전은 다양한 분야에서 획기적인 응용을 이끌어냈습니다:

- **이미지 및 음성 인식**: CNNs는 이미지에서 정확한 물체 감지와 실시간 음성 인식을 가능하게 했습니다.
- **자연어 처리**: 트랜스포머 기반 모델들은 고급 챗봇, 기계 번역 시스템, 심지어 AI 글쓰기 어시스턴트를 구동합니다.
- **헬스케어**: 딥 러닝 모델들은 의료 영상 분석, 신약 발견, 개인화된 의학에 사용되고 있습니다.
- **자율 주행 차량**: CNNs와 다른 딥 러닝 모델들은 자율 주행 차량의 인지와 의사 결정에 중요합니다.

이러한 기술들이 계속 발전함에 따라, 우리는 점점 더 인간과 비슷한 방식으로 세상을 이해하고 상호작용할 수 있는 더욱 정교한 AI 시스템을 보게 될 것입니다.

## 주요 요점

1. **계층별 훈련**: 힌튼이 선구적으로 개발한 이 기술은 깊은 네트워크 훈련의 도전과제를 극복하는 데 중요했습니다.
2. **전문화된 아키텍처**: 시각적 작업을 위한 CNNs와 순차 데이터를 위한 트랜스포머는 AI가 복잡한 실제 정보를 처리하는 능력을 극적으로 향상시켰습니다.
3. **확장성과 효율성**: 트랜스포머와 같은 현대적 아키텍처는 거대한 데이터셋에서 엄청나게 큰 모델을 훈련시키는 것을 가능하게 만들어, AI가 달성할 수 있는 것의 경계를 넓혔습니다.
4. **학제간 영향**: 신경망의 진화는 헬스케어에서 자율 시스템에 이르기까지 수많은 분야에 광범위한 영향을 미쳤습니다.

우리가 이러한 아키텍처를 계속 개선하고 새로운 것을 개발함에 따라, AI의 잠재적 응용 분야는 확장될 것이며, 앞으로 몇 년 동안 흥미진진한 발전을 약속합니다.
