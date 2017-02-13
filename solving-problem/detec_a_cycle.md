# Linked Lists: Detect a Cycle

<https://www.hackerrank.com/challenges/ctci-linked-list-cycle>

링크드 리스트에서 사이클 여부를 확인하는 문제는 항상 단골문제였던 것 같다.  
연구실에서 공부할 때, 학부 연구원이 MS인턴 지원하면서 전화인터뷰로 같은 문제를 풀었던 기억이 난다.  
당시에 질문으로 재미있던 것은 리스트에 끝이 없다면 어떻게 되는가? 라는 질문이었는데,
그 친구가 리스트에 끝이 없다면 사이클이 없다는 답을 했던 것이 기억이 난다.

이 문제의 답은 여러가지를 제시할 수 있는데, 단순하게 자료구조를 하나 더 사용해서 풀 수 있고 아니면 포인트 2개를 사용해서 풀 수 있다. 최적의 솔루션으로 알려진 것은 포인트 2개를 쓰는 것이다.

```java
	boolean hasCycle(Node head) {
		if (head == null || head.next == null) {
			return false;
		}

		Node slowNode = head;
		Node fastNode = head.next;
		while (slowNode != fastNode) {
			if (fastNode == null || fastNode.next == null) {
				return false;
			}

			slowNode = slowNode.next;
			fastNode = fastNode.next.next;
		}

		return true;
	}
```

slowNode는 항상 다음 Node만 방문하고, faseNode는 항상 2단계 이후의 Node만을 방문한다. 
이렇게 사이클 찾기를 시도하면, 최대 n(리스트 사이즈)번 내에 사이클이 있는지 여부를 찾을 수 있게 된다.
