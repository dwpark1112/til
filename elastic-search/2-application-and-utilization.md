# ElasticSearch 적용 및 활용

<https://www.slideshare.net/JunyiSong1/elasticsearch-45936425>

개요

- 검색엔진 (내부 루씬), 모든 필드를 indexing하여 검색 가능
- 실시간 분석 플랫폼
  - 저장: storing > indexing > analyzing
  - 검색: searching > filtering > ordering
  - 분석: aggregation

avoiding network overhead (mget & bulk)

- 문서를 대량으로 retrieving, indexing할때 bulk 처리함으로써, NIO를 줄일 수 있다.
- bulk 요청을 처리하려면 모든 데이터를 메모리에 로드해야함

Index is immutable

- 검색엔진은 inverted index 데이터 구조를 이용해 full-text search가 가능하도록 했고, 이는 전체 document를 통해 만들어지며 수정이 불가능하다.
  - 왜 immutable인가?
  - 단일 index라서 압축을 하여 DISK IO를 줄이고, 캐싱에 필요한 RAM용량을 줄일 수 있다.
  - 새로운 document가 있고 search가 가능하도록 하려면, 전체 index를 새로 rebuild해야만 한다.

dynamically updatable indices

- index의 immutable한 장점은 그대로 유지한 채, index를 수정 가능하게 하는 것
  - 여러개의 index를 사용
    - 기존의 inveted index는 그대로 유지
    - 새로운 document 발생시 주기적으로 새로운 index를 생성
    - search 요청 발생시 여러개의 index를 차례대로 검색한 후 결과를 병합
  - 용어 정리
    - segment: 여러개의 index들 중 하나
    - commit point: 현재 segment 목록
    - index = segments + commit point

- 동작 정리
  - 새로운 document가 들어오면
    - 일단 in-memory indexing buffer에 저장된다. (search 불가능 상태)
    - buffer는 주기적으로 commit 되는데, commit 될 때 새로운 segment가 생성된 후 새로운 commit point가 disk에 기록된다. 
    - 이제부터 새로운 document가 search 가능해진다.

**그럼 document가 수정, 삭제되면 어떻게 처리되나?** index는 수정되지 않고, `.del` 파일에 삭제 및 수정된 파일을 `deleted`로 표시한다. 검색시에 매칭은 되지만 client로 반환되지는 않는다. **그럼 storage가 꽉 차게 될텐데?** segment가 merge될 때 `deleted`로 표시된 파일을 disk에서 영구적으로 삭제한다.

## near real-time search

refresh

- document를 index하면 바로 search가 되지 않고, refresh를 해야 search가 가능하다. refresh 주기를 늘리면 indexing throughput을 높일 수 있다. (refresh에 쓰이는 자원을 indexing에 사용할 수 있기 때문이다.)
- elasticsearch는 주기적으로 refresh를 실행 (default: 1s)
  - 1s마다 새로운 segment가 생성됨 (refresh_interval) 속성으로 설정 가능
  - 기본적으로 1s 이내에는 새로운 document가 search가 가능해짐
  - **그럼 segment 수가 기하급수적으로 증가할텐데?** 백그라운드에서 segment를 병합한다. indexing이 되면 refresh 프로세스에서는 새로운 segment를 열고 search가 가능하게 한다. 그 뒤 merge 프로세스에서는 비슷한 크기의 segment를 병합하여 좀 더 큰 segment를 생성한다. 이는 새로운 commit point가 기록된다.

segment merging issue

- merging이 진행 중에 search 성능이 저하될 경우, merge throttling 기능을 통해 merging을 제약한다.
- 만일 로그처럼 날짜 단위로 인덱스를 생성하는 구조라면 직접 optimize API를 사용하여 효율적으로 merging을 직접 실행할 수도 있다.


## SearchDSL

- query dsl: 매칭된 문서에 대해 관련성을 계산하여 결과 문서에 _score를 부여한다.
- filter dsl: 필터는 yes, no로 답할 수 있는 질문으로 필드가 정확한 값을 포함하는 경우에만 사용한다.

autocomplete

- 사용자가 순서대로 입력할때마다 검색을 수행하는 prefix query 는 resource 소모가 큰 쿼리이기에 부하를 많이 주게 된다.  `edge_ngram` 을 써라. partial matching을 위한 데이터를 indexing time에 준비하여 검색 성능을 향상시킴

use synonym

- 동의어 확장 및 축약 방식 등이 있음

## Discovery

엘라스틱서치는 노드를 찾아내기 위해서 discovery 모듈을 사용한다.

- ping: 노드가 다른 노드를 찾기 위한 기작

### Zen discovery

목적

- Node 바인딩 및 해제
- 마스터 Node 선출

ping 방식 <http://guruble.com/?p=87>

- multicast: 클러스터 내의 모든 노드로 멀티캐스트 전송 ![](http://guruble.com/wp-content/uploads/2014/02/multicast_stream-svg.png)
- unicase: 노드 간 discovery.zen.ping.unicast.hosts에 등록된 노드를 통해 1:1 핑 ![](http://guruble.com/wp-content/uploads/2014/02/unicast_streaming-svg.png)

### Zookeeper discovery

- zookeeper를 discovery 프로토콜로 사용하는 방법 


  