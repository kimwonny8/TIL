# C언어

[toc]



- 20230315



## 입출력 – scanf, printf

``` c
#include <stdio.h>
 main()  {
      int  i, j, k;
      scanf(“%d %d”, &i, &j);  // 6 4
      k = i + j;
      printf(“%d \n”, k);
}
```

- `&`: 주소



``` c
#include <stdio.h>
 main()  {
      int  n1 = 15, n2 = 22; 
     // 참고 : XOR : ^ , OR : | , AND : &
      n1 ^= n2;  // 25 
      n2 ^= n1;  // 15
      n1 ^= n2;  // 22
     printf(“%d %d ”,n1, n2);
}  // 참고 : %d, %o. %x, %c, %s, %f
```

- `XOR` : 같으면 0, 다르면 1
- 15 == 00001111
- 22 == 00010110



``` c
#include <stdio.h>
 main()  {
      int result, a=100, b=200, c=300;
      result = a < b ?  b++ : --c;
      printf(“ %d, %d, %d \n”, result, b, c) 
} 
```

- 출력: `200 201 300` 



```c
#include <stdio.h>
 main()  {
      int  m , n; 
     scanf(“%o#%x”, &n, &m); // 입력 15#22
     printf(“%d %d ”,n, m);
}  
```

- `%o` : 8진수 - 숫자 15로 읽는 게 아니라 1, 5로 읽음 => 8 + 5
- `%x` : 16진수 
- 출력 : `13 34`



``` c
#include <stdio.h>
main()  {
    int  j=024, k=24, L=0x24, hap; 
    hap = j + k + L;
	printf(“%d, %d, %d, %d ”, j, k, L, hap);
} 
```

- 숫자로 인식하기 위해 앞에 0을 적음
- 출력 : `20, 24, 36, 80`



``` c
#include <stdio.h>
 main()  {
      int  a=5, b=10, c=15, d=20, result; 
      result = a * 3 + b > d || c – b / a <= d && 1;
      printf(“%d \n”, result);
}  
```

- 연산자 우선순위 : 수치 > 논리 > 관계
  1. a * 3 + b 계산: 5 * 3 + 10 = 25
  2. c - b / a 계산: 15 - 10 / 5 = 13
  3. d와 25를 비교하여 a * 3 + b > d가 거짓이므로 false 값인 0이 됩니다.
  4. 1을 d와 비교하여 c - b / a <= d가 참이므로 true 값인 1이 됩니다.
  5. 논리 연산자 ||와 &&의 우선순위에 따라 논리식이 다시 계산됩니다. 먼저 &&가 계산되어 1이 됩니다.
  6. 마지막으로 ||가 계산되어 1이 결과값으로 출력됩니다.
- 0이 아닌 수 `true` , 0만 'false'
- 출력: `1`



## 제어문 ( if, switch, for, while )

``` c
#include <stdio.h>
 main()  {
      int score[] = { 86, 53, 95, 76, 61 };
      char grade, str[]=“Rank”;
      for ( int i= 0; i < 5; i++) {
            switch (score[i]/10) {
                  case 10:   case 9:    grade = ‘A’;          break; 
                  case 8:               grade = ‘B’;          break; 
                  case 7:               grade = ‘C’;          break; 
                  default:              grade = ‘F’;
               }
          if (grade != ‘F’) printf(“%d is %c %s \n, i+1, grade, str);
          }
 } 
```

- 출력 : `1 is A Rank \n 3 is A Rank \n 4 is C Rank`



```c
#include <stdio.h>
main() {
     int i = 1, n = 0;
     while (i <=50) {
         if (i % 7 == 0) n+= i;
         i++;
      }
      printf("%d", n);
}
```

- 출력: `196` (50 이하인 7의 배수 더하기)



