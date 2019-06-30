# 의료정보제공 서비스에서의 검색어 분석 처리기 개발

## 기획의도

- 기업과 연계된 간단한 프로젝트였음. 기업은 환자들이 검색할 수 있는 자연스러운 단어들 (ex) 손발저림, 손발이 저리다 등 )에 대해서 꽤 훌륭한 검색결과를 얻고 싶어했음.

## 영상
[![Video Label](https://img.youtube.com/vi/VvlSdg_KoP8/0.jpg)](https://www.youtube.com/watch?v=VvlSdg_KoP8)
`이미지 클릭시 영상을 볼 수 있음.`

## 사용기술
`Django, ELK, html/css/javascript, pandas, selenium`

## 나의 역할
`pandas 를 이용한 데이터 전처리, 프론트, ELK를 이용한 형태소분석, 글씨 highlighting 등`

## ELK

### elasticsearch에 logstash를 통해 json 파일 업로드 시키는 방법

`elasticsearch에서 제공하는 nori_tokenizer를 이용하기 위해 index의 settings을 먼저 설정해준다`

<pre><code>
PUT index명

{
  "settings": {
    "index": {
      "analysis": {
        "tokenizer": {
          "nori_user_dict": {
            "type": "nori_tokenizer",
            "decompound_mode": "mixed",
            "tokenizer" : "nori_tokenizer"
          }
        },
        "analyzer": {
          "my_analyzer": {
            "type": "custom",
            "tokenizer": "nori_user_dict"
          }
        }
      }
    }
  }
}
</code></pre>

### Query

<pre><code>
input {
	file {
    	path => "C:/Users/kyujo/disease_final.json"
    	start_position => "beginning"
    	sincedb_path => "nul"
    	codec => json {
          charset => "UTF-8"
        }
	}
}

filter{
   mutate{
       remove_field => [ "_id" ]
   }
}
output {
	elasticsearch {
        hosts => ["http://127.0.0.1:9200"]
        index => "nnn" 
        document_type => "disease"
        document_id => "%{id}"
	}
	stdout {
	}
}
</code></pre>

## 소감
```
기업에서 검색이 잘 될 수 있도록 요구하여서 ELK을 이용하기로 하였습니다. 
엘라스틱 서치 책들을 사서 혼자서 공부하였습니다. 웹에 적용시켜보면서 어려움을 겪었지만 docs를 보면서 해결하였습니다.

형태소 분석을 하여서 검색결과를 보여주기 때문에 더 많고 원하는 결과를 보여줄 수 있었습니다. 추가적으로 하이라이팅 기능 등을 사용하였습니다. 그리고 동일한 단어에 대해서도 비슷한 결과를 보여주기 위해서 데이터베이스에 있는 데이터를 형태소 분석을 하여 동사, 형용사, 명사를 뽑아내었습니다. 그러고 나서 셀레니움, beautifulsoup4을 이용한 크롤링을 통해서 동의어, 유의어 등을 수집하였습니다.

엘라스틱 서치에서 쿼리를 날릴 때 동의어 ,유의어에 대해서도 일정 점수를 부여하여, 검색결과를 더 확장하였습니다. 

엘라스틱 서치를 이용하는 부분도 재밌었지만 디자인된 화면을 구현하는 부분도 재밌었습니다. 이 프로젝트 이후로 프론트엔드에 관심을 가지게 되었습니다. 
```
