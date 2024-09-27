# STS_Task_template
## STS_Task_template입니다.
본 레포지토리는 네이버 부스트캠프 AI Tech 7기의 첫번째 프로젝트 STS(Semantic Text Similarity)에 대한 내용입니다.
NLP 10조의 순위는 11위로, 높은 순위는 아니지만 본 프로젝트들을 통해 배운 점을 기록하고자 레포지토리를 만들었습니다.

**이 Template은 대회가 끝나고 코드를 다듬어서 가공하였습니다.**
**제가 생각하고 적용한 부분들의 나열이라 생각하시면 좋겠습니다.**

<과정>
1. EDA(Explomentary Data Analysis)
2. Data Augumentation
3. Data Preprocesisng
4. Modeling
5. Enssemble

## 1. EDA
데이터의 분포를 살피고, Preprocessing의 방향성을 잡습니다.

1. Label의 균형을 맞춰야 함.
> label의 개수가 0이 압도적으로 많은 분포를 보입니다.

2. 자주 등장하는 'PERSON' 단어를 Tokenizer마다 다르게 해석할 수 있으므로 하나의 Special Token으로 추가해야 함.
> '<PERSON>'이라는 단어가 자주 등장합니다. 이 단어는 하나의 Special 토큰으로 분류하도록 합니다. 다양한 인물들을 전부 하나의 토큰으로 대체함으로서 모델의 일반화 능력을 향상시킵니다.

3. 같은 단어들이 반복되는 문장들을 교정해야 함.
> '하하하하하하하하' 같은 문장들이 반복해서 등장합니다. 이런 문장들은 모델의 학습을 방해할 수 있으므로 단어를 줄이도록 합니다.

4. 띄어쓰기 교정을 통해 올바른 문장으로 바꿔야 함.
> '어제본영화는재미가없어'와 같이 띄어쓰기가 이루어지지 않은 문장들이 존재합니다. 띄어쓰기를 교정하여 올바른 문장으로 바꾸도록 합니다.

5. 오타로 인한 [UNK] Token 생성을 줄여야 함.
> '왜이렇게' ➡️ '왤케' 등 오타, 줄임말을 통한 ['UNK'] 토큰이 존재합니다. 단어를 교정하여 ['UNK'] 토큰을 줄이도록 합니다.


### 위와 같은 Preprocessing 과정을 설립하는 부분에 대해서는 EDA.ipynb를 통해 근거를 확인해보실 수 있습니다.

## 2. Data Augumentation
데이터를 증강하여 모델이 좀 더 일반성을 가질 수 있도록 데이터의 분포를 고르게 하고, 다양한 표현을 익힐 수 있도록 합니다.


















