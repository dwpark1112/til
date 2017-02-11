# Left rotation

이건 좀 단순한 문제인데, success rate가 낮길래 그냥 풀어보았다.

### 문제

<https://www.hackerrank.com/challenges/ctci-array-left-rotation>


단순하게 swap 하면 된다고 생각했다. rotation이니깐 array의 첫 번째 값을 저장해두고, array는 모두 한칸씩 앞으로 당겨둔다. 마지막에는 첫 번째 저장된 값을 마지막에 밀어 넣는 것이고...


```java
// if numbers [1, 2, 3, 4, 5], sizeOfNumbers 5, lotateCnt 4
// result is [5, 1, 2, 3, 4]
int[] doLeftRotation(int[] numbers, int sizeOfnumbers, int lotateCnt) {

	for (int i = 0; i < lotateCnt; i++) {
		int target = numbers[0];

		for (int j = 1; j < sizeOfnumbers; j++) {
			numbers[j - 1] = numbers[j];
		}

		numbers[sizeOfnumbers - 1] = target;
	}

	return numbers;
}

```

생각해보니, 실무 개발에서 array 써본 적이 거의 없는 것 같다. 대부분 list를 사용했네;;