``` c
#include <stdio.h>
main() {
     int arr[6];
     int max = 0, min = 99;
     int sum = 0;
     for (int i = 0; i < 6; i++) {
          arr[i] = i * i; // 0 1 4 9 16 25
          sum += arr[i]; // 55
      } 
     for (int i = 0; i < 6; i++) {
             if (max < arr[i])  max = arr[i]; // 25
             if (min > arr[i])  min = arr[i]; // 0
     }
     printf("%.2f", (sum - max - min) / 4.0);
}
```

- 출력 : ` 7.50` => (55 - 25 - 0) / 4.0 



``` c
#include <stdio.h>
main() {
    int a = 27, b = 12;
    int L, g;      
    for (int i = b; i > 0; i--) {
        if (a % i == 0 && b % i == 0) {
            g = i;   // 3
            break;
        }
    }
    L = a * b / g;  // 27 * 12 / 3
    printf("%d", g + L);
} 
```

- 최대공약수 문제
- 출력 : `108`



``` c
#include <stdio.h>
main() {
    int num=35, evencnt=0, oddcnt=0;
    for (int i = 1; i <= num; i++) {
        if (i % 2 == 0) evencnt++;  
        else oddcnt++; 
    }
	printf("%d %d", evencnt, oddcnt);	
}
```

- 출력 : `17 18`



``` c
#include <stdio.h>
main() {
    int n = 3, r = 0;
    for (int i = 1; i < 10; i = i + 2)  r = r + n * i;
    printf("%d", r);
}
```

- 출력 : `75`



``` C
#include <stdio.h>
main() {
    int a[3][5] = { {27, 13, 21, 41, 12}, 
                   {11, 20, 17, 35, 15 },
                   {21, 15, 32, 14, 10 } };
    int sum, ssum = 0;
    for (int i = 0; i < 3; i++) {
        sum = 0;
        for (int j = 0; j < 5; j++)  sum += a[i][j];
        ssum += sum;
    }
    printf("%d", ssum);
}   	
```

- 이차원 배열 안에 있는 중괄호는 없어도 C언어는 알아서 값 들어감
- 출력 : `304`



## **포인터**  * , &

```c
#include <stdio.h>
main() {
    int a = 50;
    int *b = &a;
    *b= *b + 20:
    printf("%d %d \n", a, *b); //  70  70
    
    char *s;
    s = "gilbut";
    for (int i = 0; i < 6; i += 2) {
        printf("%c, ", s[i]);  // g, g, gilbut
        printf("%c, ", *(s + i)); // l, l, lbut
        printf("%s \n", s + i); // u, u, ubut
    }
}
```

- `int *b = &a;` : b는 정수형 포인터(주소) = a의 주소
- `s[0]` = g
- `*(s+0)` = g
- `s+0` = g



``` c
#include <stdio.h>
main() {
    char a[3][5] = { "KOR", "HUM", "RES" };
    char* pa[] = { a[0], a[1], a[2] };
    int n = sizeof(pa) / sizeof(pa[0]); 	// 3
    for (int i = 0; i < n; i++)  { 
        printf("%c", pa[i][i]); // printf("%c", *(pa[i]+i)); 이랑 같음
    }
} 
```

- `pa` 는 주소의 값
- `sizeof(pa)` : (한 개의 주소가 4바이트라고 가정하면) 12바이트
- `sizeof(pa[0])` : 4바이트니까 4
- 출력 : `K U S` => `pa[0][0]`, `pa[1][1]`, `pa[2][2]` 출력함



``` c
#include <stdio.h>
main() {
    char *p = "KOREA";
    printf("%s \n", p);
    printf("%s \n", p + 3); 
    printf("%c \n", *p);
    printf("%c \n", *(p + 3));
    printf("%c \n", *p + 2);
}

```

- 출력 : `KOREA \n EA \n K \n E \n M`

 

``` c
#include <stdio.h>
int main() {
    int ary[3];
    ints=0;
    *(ary+ 0) = 1;		// 1
    ary[1] = *(ary+ 0) + 2; 	// 3
    ary[2] = *ary + 3;		// 4
    for (int i = 0; i < 3; i++)  s = s + ary[i];
    printf("%d", s);
} 
```

