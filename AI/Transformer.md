# 1. Self-Attention

```RNN이 꼭 필요할까?```
- RNN은 정보를 hidden state로 전달하며 순차적 의존선 형성
- 그에 반해 Attention은 필요한 순간, 입력 전체에서 직접 정보 전달

-> RNN이 하던 '정보 전달'을 Attention이 더 효율적으로 수행할 수 있다면 굳이, recurrence가 필요할까?

```RNN의 한계점```

### 1. 장기 의존성
- RNN은 왼쪽에서 오른쪽을 순차 전개되어 먼 단어 쌍이 상호작용하려면 시퀀스 길이만큼 단계를 거침
- 길어진 단계만큼 기울기 소실 혹은 폭발 문제가 발생해 장기 의존성 학습 어려움
- 입력된 선형 순서를 강하게 반영해 언어의 비선형적 의존성을 잘 잡애니 못함.
  -예시: 그 책은 오랫동안 방치되어 먼지가 많이 쌓인 탓에 더러웠다 -> 책이 더럽다를 잡지 못함 

### 2. 병렬화
- Forward와 Backward pass 모두 시퀀스 길이 만큼 단계 필요
- 순차적 연산이 진행되기에 병렬화 불가
- 병렬 연산에 강한 GPU 활용 어렵게 만들고, 대규모 데이터 학습 비효율적

```Attention은?```
- Attention은 각 단어 표현을 query로 두고 value 집합으로부터 필요한 정보 직접 불러와 결합
- 이러한 Attention 메커니즘을 encoder-decoder 간이 아닌, 한 문장 내부에서 적용한다면? -> Self-Attention

```Self-Attention의 장점```
1. 순차적으로 처리해야 하는 연산 수가 시퀀스 길이에 따라 증가X
2. 최대 상호작용 거리 = O(1) -> 모든 단어가 각 층에서 직접 상호작용

Self-Attention - Key, Query, Value
- Attention에서 각 단어 i를 표현하는 Query와 Value 벡터가 있었듯, Self-Attention에서는 각 단어 i를 표현하는 Query, Key, Value 벡터가 존재한다.

1. 각 단어를 Query, Key, Value 벡터로 변환한다.
- Query 벡터
  - 단어 i가 다른 단어로부터 ***어떤 정보***를 찾을 지 정의하는 벡터
- Key 벡터
  - 단어 i가 자신이 가진 ***정보의 특성***을 표현하는 벡트
- Value 벡터
  - 실제로 참조되는 ***정보 내용***을 담고 있는 벡터

2. Query, Keys 간의 유사도를 계산해, softmax로 확률분포를 구한다.
3. 각 단어의 출력을 Values의 가중합으로 계산한다.

```Self-Attention의 한계```
- 이렇듯, Self-Attention은 단어 간 관계를 효율적으로 잡아내는 강력한 메커니즘이지만 한계 존재

1. 순서 정보 부재 -> 단어 간 유사도만 계산하기 때문에 단어 순서 고려 x
2. 비선형성 부족 -> Attention 계산은 본질적으로 가중 평균 연산이라는 선형 결합에 불과하기 때문에 복잡한 패턴이나 깊은 표현력을 담기 어렵다.
3. 미래 참조 문제 -> 언어 모델은 시퀀스를 왼쪽에서 오른쪽으로 생성해야 하지만, Self-Attention은 모든 단어를 동시에 보기 때문에 아직 생성되지 않아야 할 미래 단어를 참조한다.

```Self-Attention의 한계 해결```
### 1. Positional Encoding
- 순서 정보 부재 문제를 해결하기 위해, Positional Encoding이라는 기법 사용
- 각 단어 위치 i를 나타내는 위치 벡터를 정의해 단어 임베딩 값에 더해 최종 입력으로 사용

- 두 가지 방법 존재
  - Sinusoidal Position Encoding
    - 서로 다른 주기의 사인/코사인 함수를 합성해 위치 벡터를 만들기
  - Learned Absolute Position Embedding
    - 위치 벡터를 모두 학습 파라미터로 설정해 학습 과정에서 데이터에 맞춰 최적화하기

