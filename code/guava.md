# Guava 

아직 다 모르는데, 두번 찾아볼때마다 정리하면 익숙해 질 듯

## Ordering

Mongodb에서 리턴한 결과를 정렬할 필요가 있어서, Comparable을 상속하고 Collection sort를 했더니 안된다. 결과 값 리스트가 UnmodifiableList라서 안된다. 확인해보니, MongoDB-java-driver에서 return 할 때 `Collections.unmodifiableList(values)` 이렇게 멀쩡한 결과를 Unmodifiable로 만든다. 이걸 풀고 Collection으로 다시 sort하도록 만드려다가 보니깐 guava에 Ordering으로 쉽게 된다.

`list = Ordering().natural().sortedCopy( list );` 

뭐든 다 만들어둔 guava는 java로 뭔가 코딩하면 계속 쓸 것 같다.