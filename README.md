# STS_Task_template
## STS_Task_template입니다.
본 레포지토리는 네이버 부스트캠프 AI Tech 7기의 첫번째 프로젝트 STS(Semantic Text Similarity)에 대한 내용입니다.
NLP 10조의 순위는 11위로, 높은 순위는 아니지만 본 프로젝트들을 통해 배운 점을 기록하고자 레포지토리를 만들었습니다.

**이 Template은 대회가 끝나고 코드를 다듬어서 가공하였습니다.**
**제가 생각하고 적용한 부분들과 했어야 했지만 못한 것들의 나열이라 생각하시면 좋겠습니다.**

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

## 2. Data Preprocessing
EDA파트에서 분석한 내용을 바탕으로 데이터를 전처리합니다.
1. 많이 중복된 단어들을 두번의 반복으로 줄입니다.
   
2. 문장에서 등장하는 영어를 전부 소문자로 대체하고 필요한 특수문자를 제외한 나머지 특수문자를 제거합니다.

3. 네이버 맞춤법검사기를 통해 교정된 문장을 가져와 오타, 띄어쓰기를 교정합니다.

### 위와 같은 과정은 Preprocessing.ipynb를 통해 자세하게 살펴보실 수 있습니다.

## 3. Data Augumentation
주어진 데이터는 Label의 분포가 일정하지 않습니다.

Label의 분포를 고르게 맞추는 것은 모델의 성능 향상 측면에서 중요합니다.

대회에서는 훈련된 BERT 모델을 통한 Random Insertion 방법과 Back Translation을 사용하였습니다.
추가적으로, sentence_1과 sentence_2의 순서를 바꾸는 증강 또한 유의미하다고 판단하여 같이 진행합니다.

이후, 증강된 데이터에도 오타나 잘못된 띄어쓰기가 존재할 수 있으므로 통일성을 위해 Preprocessing을 다시 수행하는 과정을 거쳤습니다.

### **데이터는 네이버 부스트캠프의 고유 저작물이기에 공유가 불가능하여 증강의 결과물을 올리지 않았습니다.**
### 위와 같은 과정은 Augumentation.ipynb를 통해 자세하 살펴보실 수 있습니다.





    


















