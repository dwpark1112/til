# Amazon EMR 소개 및 구축 패턴

2017년 3월 22일 (수) 14:00~16:00  
EMR보다도 다른 기술들이 무엇인지 왜 필요한지 확인하고, EMR은 어떻게 그것들을 활용해서 사용하는것인지 확인하려고 세미나 참석

## What?

hadoop = HDFS (file system), 대용량 데이터 처리를 위한 프레임워크  

- 대용량 데이터분석
- 실시간 데이터처리

### v2.0

- map reduce (data processing)
- YARN (cluster resource management)


## Amazon EMR

- 관리형 하둡 플랫폼
- MapReduce, Spark, Presto 활용
- customize 가능 - SW, config 등
- open source 기반의 경험이 있다면 유리, 그러나 모든 에코시스템을 제공하지 않음

## EMR 아키텍처

클러스터 형태 (Master, Core, Task - nodes)

- Master (resource manager role, node as name node)
- Core (실제 HDFS를 가지며 연산 수행 가능)
- Task (HDFS를 가지고 있지 않고)

I/O 작업이 많으면 Core node를 늘리고, CPU 작업이 많으면 Task node를 늘린다. Master nodes는 반드시 1개, 나머지는 n개가 될 수 있다.

Core를 늘리거나 줄일때 replication을 재배치 해야한다. EMR에서는 확장된 Core node를 줄일수 없다. (왜 replication이 진행되는가? 분산 로스 방지) 반면, Task는 HDFS가 없기 떄문에 늘리거나 줄일 수 있다. 

Termination 되면, 데이터가 전부 날아가니깐 s3에 저장하는 EMRFS(S3)를 쓰는게 좋은 방법인다.

**다양한 인스턴스를 활용한 EMR구성**

> 인스턴스는 그룹별로 같은 것을 사용하는게 유리하다. Core/Task그룹
> 스팟인스턴스의 Task node활용을 통한 비용 효율화

## 어떻게 만드나요?

1. 원하는 SW를 선택
2. 인스턴스 선택
3. 접근 방법을 선택, 끝

소프트웨어 스택을 선택적으로 구성할 수 있다. 단순히 클릭 한번으로 가능

## 운영은 어떻게 하나요?

- 시작은 소소하게? Hadoop + hive
- 정규작업? oozie
- NoSQL? Hbase
- 스트림? spark, storm
- 보다 빠르게? implala

## Hive Metastore, DR

S3를 쓰면 다 된다는 것  
만일 압축된 포맷을 저장하거나 쓴다면 문제가 있을 수 있음  

## EMR 구축 패턴

예제는 많은데, 근본은 `hadoop + hive`에서 필요에 따라 에코시스템을 조합하는 것이다. 그리고 S3를 적극 활용하는 것과 AWS RDS들을 활용해주는 게 포인트이다. 직접 구축하려면 시간이 꽤 걸리는 인프라 작업을 쉽게 할 수 있게 잘 만들어두었다.

