## anagram

학부과정 때는 알고리즘 문제를 종종 풀어보곤 했는데, 그 이후로는 그다지 알고리즘 문제를 풀어본 적이 없다.

석사과정 때 프로젝트를 하면서는 주로 리팩토링과 디자인 패턴에 대한 고민을 많이 했는데, 대부분의 프로그램은 순차, 선택, 반복(재귀) 안에서만 고려하면 되기 때문에 유지보수성에 중점을 더 많이 두는 것이 좋은 선택인 것 같았다.

그렇게 프로젝트를 진행하다보니, 다시 알고리즘과 자료구조가 중요하다는 것을 뒤늦게 느낀다. 그걸 많이 느낀 것이 anagram 문제이다.

## making anagram

<https://www.hackerrank.com/challenges/ctci-making-anagrams>

일단 아나그램이란 단어의 철자를 분해해 다른 단어, 혹은 다른 문장으로 바꾸는 놀이이다.

예를 들어, tom marvold riddle (톰 마보로 리들)을 고치면 i am load voldemort (나는 볼드모트 경이다.)라는 것 이 아나그램이다.

보통 2개의 input이 아나그램인가?를 맞추는 알고리즘 문제이고 2개의 input을 정렬하여 일치하는지 여부만으로 문제를 해결할 수 있다.

* input 1: `tom marvold riddle `
  * input 1 sort: `adddeillmmoorrtv`	
* input 2: `i am load voldemort`
  * input 2 sort: `aaddeillmmooortv`

대충 아래처럼 코딩할 수 있다.

```
boolean checkAnagram( String a, String b ){
    a, b trim()
    if a.length != b.length 
    	then return false;
    
    sort a, sort b
    
    compare a and b 
    if a and b equal 
       then return true, 
       else false;
}
```

## 그런데 문제에서 output 조건이 특이하다.
>Print a single integer denoting the number of characters you must delete to make the two strings anagrams of each other.

만일 input 1 이 `abc` 이고, input 2 가 `cde`라면 output은 4가 되어야 한다.

나는 이걸 보고 KMP 알고리즘을 적용해서 해결할 생각을 했다. 혹은 2중 loop를 돌려서 찾아내는 방식을 생각했다. `순차, 선택, 반복` 이거면 다 된다는 생각이 문제인데, 자료구조를 잘 활용할 생각을 못했다.

## 그래서 해결

문제를 해결하려면, input1과 2의 글자수 빈도를 체크를 하면 된다.

코드로 나타내면 아래와 같다. 

```java
public static int checkAnagram(String first, String second) {
    int[] letterFrequency = new int[26];
	
	for (char c : first.toCharArray()) {
		letterFrequency[c - 'a']++;
	}
	for (char c : second.toCharArray()) {
		letterFrequency[c - 'a']--;
	}

	int result = 0;
	for (int i : letterFrequency) {
		result += Math.abs(i);
	}
	return result;
}
```

처음에 나는 letterFrequency를 input1, input2에 대해 2개를 만들고, 마지막에 두 array의 차이를 구했는데... 더 생각하면 더 좋은 방법이 있다.


