# Mongodb Bulk operation

<https://docs.mongodb.com/v2.6/core/bulk-write-operations>

많은 양의 엑셀 데이터를 Streaming으로 읽어왔더니만 저장할때가 문제였다. 이 2가지만 해결하면, 메모리, 성능 등의 이슈가 없을 것이기에 안심하고 배포할 수 있을 것이라 생각했다. 

처음에는 그냥 1만개의 데이터를 한번에 넣어보니 안되더라... 들어가긴 하더라도 너무 느리거나 이상하다 싶으면 안되는거다. 혹시 뭔가 있지 않을까 하고 찾아보니 역시 있더라...

## What

Mongodb는 다량의 데이터를 쓰기 위한 연산을 제공하는데, 이것이 bulk operation이다. 이는 단일 collection에서만 영향을 미친다. 

## Why

왜 이런게 필요할까?
이걸 쓰면 기똥찰 것 같은 것

- 다량의 로그데이터를 실시간으로 데이터베이스에 적재하는 것이 아니라, 일정 단위로 모아서 한번에 밀어 넣을 때
- mongodb는 relation이 없다. 따라서 데이터 모델링을 할 때 반정규화를 하는데, 반정규화 시에는 잘 변하지 않는 데이터라면 그 자체를 document에 포함시켜버린다. 만일 그 잘 변하지 않는 데이터가 변해야 한다면?? 수천개의 document를 업데이트 해야 한다면? 
- 어떤 서비스에 사용자 그룹이 있는데, 그 사용자 그룹의 일부 인원들을 한번에 상위 권한으로 조정한다면? loop로 여러번 update를 실행하는 것이 아니라 bulk operation으로 한번에 업데이트 하는 거다. 

## How

무엇인지, 왜 쓰는지 알고나면 나머지는 다 '어떻게'이다. 

**Ordered vs Unordered Operations**

Bulk operation은 ordered 또는 unordered로 연산을 수행할 수 있는데, ordered로 연산을 수행하면, 단어가 주는 뉘앙스처럼 순차적으로 데이터를 저장한다. unordered라면 그냥 막 던지나 보다.

ordered가 마냥 좋은 것은 아닌 것이, ordered로 bulk operation 쓰기 연산을 수행하다가 오류가 발생하면 그 이후의 데이터에 대해서는 쓰기 연산 처리를 하지 않고 그냥 리턴한다고 한다. 이건 좀 애매해서 해봐야 할 것 같다. exception으로 잡아서 계속 쓰기 시키면 되지 않을까?;;

하여간, 프레임에 따라 ordered, unordered 잘 가려서 써야 한다는 것인데, 아마 대부분의 경우 unordered로 처리할 것 같다. ordered로 처리하면 exception 처리를 잘해줘야 할 듯하다.

**Bulk method**

step
1. bulk operation을 생성한다.
2. 지원되는 bulk 메소드를 상황에 맞게 조합해서 쓴다.
   * insert(), find, find.upsert, update, updateOne
   * replaceOne, remove, removeOne 
3. execute 메소드로 앞선 연산들을 실행한다. 
> 주의사항: 한번 execute를 실행하면, 재 사용할 수 없고, 반드시 다시 스텝을 실행해야 한다.

읽어보길 잘했다. 아마 난 bulk operation을 생성하고, 필요에 따라 연산 조합을 실행한 뒤, execute를 재사용 했을 것이다.


**Bulk Execution Mechanics**

여기서 재미있는게 나오는데, 성능으로 치자면 unordered가 더 좋을 것 같은 힌트가 나온다.

1. 연산이 ordered로 실행될 경우, mongodb는 operation type을 기반으로 인접한 operation들을 묶는다.
2. 반면 unordere로 실행할 경우, mongodb는 성능을 향상시킬 목적으로 연산들의 순서를 재조정해서 묶을 수도 있다.

뭔가 우선순위 같은 룰이 있거나, 내부적으로 같이 처리되면 성능이 잘나오는 것들이 있나보다.

**limit가 있을까?**

대량이라 한들, 어쨋든 대부분 물리적 제약을 가지고 있다. 이론상 무제한이더라도 뭔가 걸리는 제약이 있겠지 했는데 있더라.
bulk 실행시 연산을 묶어서 처리하는데, 이때 하나의 연산 그룹 당 최대 가질 수 있는 연산 수는 `1000개`이다.

만일 2만개의 연산을 bulk로 실행하면, 실행한 client 입장에서 보이는 건 한번에 2만개가 날라간 것 같지만, 실제로는 내부 로직에서 1000개씩 끊어서 보내는 것

## Practice

Spring-data-mongodb를 쓸 경우, Bulk operation은 1.9.0.REALEASE부터 사용할 수 있다.

```java
@Override 
public void insertBulk( List<ExamSummary> examSummaries )
{
	BulkOperations bulkOperations = 
			 mongoTemplate.bulkOps( BulkOperations.BulkMode.UNORDERED, entityClass );
	bulkOperations.insert( examSummaries );
	bulkOperations.execute();
}
```

일단 이렇게 써보고 나서 감동했다. 이전에 500개 가량의 테스트 데이터를 단순 insert 메소드로 저장했을 때는 데이터베이스에 쿼리를 계속 날려보아야 했다. 순차적으로 증가하고 있는게 보여서;;;;

120, 164, 230, 384... 500 이렇게 증가하던데, bulk operation으로 저장시에는 0.04s만에 저장되었다.
실제 저장해야 할 데이터 셋 4만개를 bulk operation으로 insert를 해보니, 설명처럼 1000개씩 순차 증가했다.

1000, 2000, 3000 ... 4만개를 넣으려니 느리다.

이전에는 다량의 데이터 저장에 이렇게 시간이 소요될지 고민하지 않아서
`데이터 파싱 > 리스트에 저장 > 데이터베이스에 저장` 순으로 순차 실행 로직을 구성했는데, 이렇게 1000개씩 그룹화하여 처리되는 거라면, 처리 방식을 바꾸는게 낫겠다.