# 이제 할껀데, 궁금증부터 해결

## 원하는거 다 지원되나?

- Spring-data-elasticsearch 
- RESTful API

## ElasticSearch에서 MySQL 데이터 불러오기 <http://geniedev.tistory.com/6>

![](http://cfile7.uf.tistory.com/image/2334294E5733E68D34A1FE)

1.9버전까지는 river라는 도구가 있었는데, 2.0부터 공식적인 지원 중단함. 이후 logstash를 이용하여 MySQL 데이터를 불러오기를 권장함

Elasticsearch에 적재되는 과정

```
logstash 실행 -> logstash jdbc 연결
쿼리실행 -> 로그 발생 -> 엘라스틱서치 적재
```

<https://qbox.io/blog/migrating-mysql-data-into-elasticsearch-using-logstash>


