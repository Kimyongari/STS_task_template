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

대회에서는 훈련된 BERT 모델을 통한 **Random Insertion** 방법과 **Back Translation**을 사용하였습니다.
추가적으로, sentence_1과 sentence_2의 순서를 바꾸는 증강 또한 유의미하다고 판단하여 같이 진행합니다.

이후, 증강된 데이터에도 오타나 잘못된 띄어쓰기가 존재할 수 있으므로 통일성을 위해 Preprocessing을 다시 수행하는 과정을 거쳤습니다.

### **데이터는 네이버 부스트캠프의 고유 저작물이기에 공유가 불가능하여 증강의 결과물을 올리지 않았습니다.**
### 위와 같은 과정은 Augumentation.ipynb를 통해 자세하 살펴보실 수 있습니다.

## 4. Modeling
텍스트 데이터의 가공을 마치고 증강을 톹헌 데이터의 다양성 및 Label의 분포를 맞춘 뒤에 Modeling을 착수하였습니다.

### 모델 구성

### 1. Dataset
> 데이터의 한 행을 받으면 sentence_1과 sentence_2 사이에 [SEP] 토큰을 추가하여 붙인 뒤, tokenizer를 통해 토큰화 한 결과값 중 label_ids와 attention_mask를 내보냅니다. 추가적으 token_type_ids까지 내보낼 수 있지만, 호환이 안 되는 모델이 존재할 수 있습니다.

### 2. Make Dataset
>make_dataset을 통해 원하는 데이터 형태를 받을 수 있습니다. mode를 통해 단순 train인지, k-fold train인지, test인지 구별하면 그에 맞는 데이터셋을 내보냅니다.

### 3. MyModel
> 모델은 Transformer기반 모델에서 전체 문맥의 정보를 갖는 CLS token의 임베딩 벡터를 기준으로 두 문장간의 유사성을 벡터 형태로 내보냅니다.
이후 1DCNN Block을 추가적으로 거쳐 하나의 값의 형태로 output을 내보냅니다.
1DCNN을 사용한 이유는 문장간의 유사성은 문장 중 **특정 단어**하나하나에 따라 유사성이 달라지는 경우가 많다고 판단되었기에 지역적 특성을 좀 더 잘 나타낼 수 있는 1DCNN을 활용하였습니다.

### 4. Train
> KFold train, 또는 train을 통해 매 epoch, step마다 validation을 수행합니다.
parser의 파라미터들을 조정하여 output_dir, 하이퍼파라미터등을 정할 수 있습니다.
model_list 에서는 bert, roberta의 base, large 모델 및 Electra 모델을 학습시켜
predict를 내보냅니다.
predict들을 앙상블에 활용하여 일반화 성능을 끌어올릴 수 있습니다.

### 5. Ensemble
> 본 대회에서는 lgb ensemble과 soft voting을 사용하였습니다.
soft voting은 여 모델의 결과값들을 동일한 가중치로, 또는 모델의 validation 성능을 기준으로 가중치를 매겨 합한 뒤 나눠 최종 prediction을 만듭니다.
lgb ensemble은 stacking의 일종으로 모델의 prediction들을 하나의 feature로 선정하여 lgb 모델을 학습시킨 뒤, predict를 통해 최종 prediction을 만듭니다.
**public score 기준 soft voting, hard voting을 사용하였을 때 보다 lgb ensemble을 사용하는 것이 성능이 약 0.5%정도 더 높았습니다.**
각 모델마다 강점, 단점이 있는 만큼 단순 voting보다는 틀린 부분에 가중치를 강하게 주는 lgb 모델이 ensemble에 효과적이라 판단하였습니다.

## 5. 결과
> 본 Template는 대회가 닫힌 뒤 작성하였기 때문에, Public, Private score에 대해서는 알 수 없습니다.
validation pearson score는 98 이상 기록하였지만, overfiting의 가능성이 있습니다.

## 6. 회고
1. EDA는 아주 **중요한** 작업이다.
> 이번 대회는 근소한 차이로 등수가 많이 나뉘는 task였습니다. 단순히 모델링만으로는 절대 성능을 올릴 수 없습니다. 데이터는 항상 깨끗한 상태로 주어지지 않고, Task에 맞도록 잘 가공하는 것의 중요성을 깨달았습니다.

2. KFold는 성능을 올리는 데에 도움이 되지 않을 수 있다.
> KFold training은 Validation data가 없거나 부족할 때 사용하는 방법론입니다. 보통 여러 모델을 학습시켜 일반화 성능을 올리는 데에 사용됩니다. 본 대회에서는 하나의 모델을 여러 fold로 학습시켰지만, 이는 overfiting의 가능성이 커질 뿐, 눈에 띄는 성능 향상을 이룰 수는 없었습니다.

3. 결과를 꼼꼼히 기록하자.
> STS task 뿐만 아니라 이후에 진행되는 MRC Task 전부 성능을 올리기 위해서는 철저한 결과 기록,분석이 필수적입니다.
본인이 했던 결과들은 금방 까먹기 쉽고, 다른 사람이 알아볼 수도 없습니다. 이후에 같은 실험을 반복하지 않도록 치밀한 결과 기록이 중요합니다.
잘되는 이유, 안되는 이유를 항상 곱씹으면서 맡은 일을 시간 내에 수행하고 공유하는 것이 협업에서의 꼭 필요한 능력임을 깨달았습니다.

4. Git을 두려워하지 말자.
> git이 꼬이는 것이 두려워 commit, push, pull을 안하는 것은 협업을 잘 못하는 개발자라고 생각합니다.
이번 프로젝트에서는 팀원간 git을 통한 공유가 서툴렀습니다.
결과적으로 Team play가 타 팀보다 미숙했다고 생각합니다.
문제를 피하는 것도 방법이 될 수 있지만, 어차피 계속 사용할 도구이기에 문제를 직면하고 해결하는 방법을 배워가는 과정이 필요하다고 생각했습니다.















