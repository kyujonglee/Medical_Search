# 의료정보제공 서비스에서의 검색어 분석 처리기 개발


## ELK

### elasticsearch에 logstash를 통해 json 파일 업로드 시키는 방법

elasticsearch에서 제공하는 nori_tokenizer를 이용하기 위해 index의 settings을 먼저 설정해준다

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