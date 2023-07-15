# 🖥️ Linear Search

- 선형 자료구조는 한 줄 세우기!



## 예제

```java
package SearchExam;

import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.Objects;

/* 
* search 는 indexOf, contains, remove 같은 곳에서 이미 구현되어 있습니다. : 완전 탐색
* equals 가 제공될 필요가 있다.
* 
* 이진 탐색: Collections.binarySearch
* Comparable 가 구현되어 있어야 한다.
* 순서대로 정렬되어 있어야 한다.
*/


class MyData implements Comparable<MyData> {
	int v;
	
	public MyData(int v) {
		this.v = v;
	}

	static MyData create(int v) {
		return new MyData(v);
	}
	
	@Override
	public String toString() {
		return ""+v;
	}
	
	@Override
	public boolean equals(Object o) {
		if(this == o) return true;
		if(o == null || getClass() != o.getClass()) return false;
		MyData myData = (MyData) o;
		return v == myData.v;
	}
	
	@Override
	public int hashCode() {
		return Objects.hash(v);
	}

	@Override
	public int compareTo(MyData o) {
		// 1 == 1 : 1 - 1 = 0 이면 같다
		// 1 ? 2 : 1 - 2 == 0 : 같다
		//				  < 0 : 오른쪽것이 크다
		//				  > 0 : 왼쪽것이 크다
		
		return v - o.v;
	}
}

public class Main {

	public static void main(String[] args) {
		List<Integer> list2 = new LinkedList<>();
		
		for(int i=1; i<=100; i++) list2.add(i);
		System.out.println(list2);
		
		int index = list2.indexOf(63); // indexOf에 선형탐색으로 정의되어있음
		System.out.println(index);
		
		// MyData
		List<MyData> list = new LinkedList<>();
		
		for(int i=1; i<=100; i++) list.add(new MyData(i));
		System.out.println(list);
		
		index = list.indexOf(new MyData(63));
		System.out.println(index);
		
		
		index = Collections.binarySearch(list, new MyData(63));
		System.out.println(index);
	}

}

```



# 문제 풀기

## 전화번호 목록

###### 문제 설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
  - 각 전화번호의 길이는 1 이상 20 이하입니다.
  - 같은 전화번호가 중복해서 들어있지 않습니다.

##### 입출력 예제

| phone_book                        | return |
| --------------------------------- | ------ |
| ["119", "97674223", "1195524421"] | false  |
| ["123","456","789"]               | true   |
| ["12","123","1235","567","88"]    | false  |

##### 입출력 예 설명

입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.



### 내 풀이

```java
import java.util.Arrays;

class Solution {
    public boolean solution(String[] phone_book) {
        Arrays.sort(phone_book);

        for (int i = 1; i < phone_book.length; i++) {
            if (phone_book[i].startsWith(phone_book[i-1])) {
                return false;
            }
        }
        return true;
    }
}
```



## 문자열 내 p와 y의 개수

###### 문제 설명

대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요. 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.

##### 제한사항

- 문자열 s의 길이 : 50 이하의 자연수
- 문자열 s는 알파벳으로만 이루어져 있습니다.

------

##### 입출력 예

| s         | answer |
| --------- | ------ |
| "pPoooyY" | true   |
| "Pyy"     | false  |

##### 입출력 예 설명

입출력 예 #1
'p'의 개수 2개, 'y'의 개수 2개로 같으므로 true를 return 합니다.

입출력 예 #2
'p'의 개수 1개, 'y'의 개수 2개로 다르므로 false를 return 합니다.



### 내 풀이

```java
class Solution {
    boolean solution(String s) {
        char[] arr = s.toLowerCase().toCharArray();
		
        int p = 0, y = 0;
		for(int i=0; i<arr.length; i++) {
			if(arr[i] == 'p') p++;
			else if(arr[i] == 'y') y++;
		}
        return p == y;
    }
}
```

### 다른 풀이

```java
class Solution {
    boolean solution(String s) {
      	int p = s.replaceAll("[^pP]", "").length();
        int y = s.replaceAll("[^yY]", "").length();
        return p == y;
    }
}
```





## 스킬트리

###### 문제 설명

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더`일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 `스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크`나 `라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리[1](https://campus.programmers.co.kr/tryouts/88747/challenges?language=java#fn1)를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

##### 제한 조건

- 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 스킬 순서와 스킬트리는 문자열로 표기합니다.
  - 예를 들어, `C → B → D` 라면 "CBD"로 표기합니다
- 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
- skill_trees는 길이 1 이상 20 이하인 배열입니다.
- skill_trees의 원소는 스킬을 나타내는 문자열입니다.
  - skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

##### 입출력 예

| skill   | skill_trees                         | return |
| ------- | ----------------------------------- | ------ |
| `"CBD"` | `["BACDE", "CBADF", "AECB", "BDA"]` | 2      |

##### 입출력 예 설명

- "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
- "CBADF": 가능한 스킬트리입니다.
- "AECB": 가능한 스킬트리입니다.
- "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.



### 내 풀이

```java
```

### 다른 풀이

```java
import java.util.*;

class Solution {
    public int solution(String skill, String[] skill_trees) {
        int answer = 0;
        
        for(String s : skill_tress) {
            
            String s2 = s.replaceAll("[^"+skill+"]", "");
            if(skill.startsWith(s2)) answer++;
        }
        return answer;
    }
}
```

```java
import java.util.*;

class Solution {
    public int solution(String skill, String[] skill_trees) {
        return (int) Arrays.stream(skill_trees)
            .map(s ->  s.replaceAll("[^"+skill+"]", ""))
            .filter(s -> skill.startsWith(s))
            .count();
    }
}
```

