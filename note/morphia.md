# Morphia

<http://mongodb.github.io/morphia/>

모피아는 자바 객체와 Mongodb를 연결해주는 lightweight type-safe library이다.
ORM처럼 mongodb의 document(json)을 java object에 매핑시켜주는 라이브러리로서, 이걸 그대로 쓰는 일은 이제 없을 것 같다. mongodb-java-driver에 포함되어 있다.

spring-data-mongodb와 querydsl-mongodb 둘다 mongodb-java-driver를 포함하고 있기 때문에 java에서 어떤 라이브러리를 쓰던 morphia라이브러리가 포함되어 있다. 덕분에 쉽게 코딩이 가능하다.

mysql+mybatis 조합도 꽤 편한데, mongodb+querydsl은 편한 정도가 아니라 생산성이 몇 배 이상 높다. -_-
프로토타입으로 프로젝트 개발할 때는 mongodb+querydsl이 훨씬 유리하고, 이렇게 만든 것을 필요할 경우 mysql + mongodb 하이브리드로 개발하는 것이 좋을 것 같다.