# 🖥️ Sort

- 정렬 = 순서 바꾸기



## 1. Bubble sort

- 모든 경우의 수 확인하는 버블 정렬
- O(n^)

## 2.Insertion sort 

- 값을 하나씩 옮겨가는 삽입 정렬
- O(n^)

## 3. Selection sort

- 최소값을 찾아서 가장 왼쪽에 두는 선택 정렬
- O(n^)

## 4. Quick sort

- 큰 것은 큰 것끼리, 작은 것은 작은 것끼리
- O(nlogn)

## 5. Merge sort

- 일단 나눴다가 작은 것부터 합친다
- O(nlogn)



## 예제

```java
package Sort;

import java.util.Comparator;
import java.util.LinkedList;
import java.util.List;
import java.util.Random;

/*
* 왜 종류별로 알고리즘을 학습해야 하나?
*  
* 1. 다양한 알고리즘을 학습하면서 문제풀이의 접근방식을 학습할 수 있습니다.
* 2. 문제를 해결하는 알고리즘은 한가지가 아니구나! 효율성이 달라지는 구나! 절대적인 것은 없구나!
*/

public class Main {

	public static void main(String[] args) {
		List<Integer> list = new LinkedList<>();

		Random r = new Random();
		for (int i = 0; i < 20; i++) {
			list.add(r.nextInt(50));
		}

		list.sort(Comparator.naturalOrder()); // 오름차순 정렬
		list.sort(Comparator.reverseOrder()); // 내림차순 정렬

		// 오름차순 정렬
		list.sort(new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				return o1.intValue() - o2.intValue();
			}
		});

		// 내림차순 정렬
		list.sort(new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				return o2.compareTo(o1);
			}
		});
		
		System.out.println(list);

	}

}
```



#  문제 풀기

## 문자열 내 마음대로 정렬하기

###### 문제 설명

문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 ["sun", "bed", "car"]이고 n이 1이면 각 단어의 인덱스 1의 문자 "u", "e", "a"로 strings를 정렬합니다.

##### 제한 조건

- strings는 길이 1 이상, 50이하인 배열입니다.
- strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
- strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
- 모든 strings의 원소의 길이는 n보다 큽니다.
- 인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

##### 입출력 예

| strings                 | n    | return                  |
| ----------------------- | ---- | ----------------------- |
| ["sun", "bed", "car"]   | 1    | ["car", "bed", "sun"]   |
| ["abce", "abcd", "cdx"] | 2    | ["abcd", "abce", "cdx"] |

##### 입출력 예 설명

**입출력 예 1**
"sun", "bed", "car"의 1번째 인덱스 값은 각각 "u", "e", "a" 입니다. 이를 기준으로 strings를 정렬하면 ["car", "bed", "sun"] 입니다.

**입출력 예 2**
"abce"와 "abcd", "cdx"의 2번째 인덱스 값은 "c", "c", "x"입니다. 따라서 정렬 후에는 "cdx"가 가장 뒤에 위치합니다. "abce"와 "abcd"는 사전순으로 정렬하면 "abcd"가 우선하므로, 답은 ["abcd", "abce", "cdx"] 입니다.



### 내 풀이

```java
import java.util.Arrays;

class Solution {
    public String[] solution(String[] strings, int n) {
        Arrays.sort(strings, (s1, s2) -> {
            if (s1.charAt(n) == s2.charAt(n)) {
                return s1.compareTo(s2);
            } else {
                return Character.compare(s1.charAt(n), s2.charAt(n));
            }
        });
        return strings;
    }
}
```

### 다른 풀이

```java
import java.util.Arrays;

class Solution {
    public String[] solution(String[] strings, int n) {
        Arrays.sort(strings, (s1, s2) -> {
            if (s1.charAt(n) == s2.charAt(n)) {
                return s1.compareTo(s2);
            }
            return s1.charAt(n) - s2.charAt(n);
        });
        return strings;
    }
}
```



## JadenCase 문자열 만들기

###### 문제 설명

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)
문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

##### 제한 조건

- s는 길이 1 이상 200 이하인 문자열입니다.
- s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.
  - 숫자는 단어의 첫 문자로만 나옵니다.
  - 숫자로만 이루어진 단어는 없습니다.
  - 공백문자가 연속해서 나올 수 있습니다.

##### 입출력 예

| s                       |         return          |
| ----------------------- | :---------------------: |
| "3people unFollowed me" | "3people Unfollowed Me" |
| "for the last week"     |   "For The Last Week"   |

##### 입출력 예 설명

**입출력 예 1**
"sun", "bed", "car"의 1번째 인덱스 값은 각각 "u", "e", "a" 입니다. 이를 기준으로 strings를 정렬하면 ["car", "bed", "sun"] 입니다.

**입출력 예 2**
"abce"와 "abcd", "cdx"의 2번째 인덱스 값은 "c", "c", "x"입니다. 따라서 정렬 후에는 "cdx"가 가장 뒤에 위치합니다. "abce"와 "abcd"는 사전순으로 정렬하면 "abcd"가 우선하므로, 답은 ["abcd", "abce", "cdx"] 입니다.



### 내 풀이

``` java
import java.util.*;

class Solution {
    public String solution(String s) {
        String answer = "";
        String[] arr = s.split(" ");

    	for(int i=0; i<arr.length; i++) {
    		String now = arr[i];

    		if(arr[i].length() == 0) {
    			answer += " ";
    		} 
    		else {
    			answer += now.substring(0, 1).toUpperCase();
    			answer += now.substring(1, now.length()).toLowerCase();
    			answer += " ";
    		}
    	}
    
    	if(s.substring(s.length()-1, s.length()).equals(" ")){
    		return answer;
    	}
        return answer.substring(0, answer.length()-1);
    }
}
```

### 다른 풀이

```java
class Solution {
    public String solution(String s) {
        StringBuilder sb = new StringBuilder();
        
        String s2 = s.toLowerCase();
        char last = ' ';
        for(char c: s3.toCharArray()){
            if(last == ' ') c = Character.toUpperCase(c);
            sb.append(c);
            last = c;
        }
        return sb.toString();
    }
}
```



## H-Index

###### 문제 설명

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://campus.programmers.co.kr/tryouts/88751/challenges#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

##### 입출력 예

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |

##### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.



### 내 풀이

``` java
import java.util.Arrays;

class Solution {
    public int solution(int[] citations) {
        Arrays.sort(citations); 

        for(int i=0; i<citations.length; i++){
            int h = citations.length - i;
            if(h <= citations[i]) {
                return h;
            }
        }
        return 0;
    }
}
```

