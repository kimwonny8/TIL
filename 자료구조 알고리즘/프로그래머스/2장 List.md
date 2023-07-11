# 🖥️ List

## Array와 List

### Array

- Array의 첫번째 주소가 출력된다.
- 내용 출력 : `Arrays.toString(arr)`
- 초기값 
  - 숫자: 0
  - boolean: `false`
  - Object: `null`
- 특징
  - 여러개의 데이터를 한꺼번에 다룰 수 있다.
  - 메모리상에 연달아 공간 확보 → 미리 공간을 확보해놓고 써야한다.
  - 한번 만들어진 공간은 크기가 고정된다.
  - 첫 번째 위치만 알면 index로 빠르게 찾을 수 있다.

### List

- 공간 크기를 미리 알아야하는 불편함때문에 List가 등장했다.
- 링크를 연결하는 방식 ⇒ **Linked List**
  - 데이터를 삭제할 때는 Link를 없애면 된다
  - 양방향  ⇒ **Double Linked List**
- 특징
  - 여러개의 데이터를 한꺼번에 다룰 수 있다.
  - 메모리상에 연속되지 않아도 된다 → 미리 공간을 확보해놓지 않아도 된다.
  - 필요에 따라 데이터가 늘어나거나 줄어든다.
  - index를 알려면 한 칸 한 칸 이동해야해서 속도가 느리다.

### ArrayList

- resizable list

- 크기를 변경하면 새로운 곳에 복사하고 기존걸 지움

- 생성할  때 크기를 정해줌

  `List<MyData> list = new ArrayList<>(10);`

- is not Synchronized

- **멀티스레드 환경이 아닐 때, 인덱스를 빠르게 찾고자 할 때 주로 사용**

### Vector

- growable list
- 생성할 때 크기를 정해준다.
- 몇 개씩 늘릴건지 정해줄 수 있다.
- is Synchronized
- 멀티스레드 환경에서는 ArrayList 보단 Vector를 주로 씀

```java
package Java;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Objects;
import java.util.Vector;

/* 2023-07-03
 * 
 * LinkedList
 * List 인터페이스
 * 
 * ArrayList (not synch)
 * Vector (synch) 
*/


class MyData {
	int value;
	
	public MyData(int value) {
		this.value = value;
	}

	static MyData create(int v) {
		return new MyData(v);
	}
	
	@Override
	public String toString() {
		return ""+value;
	}
	
	@Override
	public boolean equals(Object o) {
		if(this == o) return true;
		if(o == null || getClass() != o.getClass()) return false;
		MyData myData = (MyData) o;
		return value == myData.value;
	}
	
	@Override
	public int hashCode() {
		return Objects.hash(value);
	}
}

public class Array_List {
	public static void main(String[] args) {
		List<MyData> list = new LinkedList<>();
		List<MyData> list2 = new Vector<>(); // 벡터형인데, 리스트 형으로도 표현 가능하다. => 다형성
		List<MyData> list3 = new ArrayList<>(10); // resizable list
		list.add(MyData.create(1));
		list.add(MyData.create(2));
		list.add(MyData.create(3));
		
		System.out.println(list);
		System.out.println(list.contains(MyData.create(2)));
		System.out.println(list.indexOf(MyData.create(3))); // contains를 이용해 index 찾으러 가는 것
		method1(list);
		method1(list2);
	}
	
	static void method1(List<MyData> list) {
		System.out.println(list);
		System.out.println(list.size());
		System.out.println(list.isEmpty());
	}
}
```



# ✏️ 문제풀기

## 최댓값 인덱스 구하기

###### 문제 설명

주어진 입력중 최대값을 구하고, 최대값이 이 위치하는 index 값의 목록을 반환하세요.

입력:
[1, 3, 5, 4, 5, 2, 1]

입력된 목록의 최대값은 5입니다.
5와 동일한 값을 가진 위치는 3번째, 5번째 위치 입니다.
이 위치에 해당하는 index는 [2, 4] 입니다.

출력:
[2, 4]

------

입출력 예

입력: [1, 3, 5, 4, 5, 2, 1]
출력: [2, 4]
👉🏻 위와 같습니다.

입력: [3, 6, 10, 1, 7, 2, 4, 6, 10, 9]
출력: [2, 8]
👉🏻 최대값 10이 위치하는 곳은 3번째, 9번째 입니다. 이 위치의 index값은 2, 8입니다.



### 내 풀이

```java
import java.util.*;

class Solution {
    public List solution(int[] arr) {
        List<Integer> answer = new ArrayList<>();
        
        int max=arr[0];
        for(int i=0; i<arr.length; i++){
            if(max<arr[i]) max=arr[i];
        }
        for(int i=0; i<arr.length; i++){
            if(max==arr[i]) answer.add(i);
        }
        
        return answer;
    }
}
```



## 순열 검사

###### 문제 설명

길이가 n인 배열에 1부터 n까지 숫자가 중복 없이 한 번씩 들어 있는지를 확인하려고 합니다.
1부터 n까지 숫자가 중복 없이 한 번씩 들어 있는 경우 true를, 아닌 경우 false를 반환하도록 함수 solution을 완성해주세요.

##### 제한사항

- 배열의 길이는 10만 이하입니다.
- 배열의 원소는 0 이상 10만 이하인 정수입니다.

------

##### 입출력 예

| arr          | result |
| ------------ | ------ |
| [4, 1, 3, 2] | true   |
| [4, 1, 3]    | false  |

##### 입출력 예 설명

입출력 예 #1
입력이 [4, 1, 3, 2]가 주어진 경우, 배열의 길이가 4이므로 배열에는 1부터 4까지 숫자가 모두 들어 있어야 합니다. [4, 1, 3, 2]에는 1부터 4까지의 숫자가 모두 들어 있으므로 true를 반환하면 됩니다.

입출력 예 #2
[4, 1, 3]이 주어진 경우, 배열의 길이가 3이므로 배열에는 1부터 3까지 숫자가 모두 들어 있어야 합니다. [4, 1, 3]에는 2가 없고 4가 있으므로 false를 반환하면 됩니다.



### 내 풀이

```java
import java.util.*;

class Solution {
    public boolean solution(int[] arr) {
        boolean answer = true;
        Arrays.sort(arr);
        for(int i=0; i<arr.length; i++){
            if(arr[i]!=i+1) return false;
        }
        return answer;
    }
}
```



## 자연수 뒤집어 배열로 만들기

###### 문제 설명

자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

##### 제한 조건

- n은 10,000,000,000이하인 자연수입니다.

##### 입출력 예

| n     | return      |
| ----- | ----------- |
| 12345 | [5,4,3,2,1] |



### 내 풀이

```java
import java.util.*;

class Solution {
    public List<Long> solution(long n) {
        List<Long> answer = new ArrayList<>();
        while(n>0) {
            answer.add(n%10);
            n/=10;
        }
        
        return answer;
    }
}
```

