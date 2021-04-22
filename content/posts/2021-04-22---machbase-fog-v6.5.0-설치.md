---
title: "machbase fog v6.5.0 설치"
date: "2021-04-22"
template: "post"
draft: false
slug: "machbase-fog-v6.5.0-설치"
category: "Machbase"
tags:
  - "#Machbase"
description: ""
---

CentOS7에 machbase를 설치해보겠습니다.

### 시스템 사양
+ cpu : AMD Ryzen 7 2700X Eight-Core Processor (16CPUs), ~3.7GHz
+ OS : centos-release-7-9.2009.1.el7.centos.x86_64
+ Virtual machine : VMware Workstaion 16 Player

### Tarball 설치 파일
1. [마크베이스 홈페이지](https://www.machbase.com/)에 접속한 후 우측 최상단에 'GET MACHBASE'를 클릭합니다.
![machbase-install-01.png](/media/posts/2021-04-22---Machbase-설치/machbase-install-01.png)

2. 아래의 내용들을 작성한 후 제출합니다.
![machbase-install-02.png](/media/posts/2021-04-22---Machbase-설치/machbase-install-02.png)

3. [http://dl.machbase.com/dist/6.5.0/machbase-fog-6.5.0.official-LINUX-X86-64-release.tgz](http://dl.machbase.com/dist/6.5.0/machbase-fog-6.5.0.official-LINUX-X86-64-release.tgz)로 접속해서 파일을 받습니다.
![machbase-install-03.png](/media/posts/2021-04-22---Machbase-설치/machbase-install-03.png)

### 파일 LIMIT 설정

파일 limit에 대해 default 설정으로 진행을 하게 되면 아래와 같이 파일의 갯수를 최대 8192개로 바꿔달라는 Error가 나타납니다.
```bash
[Error] Failed to initialize database engine.
Handle limit(1024) from the system is less than that of property(8192). Tune system handle limit or decrease the property 'HANDLE_LIMIT', Errorcode :BCF
```

machbase의 설치하기 위해서는 파일 Limit을 변경해주어야 합니다.  
```bash
[machbase@localhost ~]$ ulimit -Sn
1024 # default size

[machbase@localhost ~]$ sudo vi /etc/security/limits.conf

# 아래의 내용을 추가
#<domain>     <type>     <item>     <value>
#
*              hard        nofile     65535
*              soft        nofile     65535
```
재부팅하면 설정이 완료 됩니다.
```bash
[machbase@localhost ~]$ ulimit -Sn
65535
```

### 환경변수 설정
```bash
[machbase@localhost machbase_home]$ tar -xvf machbase-fog-6.5.0.official-LINUX-X86-64-release.tgz

[machbase@localhost ~]$ vi ~/.bashrc

# machbase
export MACHBASE_HOME=/home/machbase/machbase_home
export PATH=$MACHBASE_HOME/bin:$PATH
export LD_LIBRARY_PATH=$MACHBASE_HOME/lib:$LD_LIBRARY_PATH
```

### Machbase DB 생성
```bash
[machbase@localhost ~]$ machadmin -c
-----------------------------------------------------------------
     Machbase Administration Tool
     Release Version - 6.5.0.official 
     Copyright 2014, MACHBASE Corp. or its subsidiaries
     All Rights Reserved
-----------------------------------------------------------------
Database created successfully.
```

### Machbase 서버 실행
```bash
[machbase@localhost ~]$ machadmin -u
-----------------------------------------------------------------
     Machbase Administration Tool
     Release Version - 6.5.0.official 
     Copyright 2014, MACHBASE Corp. or its subsidiaries
     All Rights Reserved
-----------------------------------------------------------------
Waiting for Machbase server start
Machbase server started successfully.
HTTP service enabled.(http://localhost:5657/)
```

### Machbase 서버 접속
machsql 이라는 접속 유틸리티를 이용하여 마크베이스 서버에 접속합니다.  
관리자 계정인 SYS가 준비되어 있으며, 패스워드는 MANAGER로 설정되어 있습니다.

```bash
[machbase@localhost ~]$ machsql
=================================================================
     Machbase Client Query Utility
     Release Version 6.5.0.official
     Copyright 2014 MACHBASE Corporation or its subsidiaries.
     All Rights Reserved.
=================================================================
Machbase server address (Default:127.0.0.1) : 
Machbase user ID  (Default:SYS)
Machbase User Password : 
MACHBASE_CONNECT_MODE=INET, PORT=5656
Type 'help' to display a list of available commands.
Mach>
```

### Machbase 서버 중단
```bash
Mach> exit
[machbase@localhost ~]$ machadmin -s
-----------------------------------------------------------------
     Machbase Administration Tool
     Release Version - 6.5.0.official 
     Copyright 2014, MACHBASE Corp. or its subsidiaries
     All Rights Reserved
-----------------------------------------------------------------
Waiting for Machbase server shut down...
Machbase server shut down successfully.
```

### MWA 서버 실행
Machbase Web Analytics (MWA)는 Python 2.7과 Flask기반의 Werkzeug, Jinja2로 개발된 Web application입니다.
```bash
[machbase@localhost ~]$ MWAserver start
SERVER STARTED, PID : 3597
     Connection URL : http://192.168.91.130:5001
```
![MWA.png](/media/posts/2021-04-22---Machbase-설치/MWA.png)
제공되는 URL로 접속하면 기본적인 id, pw의 admin / machbase 로 로그인할 수 있습니다.

#### 쿼리 대시보드
![MWA-dashboard-01.png](/media/posts/2021-04-22---Machbase-설치/MWA-dashboard-01.png)

### 데이터베이스 삭제
```bash
[machbase@localhost machbase_home]$ machadmin -d
-----------------------------------------------------------------
     Machbase Administration Tool
     Release Version - x.x.x.official
     Copyright 2014, MACHBASE Corp. or its subsidiaries
     All Rights Reserved
-----------------------------------------------------------------
Destroy Machbase database. Are you sure?(y/N) y
Database destoryed successfully.
```

### 참고 자료
+ [Machbase 6 Manual-Tarball 설치](http://krdoc.machbase.com/pages/viewpage.action?pageId=13436158)  
+ [마크베이스(Machbase) RPM을 레드햇 계열 리눅스인 CentOS에 설치해 보자](https://machbase.tistory.com/2)  
+ [[튜토리얼 1/10] Machbase v6 - 제품 설치](https://www.youtube.com/watch?v=13pgFtxMvc4&list=LL&index=4)