# 시작하세요! 엘라스틱 서치?

아파치 루신 기반으로 만들어져서 루씬의 기능을 대부분 지원한다고 하는데, 루씬을 모름

- 사용자 위치정보 이용, 다국어 검색 지원, 철자 수정 기능 지원 및 기타 등등이 있다.
- 여러개의 `노드`로 구성되는 분산시스템
- `노드`는 데이터를 색인하고 검색을 수행하는 단위 프로세스
- 노드를 연결하는 것만으로도 확장 가능, 각 노드에 분산 저장되는 데이터, 복사본 유지로 충돌로부터 데이터 보호

## MongoDB와 ElasticSearch 비교하자면

엘라스틱서치는 검색엔진 어플리케이션이지 데이터 저장소가 아니다. 따라서 저장이 중요한 시스템에는 MongoDB를 사용하고 검색이 중요한 시스템에는 엘라스틱 서치를 사용하도록 권장한다.

접속방식에서도 차이가 나는데, MongoDB는 자체 프로토콜을 사용하고 ElasticSearch는 RESTful API를 사용한다.

## 용어 설명한다.

### Cluster

- 엘라스틱서치의 가장 큰 시스템 단위로서 하나의 클라스터는 여러개의 노드로 구성됨
- 같은 클러스터의 이름으로 노드를 실행하는 것만으로 자동확장

### Node (Master, Data)

- Master 노드는 전체 클러스터 상태의 메타데이터를 관리
- 마스터가 종료되면 새로운 마스터가 선출됨

#### Node Binding

- 9200번대 포트는 Client 접속을 위해 바인딩, 9300번대는 Node간 바인딩인데 그럼 최대 100개만?

#### Network Binding

- 네트워크 바인딩을 위해 Zen discovery 내장,  멀티캐스트와 유니캐스트 모두 지원 (공식 그룹에서는 유니캐스트의 사용 권장)
  - 유니캐스트? 1:1 통신 방법으로 프레임에 데이터 수신자의 MacAddr를 포함시켜서 보내는 방식이다. LAN카드에서 프레임 수신 후 MacAddr가 다르면 CPU까지 처리가 올라가지 않고 버려진다. 좋네 
- 반드시 엘라스틱서치의 버전을 동일해야 함

## Data Processing

- 데이터구조는 Index, Type, Document 단위로 이루어짐
- 서로 다른 인덱스 타입들은 서로 다른 매핑구조로 구성된다.

| MySQL |  ELS |
|----|----|
| Database | Index |
| Table | Type |
| Row | Document |
| Column | Field |
| Schema | Mapping |

### REST API

Usage

`curl -X{HttpMethod} http://host:port/{INDEX}/{TYPE}/{ID} -d '{DATA}'`

### Bulk API를 이용한 배치 작업 가능!

- index, create, delete, update의 4가지 동작 처리 가능
- delete를 제외한 동작은 메타데이터와 요청데이터가 한 쌍
- Document가 존재할 때 Create명령을 사용할 경우 에러 발생
- UDP 사용 가능 (설정으로)

## 검색

- query 명령어로 검색 가능, type 및 index 범위로 query 가능
- 여러개의 인덱스를 묶어서 멀티 인덱스 범위로 query (멀티테넌시)
   - 인덱스들을 쉼표로 구분하여 입력 `host:port/books,magazine/_search?q=???`
- 방식
   - URI방식 (문자열 파라미터)
     - _search API 사용, q 파라미터
   - RequestBody방식 (Http data?)
     - 일반적인 Restful 사용 방식 의미함, QueryDSL 사용

- 검색연산자
   - AND, OR
- _source: true, false로 메타데이터만 가져올 수 있음
- 출력 필드 지정 가능: _fields로 각 필드 구분자는 `,` 쉼표
- 정렬 가능: 기본적으로 점수 값을 기준으로 정렬
   - `sort={필드명}:{정렬방식}`
- 페이징(`from, size`)이 가능하니깐 적절한 값을 지정한다. `max_content_length` 설정 참고해야 할 듯
- 검색을 수행하는 방법을 지정할 수 있다. `search_type` 옵션
   - 전체 샤드의 검색이 모두 수행된 후에 결과 출력
   - 샤드별로 검색되는대로 결과 출력
   - 검색결과를 바로 보여주지 않고 저장했다가 나중에 결과를 출력 등등 많음

## QueryDSL (질의) 뭐지? 프레임워크랑 같은건가??

기본적인 형태소 분석 과정

- 대문자는 모두 소문자로 변환
- 중복단어 제거
- 저장된 토큰을 `Term`이라고 함

형태소분석시, analyzer옵션을 통해 원하는 형태소 분석 적용 가능

### Query 말고 Filter

- 스코어 계산을 하지 않으므로 쿼리에 비해 월등히 빠름
- 결과가 메모리에 캐싱됨


## Aggregation

몽고DB랑 같은가? 그런것 같은데

- Bucket aggregation
  - 조건에 매칭되는 document를 bucket이라는 저장소 단위로 구분
  - bucket별로 하위 연산을 반복해서 수행 가능
    - 레벨이 깊어질수록 메모리 등의 자원 소모가 심하니깐 주의
    - bucket 하위로 metric aggregation 사용 가능
    - filter, missing, terms, range, histogram 등이 있음
- Metric aggregation이 있음
  - min, max, sum, avg 등 

## 분석기?

- 검색어를 추출하기 위한 프로세스로 Tokenizer와 TokenFilter로 구성
- 사용자가 직접 분석기를 생성하여 적용 가능
- analyze API 제공
- 한글 분석기로 아리랑, 은전한닢 분석기 등이 있음 (오픈소스)
   - 카카오토픽에서 은전한닢 프로젝트 사용 중
