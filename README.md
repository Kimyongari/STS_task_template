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
주어진 데이터는 sentence_1, sentence_2로 이루어진 두 문장과 0~5사이의 값을 갖는 label로 이루어져 있습니다.
데이터의 분포는 고루 퍼져있지 않고 0에 많이 편향된 모습을 보입니다.