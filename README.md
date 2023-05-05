# Text Generation: False Sentence Generation
- Using KoGPT: 입력을 넣었을 때, 그 다음 단어로 올 수 있는 후보들 중 가장 확률이 낮은 단어 5개를 뽑아 문장 생성
  ![image](https://user-images.githubusercontent.com/83209355/236360574-fe98d86c-a344-4c83-9026-66c5e8e0f2a8.png)
  - 순차적으로 다음에 올 가장 적절한 단어를 확률적으로 예측
  - ![image](https://user-images.githubusercontent.com/83209355/236360627-33a06bb2-e704-4ea5-836d-0349d05ecda5.png)
  - Top K Sampling: 가장 확률이 높은 K개의 다음 단어를 필터링
    => 적용할 방식: 1000개를 뽑아 가장 확률이 낮은 K개를 선택
(1) KoGPT를 이용한 학습 데이터 생성
- Pretrained Model: skt/kogpt2-base-v2
- Tokenizer: skt/kogpt2-base-v2
- 학습 데이터: 평가원 교과목 데이터 텍스트
![image](https://user-images.githubusercontent.com/83209355/236360751-01524f60-5466-4572-94c2-cbd62455edbc.png)

- ORIGINAL TEXT:  “봄이 오면 나무에는 파릇파릇한 싹이 돋아나고, 천지에 생명력이 가득하여 우리를 설레게 한다”
- idx0: 봄이
![image](https://user-images.githubusercontent.com/83209355/236360794-724e8eed-7f68-4a30-93eb-2d80cef818a7.png)
- idx1: 오면
![image](https://user-images.githubusercontent.com/83209355/236360804-51b1c93b-cd0d-4b6e-9677-78a0833ede44.png)
- idx2: 나무에는
![image](https://user-images.githubusercontent.com/83209355/236360819-2a24eaa4-41c3-4649-b39d-b586cb258a6a.png)

(2) Using LSTM: GPT를 이용해 생성된 문장 데이터를 이용해 False Sentence를 만들어주는 LSTM 모델 생성 
- 학습 데이터: GPT를 이용해 생성된 문장
- 생성된 학습 데이터를 바탕으로 Tokenizer(keras.preprocessing.text) 학습
- 하나의 문장을 여러 줄로 분해해 훈련 데이터 구성
- 임베딩 벡터 차원: 10
- 은닉층 크기: 128
- FC Layer를 출력층으로 단어 집합의 크기만큼 구성
- 활성화 함수: 소프트맥스 함수
- 손실 함수: 크로스 엔트로피 함수
![image](https://user-images.githubusercontent.com/83209355/236360909-c2508df0-d7f9-4a6b-bb67-7a4e6004edc6.png)
- idx0: 봄이
![image](https://user-images.githubusercontent.com/83209355/236360924-585b3f9a-cf18-411a-a944-70b9dfcffbab.png)
- idx0 + idx1: 봄이 오면
![image](https://user-images.githubusercontent.com/83209355/236360946-98a4872b-51b0-41fe-b360-fdc3eec131da.png)

  
