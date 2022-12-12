# Hotel_Review

## 진행기간 
- 2022.09.13. ~ 2022.09.20

## 목적
- **Text 기반으로 사람들의 MBTI를 분류하여 다양한 분야에 유용하게 사용.( 특히, 마케팅 분야)**  
          
## 코드 설명

   
코드     | 코드 링크   | 
:-------:|:-----------:|
텍스트 전처리|[Text_Preprocess](https://github.com/AliceecilA93/MBTI-Classification-based-on-Text-with-BERT/blob/main/Text_Preprocess.ipynb)|         
MBTI 이진분류 | [MBTI Classification (Binary)](https://github.com/AliceecilA93/MBTI-Classification-based-on-Text-with-BERT/blob/main/MBTI%20Classification%20(Binary_Kaggle).ipynb)|
MBTI 16분류| [MBTI Classification(16)](https://github.com/AliceecilA93/MBTI-Classification-based-on-Text-with-BERT/blob/main/MBTI%20Classification(16_Kaggle).ipynb)| 
Confusion Matrix| [MBTI Confusion Matrix](https://github.com/AliceecilA93/MBTI-Classification-based-on-Text-with-BERT/blob/main/MBTI_Confustion%20matrix.ipynb) |
Wordcloud| [MBTI Wordcloud](https://github.com/AliceecilA93/MBTI-Classification-based-on-Text-with-BERT/blob/main/MBTI_Wordcloud.ipynb)|          


## 사용된 데이터  

- Reddit data [final_2700.pkl](https://drive.google.com/uc?id=1--Kj5WvDiNmwzYwPp4L12LMuCqP0xqJU)
- Personality cafe data [kaggledata.pkl](https://drive.google.com/uc?id=1NmSV5Oaa2UIrSloWEQGgNvRyBRIDYNdA)

## 사용된 모델 

- BERT 
  * Transformer의 Encoder를 활용한 Language model 
  * Bidirectional Transformer Encoder를 가지고 있으며 대용량 Unlabled data로 모델을 미리 학습 시킨 후, 특정 task를 가지고 있는 labeled data로 학습하는 모델 
- DistillBERT
  * 기존 BERT model에 Distilling Knowldege 기법 적용 
    ==> 모델 경량화 + 추론 속도 높여주는 BERT model인 DistillBERT


## 과정  

 1. 개발환경 : Python, Tensorflow, Colab
 
 2. 데이터 전처리
     - 대문자로 라벨 통일 
     - 필요없는 열 제거 
     - null값 제거 
     - 중복제거 
     - 링크제거
     - Labeling dataset  

 3. 데이터셋
   
 데이터셋 | 데이터 갯수 | 
 :-------:|:-----------:|
 Reddit data (16분류)| 43,200 |         
 Reddit data (이진분류) | 80,000 |       
 Personality cafe data (16분류) | 8,675 |         
 Personality cafe data (이진분류)| 8,675 |        


 4. 데이터셋 분석
  
  - Reddit data
  
 ![image](https://user-images.githubusercontent.com/112064534/207056926-e59eaeab-7ab6-42b9-81ea-85bb9e1f8aa7.png)
 
  - Personality cafe data
  
![image](https://user-images.githubusercontent.com/112064534/207057062-fcad37af-0f87-4d72-8440-6c2966148573.png)

 ==> Reddit data의 라벨간 불균형이 심해 이를 해소하기위해서 데이터셋을 Reddit data에서 Personality cafe data로 변경하였지만 여전히 라벨간 불균형 하다는 것을 알 수 있다. 양으로 따지면 Reddit data가 훨씬 많지만 Personality cafe data의 질이 더 좋아 성능이 더 높았다. 
 
 
 5. 모델 분석 
 - Reddit dataset에서는 DistillBERT의 성능이 더 높았지만 Personality cafe dataset에서는 BERT + hidden layer 24로 Fine-tuning한 것이 높았다. 
 

## 결과
- Confusion Matrix를 확인했을 때 이진분류보다 이진분류가 수치상으로 더 높았다. 데이터의 양 균형과 질이 좋았다면 데이터 양에 치우치지 않고 더 좋은 결과가 기대된다. 수치적으로는 16분류가 더 높았으나 데이터의 불균형으로 이진분류의 결과를 고객 성향에 따른 마케팅에 활용 가능할 것 같다.  
    


## 참조
-DEVLIN, Jacob, et al. Bert: Pre-training of deep bidirectional transformers for language understanding. arXiv preprint arXiv:1810.04805, 2018.
https://doi.org/10.48550/arXiv.1810.04805 

-SANH, Victor, et al. DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter. arXiv preprint arXiv:1910.01108, 2019.
https://doi.org/10.48550/arXiv.1910.01108 