- 출력 : `9` ( 1+3+4 )



``` c
#include <stdio.h>
int main() {
    int * array [3];
    int a = 12, b=24, c=36;
    array[0] = &a;	// a의 주소
    array[1] = &b;	// b의 주소
    array[2] = &c;	// c의 주소
    printf("%d", *array[1] + **array + 1);
}
```

- `**array` : array 배열의 첫 번째 포인터가 가리키는 정수의 값 => 12
- 출력 : `37` ( 24+12+1 )



## **구조체** **–** 여러가지 자료형을 하나로 만드는 것

``` c
#include <stdio.h>
struct jsu {
    char nae[12];
    int os, db, hab, hhab;
}
int main() {
    struct jsu st[3] = { {“데이터1”, 95, 88 } , {“데이터2”, 84, 91 } , {“데이터3”, 86, 75 } }; 
    struct jsu* p;
    p = &st[0];
    (p+1)->hab = (p+1)->os + (p+2)->db; 
    (p+1)->hhab = (p+1)->hab + p->os + p->db; 
    printf(“%d”, (p+1)->hap + (p+1)->hhab); 
} 
```

- p는 구조체를 가르키는 포인터로 사용하겠다. => st 전체를 가르키는 주소

- p 는 데이터1, p+1는 데이터2, p+2는 데이터3

- `->` : 구조체 안에있는 멤버

- `(p+1) ->hab = (p+1)->os + (p+2)->db; `  : 159

  - (p+1) 의 hab 은 (p+1)의 os + (p+2)의 db
    - (p+1)의 os는 84
    - (p+2)의 db는 75

- ` (p+1)->hhab = (p+1)->hab + p->os + p->db; ` : 342

  - p->os는 95
  - p->db는 88

  

```c
#include <stdio.h>
int main() {
    struct insa {
        char name[10];
        int age;
    } a[] = { “Kim”, 28 , “Lee”, 38, “Park”,42, “Choi”, 31 }; 
    struct insa* p;
    p = a;
    p++;
    printf(“%s \n”, p->name); 
    printf(“%d \n”, p->age); 
} 
```

- p는 a[]의 시작 부분을 가르키고있고
- `printf(“%s \n”, p->name); ` : `Lee`
-  `printf(“%d \n”, p->age); `:`38`



## 사용자 정의 함수

```c
#include <stdio.h>

int factorial(int n) {
    if (n<=1) return 1;
    else return n * factorial(n-1);
 } 

int main() {
     int (*pf)(int); 
     pf = factorial;
     printf(“%d”, pf(3));   
} 
```

- 출력 : `6` ( `printf(“%d”, factorial(3));` )



``` c
#include <stdio.h>
#include <math.h>
main() {
      int arr[5];
      for (int i = 0; i < 5; i++)  arr[i] = (i + 2) +(i * 2); // 2 5 8 11 14
      for (int i = 0; i < 5; i++)  printf("%d", check(arr[i])); 
}
int check (int a) {
      int n = (int) sqrt(a);
      int i = 2; 
      while (i <= n) {
             if (a % i == 0) return 0;
             i++;
       }
       return 1;
}
```

- 출력: `1 1 0 1 0`



```c
#include <stdio.h>
func (int *p) {
     printf("%d \n", *p);
     printf("%d \n", p[2]);
}
main() { 
     int a[7] = {1, 2, 3, 4, 5};
     func(a);
     func(a + 2);
}
```





```c
#include <stdio.h>
void align (int a[]) {
    int temp;
    for (int i = 0; i < 4; i++)
         for (int j = 0; j < 4 - i; j++) 
              if (a[j] > a[j+1]) {
                  temp = a[j];
                  a[j] = a[j+1];
                  a[j+1] = temp;
             }
 }    
main() {
    int a[] = { 85, 75, 50, 100, 95 };
    align (a);
    for (int i = 0; i < 5; i++)  printf("%d ", a[i]);
}
```