### 2. Feed-Forward-Network 추가하기
- Self-Attention 연산은 비선형 변환이 없어, 복잡한 패턴 학습에 한계 존재
- 각 단어 출력 벡터에 Feed-Forward Network(Fully Connected + ReLU 등)을 추가해 Self-Attention이 만든 표현을 깊고 비선형적 표현으로 확장

 ### 3. Masked Self-Attention
 - Self-Attention은 기본적으로 모든 단어(미래 포함)을 동시에 참조
 - Attention Score를 계산할 때, 미래 단어에 해당하는 항목을 -무한대로 설저앻 계산 수행 시 반영않도록 한다

```Self-Attention 정리```
- Self-Attention은 문장 내 모든 단어가 서로 직접 상호작용하여

1. 장거리 의존성을 효율적으로 포착하고
2. 병렬 처리를 가능하게 하는 메커니즘

- 한계 해결 방법
  1. 순서 정보 부재 -> Positional Encoding
  2. 비선형성 부족 -> Feed-Forward Network 추가
  3. 미래 참조 문제 -> Masked Self-Attention

# 2. Transformer
- Transformer: Google의 "Attention Is All You Need"라는 논문에서 처음 제안된 아키텍처로 Self-Attention을 핵심 메커니즘으로 하는 신경망 구조  
  (이 때부터 논문 이름을 좀 웃기게 짓기 시작함ㅋㅋ 센스 넘치게 그냥)

```Transformer```
- Transformer는 encoder-decoder 구조로 설계된 신경망 모델
  - encoder: 입력 문장을 받아 의미적 표현으로 변환 수행
  - decoder: 인코더의 표현과 지금까지 생성한 단어들을 입력 받아 다음 단어 예측
=> 이 중 decoder가 언어 모델과 같은 방식으로 동작

```Multi-Headed Attention```
- 문장에서 같은 단어라도 여러 이유(문법적 관계, 의미적 맥락 등)로 다른 단어에 주목할 수 있음
- 단일 Self-Attention Head로는 한 가지 관점에서의 단어 간 관계밖에 파악할 수 없다.
=> 따라서 여러 Attention Head를 두어 다양한 관점에서 동시에 정보 파악

example: 문맥적 관계 attention head1 / 시제 attention head2 / 명사 attention head3

```Scaled Dot Product```
- Query와 Key의 차원이 커질수록 두 벡터의 내적 값도 자연스레 커짐
- 이 값이 너무 크면 softmax 함수 출력이 지나치게 뾰족해져 미세한 변화에도 큰 차이 발생 
=> gradient vanishing
- 따라서 내적 값을 그대로 사용하지 않고 나눠주어 스케일을 조정
- 값이 안정적으로 분포되어 학습이 훨씬 빠르고 안정적으로 진행

```Residual Connection```
- 깊은 신경망은 층이 깊어질수록 학습이 어려워진다
- 단순히 Layer의 출력만 사용하면 정보 소실로 layer가 전체를 예측하는 것이 아니라 기존 입력과 차이만 학습하도록하는 residual connection을 사용한다

```Layer Normalization```
- 층이 깊어질수록, hidden vector 값들이 커졌다 작아졌다 하면서 학습 불안정해진다
- Layer Normalization은 각 레이어 단위에서 hidden vector 값을 정규화해 안정적이고 빠른 학습 돕는다

```Decoder```
- Transformer의 decoder는 여러 개의 decoder 블록들을 쌓아 올려 만든 구조이다.
- 각 블록은 다음으로 구성
  - Masked Self-Attention(Multi-Head)
    - 미래 단어를 보지 않도록 마스크를 싀운 Multi-Head Self-Attention
  - Add & Norm(Residual Connection + Layer Normalization)
  - Feed-Forward Network
    - 각 위치 별 비선형 변환을 수행한다.

-> 언어 모델처럼 단방햔 문맥만 활용

```Encoder```
- 반면 Transformer의 encoder는 양방향 문맥을 모두 활용한다
  - 입력 문장을 의미적 표현으로 변환
  - 각 단어가 양방향 문맥을 모두 반영한 벡터로 인코딩
- Decider와의 차이점은 Self-Attention에서 masking을 제거한 것 뿐