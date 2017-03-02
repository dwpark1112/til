# Balanced Brackets

<https://www.hackerrank.com/challenges/ctci-balanced-brackets>

문제 자체에 stack이라고 써 있어서 별 고민없이 stack을 사용해서 풀긴 했는데, 다른 사람들 풀어본 것 보니 좀더 고민했어야 하는데, 잘 푼다.;;

FM으로 push하고 pop으로 지워나가면 될 것으로 생각하고, map을 써야겠구나 했다. key로 bracket을 쓰고, value로 pair를 맞추려 했는데...

```java
	public boolean isBalanced(String expression) {
		Map<Character, Integer> map = new HashMap<>();
		map.put( '{', 1 );
		map.put( '[', 2 );
		map.put( '(', 3 );
		map.put( '}', -1 );
		map.put( ']', -2 );
		map.put( ')', -3 );
		
		char[] expressionArr = expression.toCharArray();
		if ( expressionArr.length % 2 != 0 ) {
			return false;
		}

		Stack<Character> stack = new Stack<>();
		for (int i = 0; i < expressionArr.length; i++) {
			int point = map.get( expressionArr[i] );

			if (point > 0){
				stack.push( expressionArr[i] );
			}else{
				if ( stack.isEmpty() ){
					return false;
				}

				Character target = stack.pop();
				int popPoint = map.get( target );

				if (popPoint + point != 0){
					return false;
				}
			}
		}

		return stack.isEmpty();
	}
```

아래는 참 잘 구현한 어떤이...
애초에 map 자체를 쓸 필요가 없었던 것 ㅠ_ㅠ  
코드 라인 수가 줄어든 것뿐만 아니라, 가독성도 훨씬 좋다. 이해도 잘 되고;;

```java
bool is_balanced(string expression) {
  stack<char> s;
  for (char c : expression) {
    if      (c == '{') s.push('}');
    else if (c == '[') s.push(']');
    else if (c == '(') s.push(')');
    else {
      if (s.empty() || c != s.top())
        return false;
      s.pop();
    }
  }
  return s.empty();
}
```