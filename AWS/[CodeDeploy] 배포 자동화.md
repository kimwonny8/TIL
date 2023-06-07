ec2에 jdk17 설치

```
sudo yum install java-17-amazon-corretto

java -version
```

## ubuntu

```
sudo apt update
sudo apt install ruby-full
sudo apt install wget

cd /home/ubuntu

sudo apt install awscli
aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2
# 안되면 아래
aws --debug --no-sign-request s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2

chmod +x ./install
sudo ./install auto

# 아래 코드로 설치 확인
sudo service codedeploy-agent status
```

```
# jdk 17버전 설치
sudo apt-get install openjdk-17-jdk

# java 설치 확인
java -version
```



## unix

codedeploy 에이전트 설치

```
sudo yum update
sudo yum install ruby
sudo yum install wget

cd /home/ubuntu

sudo yum install awscli

aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2

wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install

chmod +x ./install

sudo ./install auto
```

```
systemctl status codedeploy-agent

systemctl --now enable codedeploy-agent

yum info codedeploy-agent
```

https://sangchul.kr/entry/aws-codedeploy-%EC%97%90%EC%9D%B4%EC%A0%84%ED%8A%B8-%EC%84%A4%EC%B9%98codedeploy-agent-install

https://be-developer.tistory.com/51



## deploy.yml

```yml
name: mydays deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  FRONTEND_S3_BUCKET_NAME: www.wonny.kim
  BACKEND_S3_BUCKET_NAME: mydays-deploy

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      # Backend 빌드 및 배포
      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          java-version: '17'
          distribution: 'temurin'

      - uses: actions/checkout@v3
      - run: touch ./backend/src/main/resources/application.properties
      - run: echo "${{ secrets.APPLICATION }}" > ./backend/src/main/resources/application.properties
      - run: cat ./backend/src/main/resources/application.properties

      - name: Grant execute permission for gradlew
        run: cd backend && chmod +x gradlew

      - name: Build with Gradle
        run: cd backend && ./gradlew clean build

      # Backend 파일 복사
      - name: Copy Backend files
        run: |
          mkdir -p deploy/backend
          cp backend/build/libs/*.jar deploy/backend/
          cp backend/appspec.yml deploy/
          cp backend/scripts/*.sh deploy/

      # Frontend 빌드 및 배포
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '18'

      - name: Install Dependencies and Build Frontend
        run: |
          cd frontend
          npm ci
          npm run build

      - name: Copy Frontend files
        run: |
          mkdir -p deploy/frontend
          cp -R frontend/dist/* deploy/frontend/

      # Zip 파일 생성
      - name: Make zip file
        run: cd deploy && zip -r ../mydays.zip ./*
        shell: bash

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-2

      # Backend Zip 파일 S3에 업로드
      - name: Upload Backend to S3
        run: aws s3 cp --region ap-northeast-2 ./mydays.zip s3://${{ env.BACKEND_S3_BUCKET_NAME }}/

      # Frontend Zip 파일 S3에 업로드
      - name: Upload Frontend to S3
        run: aws s3 cp --region ap-northeast-2 ./mydays.zip s3://${{ env.FRONTEND_S3_BUCKET_NAME }}/

      # Backend Deploy
      - name: Deploy Backend
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: |
          aws deploy create-deployment \
            --application-name mydays \
            --deployment-group-name mydays-group \
            --file-exists-behavior OVERWRITE \
            --s3-location bucket=${{ env.BACKEND_S3_BUCKET_NAME }},bundleType=zip,key=mydays.zip \
            --region ap-northeast-2

      # Frontend Deploy
      - name: Deploy Frontend
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: |
          aws s3 sync ./deploy/frontend/ s3://${{ env.FRONTEND_S3_BUCKET_NAME }}/
```

## backend > appspec.yml

```yml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/app/
    overwrite: yes

permissions:
  - object: /
    pattern: "**"
    owner: ec2-user
    group: ec2-user

hooks:
  ApplicationStart:
    - location: deploy.sh
      timeout: 60
      runas: ec2-user
```

## backend > scripts > deploy.sh

```sh
#!/usr/bin/env bash

REPOSITORY=/home/ec2-user/app/backend

echo "> 현재 구동 중인 애플리케이션 pid 확인"

CURRENT_PID=$(pgrep -fla java | grep hayan | awk '{print $1}')

echo "현재 구동 중인 애플리케이션 pid: $CURRENT_PID"

if [ -z "$CURRENT_PID" ]; then
  echo "현재 구동 중인 애플리케이션이 없으므로 종료하지 않습니다."
else
  echo "> kill -15 $CURRENT_PID"
  kill -15 $CURRENT_PID
  sleep 5
fi

echo "> 새 애플리케이션 배포"

JAR_NAME=$(ls -tr $REPOSITORY/*SNAPSHOT.jar | tail -n 1)

echo "> JAR NAME: $JAR_NAME"

echo "> $JAR_NAME 에 실행권한 추가"

chmod +x $JAR_NAME

echo "> $JAR_NAME 실행"

nohup java -jar -Duser.timezone=Asia/Seoul $JAR_NAME >> $REPOSITORY/nohup.out 2>&1 &
```



