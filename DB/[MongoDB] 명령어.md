**1. Database**

\- 데이터베이스는 컬렉션의 물리적 컨테이너 입니다. 하나의 데이터베이스에는 보통 여러개의 컬렉션을 가지고 있습니다.



**2. Collection**

\- 컬렉션은 몽고DB Document 의 그룹이며 RDBMS 의 예를 들면 Table 과 개념과 유사합니다.

\- 컬렉션은 단일 데이터베이스에 존재합니다.

\- 컬렉션은 스키마를 강요하지 않습니다. 따라서 컬렉션 내부의 도큐먼트는 서로 다른 필드를 가질수 있습니다.

\- 컬렉션 안에 도큐먼트는 일반적으로 서로 유사한 하거나 관련된 목적이 있습니다.



**3. Document**

\- Docuemtn 는 하나의 키(key) 와 값(value)의 집합으로 이루어져 있으며 동적 스키마 입니다.

\- 동적 스키마는 동일한 컬랙션 내의 도큐먼트가 동일한 필드 또는 구조를 가지필요 없을을 의미한다.

\- 그리고 동일한 필드안에 다른타입의 데이타를 보유할수 있음을 의미합니다.





## Shell Instructions (쉘 명령어)

**show databases (=show dbs)**
\- 현재 MongoDB에 생성된 데이터베이스들의 이름과 크기를 출력한다.

**show collections**
\- 현재 데이터베이스에 생성된 Collection들의 이름을 출력한다.

**use <database_name>**
\- <database_name> 데이터베이스를 선택한다.

**db.createCollection("<collection_name>")**
\- 현재 데이터베이스에 <collection_name> Collection을 생성한다.
\- {"ok" : 1} 메시지가 출력되면 Collection이 정상적으로 생성된 것이다.