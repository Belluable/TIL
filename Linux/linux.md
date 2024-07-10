# Linux(Cloud)

Tags: AWS
Start Date: 2024년 7월 8일

## Linux

- GNU
- unix의 kernel을 확장해서 만듦
- git 만든 사람이 Linux 만듦

### Linux Kernel

- Management the Memory, File System, CPU, Device, …
- CentOS 8 - Kernal ver.4.18

![linux_1](linux/1.png)

### Linux Distro

- Red Hat
    - RHEL (Red Hat Enterprise Linux)
    - CentOS
    - Fedora
- Debian
    - Debain, Ubuntu, KNOPPIX
- Slackware
    - openSUSE(Novell)

### Install Linux

![linux_2](linux/2.png)

![linux_3](linux/3.png)

## Cloud Platform

### Serverless(XaaS)

- Serviceful Serverless: Firebase
- FaaS (Fuctions as a Service): Amazon Lambda
- AaaS (Application)
- IaaS (Infrastructure): Public Cloud Infrastructure Hosting
- SaaS (Software): Mail, VAN, Google Docs, game
- PaaS (Platform): Dev, framework&tools, deployment, code lifecycle, DB, APIs

![linux_4](linux/4.png)

- on-premise: close cloud (회사에서 사용)
    
    ![linux_5](linux/5.png)
    

## Server Settings

### 알아둘 개념들

- EC2: 컴퓨티드 엔진(os를 탑재한서버)
- os: linux
- machine: 실제 기계,
- virtual: 가상
- image: window, excel, python, linux, mysql 등 모든 어플리케이션(마치 이미지처럼 떠놓은거)
- instance: 실제 떠있는 서버
- 키: pc ↔ EC2 연결 (키페어)
- shell: 명령어들의 집합 (linux 기본 shell: bash shell)
    
    *linux에서 shell 더 깊게(강의 참조): 7/8(월) 04:45:50부분
    

- `chmod 600 seocho.pem` 에서 600의 의미
    
    ![linux_6](linux/6.png)
    
    ![linux_7](linux/7.png)
    

- 전체적인 구성: browser, NginX, node, pm2, RDS(sql)
    
    ![linux_8](linux/8.png)

### AWS 서버 설치

- 내 PC에서 EC2 들어가기
    
    ```bash
    # 터미널에서 EC2 들어가기 내거 저장
    ssh -i seocho.pem ec2-user@13.54.113.219
    ```
    
- EC2 터미널 앞에 보여지는 경로 테마 수정 :
    
    ```bash
    # vi bash_profile 
    export PS1='\e[0;33m[\t:\u \w]$ \e[m'
    ```
    

### Install NginX by dnf

```bash
ec2-user> sudo dnf install nginx

# 포트 확인, telnet 설치
ec2-user> sudo dnf install telnet
ec2-user> telnet localhost 80

# ps: 작업 관리자에 뭐가 떠있나 봄, grep: 찾음(필터링)
ec2-user> ps -ef | grep nginx

# 실행
sudo nginx
# nginx의 홈디렉토리, nginx가 설치돼있는 곳
cd /etc/nginx/
```

ssh: telnet을 한번 더 암호화함

telnet: 내가 나를 부름

EC2 - yarn 설치

[https://velog.io/@whdgnszz1/AWS-AWS-EC2-Amazon-Linux-에Node.js-yarn패키지-설치하기](https://velog.io/@whdgnszz1/AWS-AWS-EC2-Amazon-Linux-%EC%97%90Node.js-yarn%ED%8C%A8%ED%82%A4%EC%A7%80-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

## Client Settings

## 필수 명령어 및 Linux 구성

(NW, Disk, …)

## Shell Script

## 실무에서 알면 편한 기술