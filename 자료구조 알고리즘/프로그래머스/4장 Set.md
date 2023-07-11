# 🖥️ Set

- 집합 자료 구조
- 내부는 hash table 사용하고 있다.



## 규칙

- 중복된 값을 허용하지 않는다
  - 추가 시도 → 이미 존재하는 데이터인가? →  X : 추가 / O : 버림
  - 이미 존재하는 지 어떻게 아는가? 
  - **선형 데이터 구조 + 탐색 알고리즘**



## 예제

``` java
package SetExam;

import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Objects;
import java.util.Set;

class MyData {
	int v;
	
	public MyData(int v) {
		this.v = v;
	}
	
	@Override
	public String toString() {
		return "" + v;
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
	
}
public class Main {
	 public static void main(String[] args) {
		 
//		 List<Integer> list = new LinkedList<>();		 
//		 list.add(1);
//		 list.add(2);
//		 list.add(3);	 
//		 if(!list.contains(1)) list.add(1);
//		 if(!list.contains(2)) list.add(2);
//		 if(!list.contains(3)) list.add(3);

		 
		 // set은 중복 제거를 목표로 한다.
		 HashSet<Integer> list = new HashSet<>();
		 list.add(1);
		 list.add(2);
		 list.add(3);		 
		 System.out.println(list);
		 
		 // set 은 순서를 보장하지 않는다.
		 Set<MyData> setA = new LinkedHashSet<>();
		 Set<MyData> setB = new LinkedHashSet<>();
		 // A
		 setA.add(new MyData(1));
		 setA.add(new MyData(2)); 
		 setA.add(new MyData(3));
		 // B
		 setB.add(new MyData(2));
		 setB.add(new MyData(3)); 
		 setB.add(new MyData(4));
		
		 // A+B
		 setA.addAll(setB);
		 System.out.println(setA);
		 
		 // A-B
		 setA.removeAll(setB);
		 System.out.println(setA);
		 
		 // A*B(교집합)
		 setA.retainAll(setB);
		 System.out.println(setA);
	 }
}

```



# ✏️ 문제 풀기

## 1. 로또 번호 검출기

###### 문제 설명

로또복권의 번호는 1에서 45 사이의 값을 가진 6개의 숫자로 구성됩니다.
로또복권을 신청하는 사용자들은 OMR카드에 숫자를 마킹하여 신청을 하는데, 가끔 잘못 표시하여 신청하는 사용자들이 있습니다.
로또복권에 등록 가능한 숫자조합인지 확인하는 기능을 작성해 주세요

입력1: [4, 7, 32, 43, 22, 19]
출력1: true

6개의 숫자가 중복없이 1~45사이의 값을 가지고 로또복권 등록이 가능합니다.

입력2: [3, 19, 34, 39, 39, 20]
출력2: false

6개의 숫자 중 39가 중복되어 로또복권 등록이 불가능합니다.

------

입출력 예

입력1: [4, 7, 32, 43, 22, 19]
출력1: true

입력2: [3, 19, 34, 39, 39, 20]
출력2: false



### 내 풀이

``` java
import java.util.HashSet;

class Solution {
    public boolean solution(int[] lotto) {
        HashSet<Integer> set = new HashSet<>();
        for(int num : lotto) {
            set.add(num);
        }
        return set.size() == 6 ? true : false;
    }
}
```

### 다른 풀이

```java
class Solution {
    public boolean solution(int[] lotto) {
        boolean [] checker = new boolean[45+1];
        for(int l : lotto) {
          	if(l < 1 || l > 45) return false;
            if(checker[l] == true) return false;
            checker[l] = true;
        }
        return true;
    }
}
```

```java
import java.util.HashSet;

class Solution {
    public boolean solution(int[] lotto) {
      	Set<Integer> set = new HashSet<>();
        for(int l : lotto) {
          	if(l < 1 || l > 45) return false;
             set.add(l);
        }
      	return set.size() == lotto.length;
    }
}
```



## 2. 끝말 잇기

###### 문제 설명

입력되는 단어가 순서대로 배치될 때 끝말잇기로 끝까지 이어지는지 확인하세요.
끝말잇기는 사용했던 단어가 다시 사용되면 안됩니다.
단어의 첫 글자는 앞 단어의 마지막 글자로 시작되어야 합니다.
(단, 첫 단어의 첫 글자는 확인하지 않습니다.)

입력1: ["tank", "kick", "know", "wheel", "land", "dream"]
출력1: true

단어의 연결이 모두 이어지고, 중복되는 단어가 없었습니다.

입력2: ["tank", "kick", "know", "wheel", "land", "dream", "mother", "robot", "tank"]
출력2: false

사용되었던 tank 단어가 다시 사용되었습니다.



### 내 풀이

``` java
import java.util.HashSet;

class Solution {
    public boolean solution(String[] words) {
        HashSet<String> set = new HashSet<>();
        int len = words.length;
        
        for (int i = 0; i < len; i++) {
            set.add(words[i]);
            if(i != len-1) {
                String str1 = words[i];
                String str2 = words[i + 1];
                if (str1.charAt(str1.length() - 1) != str2.charAt(0)) return false;
            }
        }
        
        if (set.size() != len) return false;        
        return true;
    }
}
```

## 다른 풀이

```java
import java.util.*;

class Solution {
    public boolean solution(String[] words) {
        Set<String> set = new HashSet<>();
        
        set.add(words[0]);
        char last = words[0].charAt(words[0].length()-1);
        
        for(int i = 1; i < words.length; i++) {
            String w = words[i];
            char first = w.charAt(0);
            
            if(last != first) return false;
            if(set.add(words[i])) return false;
           
            last = w.charAt(w.length() - 1);
        }
        return true;
    }
}
```





## 같은 숫자는 싫어

###### 문제 설명

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

- arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
- arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

##### 제한사항

- 배열 arr의 크기 : 1,000,000 이하의 자연수
- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

------

##### 입출력 예

| arr             | answer    |
| --------------- | --------- |
| [1,1,3,3,0,1,1] | [1,3,0,1] |
| [4,4,4,3,3]     | [4,3]     |

##### 입출력 예 설명

입출력 예 #1,2
문제의 예시와 같습니다.



### 내 풀이

``` java
import java.util.*;

class Solution {
    public List<Integer> solution(int[] arr) {
        List<Integer> list = new LinkedList<>();
        
        for (int i = 0; i < arr.length - 1; i++) {
            if (arr[i] != arr[i + 1]) list.add(arr[i]);
        }
        list.add(arr[arr.length - 1]);
       
        return list;
    }
}
```

### 다른 풀이

``` java
import java.util.*;

class Solution {
    public int[] solution(int[] arr) {
        List<Integer> list = new LinkedList<>();
        
        int last = -1;
        for(int a : arr) {
            if(last == a) continue;
            last = a
            list.add(a);
        }
        return list.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

