# Hotel_Review

## 진행기간 
- 2022.07.21 ~ 2022.08.04

## 목적
- **HTML, Beautifulsoup을 사용하여 비정형 데이터인 호텔 리뷰를 크롤링한 후 전처리하여 호텔리뷰를 통한 고객맞춤형 트렌드 분석 및 시각화**  
          
## 코드 설명
   
코드     | 코드 링크   | 
:-------:|:-----------:|
야놀자 크롤링|[Crawling_Yanolja](https://github.com/AliceecilA93/Hotel_Review/blob/main/Crawling/Crawling_Yanolja.ipynb)|      
여기어때 크롤링 | [Crawling_Good choice](https://github.com/AliceecilA93/Hotel_Review/blob/main/Crawling/Crawling_Good%20choice.ipynb)|
데일리호텔 크롤링| [Crawling_Daily hotel](https://github.com/AliceecilA93/Hotel_Review/blob/main/Crawling/Crawling_Daily%20hotel.ipynb)| 
주소 기반 호텔이름 통일화| [Unified hotel name based on address](https://github.com/AliceecilA93/Hotel_Review/blob/main/Crawling/Unified%20hotel%20name%20based%20on%20address.ipynb) |
키워드 기반 Wordcloud| [Detection keyword_Word cloud](https://github.com/AliceecilA93/Hotel_Review/blob/main/Data%20visualization/Detection%20keyword%20_%20Word%20cloud.ipynb)|
리뷰 카운팅| [Counting_review](https://github.com/AliceecilA93/Hotel_Review/blob/main/Data%20analysis/Counting_review.ipynb)|
토픽 모델링(N-gram)| [Topic_modeling](https://github.com/AliceecilA93/Hotel_Review/blob/main/Data%20analysis/Topic_modeling.ipynb)|


## 사용된 데이터  

- 야놀자 리뷰 [야놀자.csv](https://drive.google.com/file/d/19a8yAfbCKXIV_kWm6ln77K_sNM9zDjRx/view?usp=sharing)
- 여기어때 리뷰[여기어때.csv](https://drive.google.com/file/d/1frdR31d3AMMGn_BE2KdB5mXyL7dPnyH5/view?usp=share_link)
- 데일리호텔 리뷰[데일리호텔.csv](https://drive.google.com/file/d/1N9kPyLrzYVmcrTQjW0rlLz2wAb3tbQIP/view?usp=share_link)
- All-review [최종호텔리뷰통합.csv](https://drive.google.com/file/d/1BZE7WU9GRu6T-8vJZYEidjLjiOl618Gh/view?usp=share_link)

## 과정  

 1. 개발환경 : Python, Tensorflow, Colab
 
 2. 데이터 전처리
    * text_cleaning : 토큰화 / 품사 추출 / 중요한  한 글자 키워드만 남기고 한 글자 단어 삭제/ 불용어제거 
```c

def text_cleaning(text):
    hangul = re.compile('[^ ㄱ-ㅣ 가-힣]+')  # 정규 표현식 처리
    result = hangul.sub('', text)
    # okt 객체를 활용해서 형태소 토큰화 + 품사 태깅
    words = okt.pos(result , stem = True)
    # 명사, 동사, 형용사 추출
    words = [(token, pos) for token, pos in words if pos in ['Noun', 'Verb', 'Adjective']]
    # 한글자 키워드 제거& 살리기 
    words = [token for token, pos in words if len(token) > 1 or token in ['방','뷰','앞','옆','값','룸','싼']]
    # 불용어 제거
    words = [x for x in words if x not in stopwords]

    return words
     
```
   

   * 빈도수 기반 라벨별 키워드 


    위치 = ['위치', '바다', '바닷가', '렌트', '렌터', '렌트카', '렌터카', '식당', '카페', '편의점', '근처', '근접', '접근성', '맛집', '먹거리', '음식점', '시장', '시내', '도보', '교통', '버스', '택시','주변', '음식', '해수욕장', '공항', '시내', '시장', '가깝다','앞','옆','근처','근접하다','관광지']
    시설 = ['뷰','전망','조경','야경','경치','전경','풍경', '시설','부대','온천','루프','바베큐','헬스','세탁기','스파','냉장고','어메니티','피트니스', '온돌','마사지','사우나','라운지','캠핑','수영장','인피니티','테라스','주차장','주차공간', '컨디션','가구','욕조','이불','수압','침대','침구','온수','난방', '분위기','인테리어']
    서비스 = ['조식', '아침', '식사', '뷔페', '직원', '발렛', '보관', '정비', '룸서비스', '샌딩서비스', '청소', '프론트', '프런트', '로비', '리셉션', '다리미', '업그레이드' , '대여', '친절하다', '맛있다', '섬세하다', '안내', '친절', '업글', '응대']
    청결 = ['깨끗하다', '청결도', '소독', '청소','깔끔하다', '쾌적하다', '청결하다', '화장실', '깨끗' ,'청결' , '위생', '카페트', '카펫']
    가격 = ['가성', '가격', '대비', '값', '합리', '경제적', '비싸지않다', '저렴하다', '저렴', '싼', '싸다', '합리적']
    부정 = ['작다','부실하다','소음','층간소음','좁다','비좁다','비좁은','더럽다','실망','냄새','아쉬움','늦어지다','늦다','멀다','문제','번거롭다','찝찝하다','유료','딱딱하다','어수선하다','미끄럽다','부족하다','없이','시끄럽다', '불가','담배','곰팡이','불량하다','박하다','힘들다','춥다','덥다','심하다','오래되다', '이상하다', '잘못','짧다','담배','다소','어렵다','최악','불친절하다','무겁다','나쁘다','단점','막히다', '비싸다','작고','불편', '불편하다','어둡다','낡다','협소', '약하다','냄새', '머리카락', '물때','먼지', '건조하다','아쉽다','답답하다','별로','멀다', '없다']


 3. 데이터셋
   
 데이터셋 | 데이터 갯수 | 
 :-------:|:-----------:|
 All-review| 222,483 |         


 4. 라벨별 리뷰 카운팅 : 라벨간 키워드 수 불균형으로 인한 형평성 문제를 해결하기위함

```c
for i in tqdm(range(len(review))):
    review = my_tokenizer(review[i])
    for j in review:
        if j in price:
            total_hotel.loc[i,'price'] = 1
            break
        else:
            continue
```
  
 
 5. N-gram

  
  
        Topic 1: [('직원 친절하다', 2662.14), ('깨끗하다 직원', 932.96), ('공항 근처', 905.05), ('깔끔하다 친절하다', 542.43), ('직원 친절', 481.67), ('깔끔하다 직원', 412.25), ('깨끗하다 조식', 378.55)]​

        Topic 2: [('공항 가깝다', 2335.91), ('깨끗하다 친절하다', 1415.67), ('시설 깨끗하다', 1056.14), ('룸 컨디션', 719.42), ('넓다 깨끗하다', 647.12), ('깨끗하다 위치', 526.76), ('직원 친절하다', 476.97)]​

        Topic 3: [('조식 맛있다', 734.27), ('시설 깔끔하다', 535.28), ('시장 가깝다', 398.12), ('없다 아쉽다', 373.78), ('깨끗하다 뷰', 328.57), ('깨끗하다 공항', 325.03), ('침대 푹신하다', 317.3)]​

        Topic 4: [('오션 뷰', 1056.61), ('깨끗하다 조용하다', 668.41), ('깨끗하다 넓다', 580.01), ('바다 뷰', 539.31), ('깨끗하다 시설', 526.14), ('친절하다 깔끔하다', 449.91), ('조용하다 깨끗하다', 316.41)]​

        Topic 5: [('가격 대비', 2827.31), ('침구 깨끗하다', 1694.38), ('친절하다 깨끗하다', 1221.99), ('직원 친절하다', 739.62), ('저렴하다 가격', 536.66), ('위치 시설', 475.68), ('가격 저렴하다', 421.98)]


==> N-gram으로는 라벨별 키워드를 확인할 수가 없고 전체 리뷰에 어떤 내용(Topic)들이 많은지만 알려줘서 트렌드 분석이 어렵다고 판단




## 결과
- 본 프로젝트의 목적은 호텔리뷰 분석을 통한 고객맞춤형 운영방안을 제시하는 것이였으로 국내 호텔 예약 사이트에서 선정해놓은 Labels을 기반으로 전체 리뷰를 통해 어떤 단어들이 많은지 카운트 기반 빈도수 탐색 후 라벨별 키워드 사전을 만들었다. 이를 통해 라벨별로 리뷰에 점수를 매김으로써 각 호텔이 가지고 있는 장점이 무엇이고 개선할 점이 무엇인지 확인할 수 있었다. 또한 N-gram model을 사용하여 Topic을 뽑아내고자 하였지만 프로젝트의 방향성과 맞지 않아 사용할 수가 없었고 그 대안으로 라벨별로 리뷰에 점수를 주는 것으로써 프로젝트를 마무리 할 수 있었다. 




    
