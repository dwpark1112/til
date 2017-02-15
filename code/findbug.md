# FindBug

findbug가 써보니깐 좋은 것 같다. 코드 코칭해주는 것 같아서 유익

## 비효율적인 Number 생성자를 호출하는 메소드 (DM_NUMBER_CTOR)

`new Integer(int)`는 항상 새로운 개체가 생성되니깐, 가상머신에 값이 캐시되는 방식으로 `Integer.valueOf(int)` 값을 생성하는 것이다.

valufOf 메소드를 사용하는 것은 생성자를 사용하는 것보다 3.5배 정도 빠르다고 한다.

## Primitive value is boxed then unboxed to perform primitive coercion

`new Double(d).intValue()` 이렇게 쓰게 되면, 생성하자마자 primitive type으로 형변환이 일어나니깐 그냥 direct로 primitive type으로 형변환을 하라는 것 `(int) d`


## Boxing/unboxing to parse a primitive

`Integer.valueOf(String s)` 이렇게 사용하면 객체를 만드는 거니깐, 객체를 만든 로직에서 단순하게 원하는게 primitive type이라면 `Integer.valueOf(String s)` 이걸 쓰라는 것, 대충 IDE로 개발할 때는 그냥 넘어갔는데, 자세히 보면 return 타입이 다르다. Integer.valueOf의 경우 primitive type인 int를 반환한다.
