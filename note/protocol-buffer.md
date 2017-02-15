# Protocol buffers, google

아직 해보진 않았고, 해볼 일이 있을까 싶은 protocol buffers
간단히 자료를 좀 읽어보기만 했다. 해볼 일이 없으면 그냥 간단하게 토이 프로젝트 정도만 만들어봐야겠다.

ref <https://developers.google.com/protocol-buffers/>

원래 Apache Thrift를 보려다가, 자료 정리가 protocol buffers가 잘되어 있어서 protocol buffers를 알아보기로 방향을 돌렸다. 오픈소스는 아무리 기똥차더라도 문서화가 부실하면 삽질만 늘고, 재미도 떨어진다.

## What are protocol buffers?

> Protocol buffers are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data – think XML, but smaller, faster, and simpler.

```
message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;
}
```

Protocol buffers는 구글의 language 중립적, 플랫폼 중립적, 구조화된 데이터의 직렬화를 위한 확장성 있는 메카니즘이다. 구조화된 데이터는 XML 같은 것을 고려하면 되는데, 이보다 작고, 빠르며 간단하다. 

음, XML의 특징 중 하나는 Human readable도 있다. Protocol buffers의 structured data도 그러한가? 그렇진 않을 것 같다. 'XML은 스키마가 있어서 유효성 검증이 쉬운 것도 있다.'라고 생각이 났지만, 아래 글을 읽어보니 또 그건 아니다. client side stub을 만들어주면 유효성 검증 같은건 없어도 된다.

## Pick your favourite language

> Protocol buffers currently supports generated code in Java, Python, and C++. With our new proto3 language version, you can also work with Go, JavaNano, Ruby, and C#, with more languages to come.

```java
Person john = Person.newBuilder()
    .setId(1234)
    .setName("John Doe")
    .setEmail("jdoe@example.com")
    .build();
output = new FileOutputStream(args[0]);
john.writeTo(output);
```

여러가지 언어를 지원한다는데, 살짝 살펴보니깐 Protocol buffers 문법으로 작성해둔 것을 컴파일하면 원하는 언어로 쓸 수 있는 소스코드를 내어주는 것 같다. 마치 SOAP Webservice에서 WSDL을 만들고, 이걸 wsimport로 코드를 생성하는 것과 비슷하다. skeleton과 stub 같은?

예제 코드가 참 마음에 든다. 빌더패턴으로 객체를 생성시키고 setter method들도 쓰기 참 재미있게 만든다. 구글 코드들에 그런게 많은 것 같다. 예전에 안드로이드로 AlertDialog 만들때도 저런식으로 빌딩하곤 했던 추억이 떠오른다.

아무튼 Thrift가 더 많은 언어를 지원한다고 하는데, 난 어차피 다 몰라서... ㅠㅠ

## Developer Guide 
<https://developers.google.com/protocol-buffers/docs/overview>

문서 참 읽기 좋게, 잘 만들어놨다. 스타일, 인코딩, add-on 뿐만 아니라 지원 언어별 튜토리얼도 있어서 시작하기 좋다. 아무래도 이번 주말에 간단하게 만들어봐야겠다.


## 훓어보고 난 뒤 고찰

XML, JSON, Protocol buffers 등등 다들 적절한 사용 목적이 있을 것이다. 

단적인 예로 ISO/HL7 27932 HL7 Clinical Document Architecture R2는 전자진료기록 표현을 위한 구조인데, XML을 사용하고 있다. 이 표준이 XML을 사용한 것은 방대한 전자진료기록을 자유롭게 표현할 수 있고 그것을 쉽게 검증할 수 있고, 사람도 읽을 수 있기 때문이다. 데이터 스키마가 같으니깐 전자진료기록 교류가 가능하고, 서로 다른 벤더가 만든 병원정보시스템 간에 정보 교류가 가능하다. 

반면 간단한 데이터 교류에서는 XML보다 JSON이 훨씬 쉽고, 간단하다. 적절한 곳에 쓰면 다 장점이 있는데... Protocol buffers는 왜 써야하는지, 어디에 주로 쓰면 좋을지 등이 안나와있어서 아쉽다. What, Why가 중요하고, How는 그다지 안중요한데!!!

Why는 하면서 찾아보거나, 하기전에 찾아봐야겠다. 빌더로 코드 만든것 보니깐 작은 데이터 페이로드에서 쓰기 위한 것 같은 느낌이 강하게 든다. 작은 데이터를 자주 빠르게 주고 받아야 하는 게임 같은 분야에서 좋을 것 같은데, 바인딩하기 위한 라이브러리나 방법이 없다면 전자진료기록처럼 방대한 내용을 관리하는 도메인에서는 그다지 의미가 크진 않을 것 같은 생각도 든다.