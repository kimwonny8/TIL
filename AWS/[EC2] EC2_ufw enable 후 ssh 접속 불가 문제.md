https://gksdudrb922.tistory.com/202



혹은



### 1. 인스턴스 중지 (종료아님)

해당 인스턴스 선택 후 [인스턴스 상태] - [인스턴스 중지]
한참 후에 인스턴스가 멈춘다

### 2. 사용자 데이터 편집

중지 상태가 된 후에 [작업] - [인스턴스 설정] - [사용자 데이터 편집] 으로 들어가 해당 코드를 복붙 후 저장한다.

```null
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0
--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"
#cloud-config
cloud_final_modules:
- [scripts-user, always]
--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"
#!/bin/bash
ufw disable
iptables -L
iptables -F
--//
```

### 3. 인스턴스 시작