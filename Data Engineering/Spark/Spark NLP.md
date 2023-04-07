# Spark NLP
Spark NLP는 John Snow Labs에서 제작한 Apache Spark를 기반으로 구축된 오픈 소스 자연어 처리 라이브러리입니다.

명명된 엔터티 인식, 감정 분석, 텍스트 분류 등과 같은 고급 NLP 기술을 사용하여 대량의 텍스트 데이터를 처리하기 위한 확장 가능한 분산 인프라를 제공합니다.

Spark NLP는 NLP 파이프라인 구축 및 배포 프로세스를 간소화하는 고급 API를 통해 사용하기 쉽게 설계되어있고 일반적인 NLP 작업을 위한 다양한 사전 구축된 모델 및 파이프라인은 물론 특정 사용 사례에 맞게 모델을 사용자 지정하고 교육하는 기능이 제공됩니다.

Spark NLP의 가장 큰 장점은 분산 컴퓨팅 클러스터에서 실행할 수 있어 사용자가 대량의 텍스트 데이터를 빠르고 효율적으로 처리할 수 있다는 것입니다. 또한 아파치 카프카와 같은 다른 빅데이터 기술과 잘 통합되어 분산 컴퓨팅 환경에서 NLP 애플리케이션을 구축하기 위한 도구로 적합합니다.


<br>


## PretrainedPipeline 모델을 불러와 간단한 영문 감성 분석 진행

```python
import sys

from pyspark.sql import SparkSession
from pyspark.ml import Pipeline
from pyspark.sql.functions import array_contains
from pyspark.ml import Pipeline, PipelineModel

from sparknlp.annotator import *
from sparknlp.pretrained import PretrainedPipeline


import sparknlp

# SparkSession을 만들고 Spark NLP에 대한 설정으로 간단 구성함
spark = sparknlp.start()


# analyze_sentiment라는 사전 훈련된 NLP 모델을 사용하여 pipeline 객체 생성
pipeline = PretrainedPipeline("analyze_sentiment", lang="en")


testDocs = [
    "I felt so disapointed to see this very uninspired film. I recommend others to awoid this movie is not good.",
    "This was movie was amesome, everything was nice."]


result = pipeline.annotate(testDocs)
[(r['sentence'], r['sentiment']) for r in result]


print(result)

```

```
결과값 반환

[{'checked': ['I','felt', 'so', 'disappointed', 'to', 'see', 'this', 'very', 'uninspired', 'film', '.', 'I', 'recommend', 'others', 'to', 'avoid', 'this', 'movie', 'is', 'not', 'good', '.'], 'document': ['I felt so disapointed to see this very uninspired film. I recommend others to awoid this movie is not good.'], 'sentiment': ['positive', 'negative'], 'token': ['I', 'felt', 'so', 'disapointed', 'to', 'see', 'this', 'very', 'uninspired', 'film', '.', 'I', 'recommend', 'others', 'to', 'awoid', 'this', 'movie', 'is', 'not', 'good',
'.'], 'sentence': ['I felt so disapointed to see this very uninspired film.', 'I recommend others to awoid this movie is not good.']}

,{'checked': ['This', 'was', 'movie', 'was', 'awesome', ',', 'everything', 'was', 'nice', '.'], 'document': ['This was movie was amesome, everything was nice.'], 'sentiment': ['negative'], 'token': ['This', 'was', 'movie', 'was', 'amesome', ',', 'everything', 'was', 'nice', '.'], 'sentence': ['This was movie was amesome, everything was nice.']}]

```

<br>


## John Snow Labs에서 제공하는 한국어 감성 분석 모델
https://sparknlp.org/2022/09/14/electra_classifier_ko_base_v3_generalized_sentiment_analysis_ko.html
https://huggingface.co/jaehyeong/koelectra-base-v3-generalized-sentiment-analysis


```python

from transformers import AutoTokenizer, AutoModelForSequenceClassification, TextClassificationPipeline

# load model
tokenizer = AutoTokenizer.from_pretrained("jaehyeong/koelectra-base-v3-generalized-sentiment-analysis")
model = AutoModelForSequenceClassification.from_pretrained("jaehyeong/koelectra-base-v3-generalized-sentiment-analysis")
sentiment_classifier = TextClassificationPipeline(tokenizer=tokenizer, model=model)

# target reviews
review_list = [
    '이쁘고 좋아요~~~씻기도 편하고 아이고 이쁘다고 자기방에 갖다놓고 잘써요~^^',
    '아직 입어보진 않았지만 굉장히 가벼워요~~ 다른 리뷰처럼 어깡이 좀 되네요ㅋ 만족합니다. 엄청 빠른발송 감사드려요 :)',
    '재구매 한건데 너무너무 가성비인거 같아요!! 다음에 또 생각나면 3개째 또 살듯..ㅎㅎ',
    '가습량이 너무 적어요. 방이 작지 않다면 무조건 큰걸로구매하세요. 물량도 조금밖에 안들어가서 쓰기도 불편함',
    '한번입었는데 옆에 봉제선 다 풀리고 실밥도 계속 나옵니다. 마감 처리 너무 엉망 아닌가요?',
    '따뜻하고 좋긴한데 배송이 느려요',
    '맛은 있는데 가격이 있는 편이에요'
]

# predict
for idx, review in enumerate(review_list):
  pred = sentiment_classifier(review)
  print(f'{review}\n>> {pred[0]}')
  
```

```
결과값 반환

이쁘고 좋아요~~~씻기도 편하고 아이고 이쁘다고 자기방에 갖다놓고 잘써요~^^
>> {'label': '1', 'score': 0.9906477332115173}
아직 입어보진 않았지만 굉장히 가벼워요~~ 다른 리뷰처럼 어깡이 좀 되네요ㅋ 만족합니다. 엄청 빠른발송 감사드려요 :)
>> {'label': '1', 'score': 0.9912862181663513}
재구매 한건데 너무너무 가성비인거 같아요!! 다음에 또 생각나면 3개째 또 살듯..ㅎㅎ
>> {'label': '1', 'score': 0.9904279708862305}
가습량이 너무 적어요. 방이 작지 않다면 무조건 큰걸로구매하세요. 물량도 조금밖에 안들어가서 쓰기도 불편함
>> {'label': '0', 'score': 0.9968683123588562}
한번입었는데 옆에 봉제선 다 풀리고 실밥도 계속 나옵니다. 마감 처리 너무 엉망 아닌가요?
>> {'label': '0', 'score': 0.9988909363746643}
따뜻하고 좋긴한데 배송이 느려요
>> {'label': '0', 'score': 0.5224758386611938}
맛은 있는데 가격이 있는 편이에요
>> {'label': '1', 'score': 0.7503753900527954}

label 0 : negative review
label 1 : positive review

```

한국어 감성분석 결과는 긍부정 결과만 제공하고 중립 결과는 제공되지 않음 만약에 중립 결과를 표현하고 싶을 때 score를 활용해야 합니다.
또한 기존의 감성분석에서 제공되던 형태소 분석이 따로 제공되지 않기 때문에 Spark NLP를 활용하여 따로 형태소 분석을 진행해야 할 것으로 보입니다.

