---
layout: default
title: Dockerfile
parent: Docker
nav_order: 4
---

# Dockerfile
{: .no_toc }


Dockerfile로 서버 구축
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## Dockerfile의 기본

### Dockerfile의 용도
- Dockerfile은 Docker상에서 동작하는 컨테이너 구성 정보를 저장한 파일
- Docker는 docker build 커맨드를 통해 Dockerfile에 저장된 구성 정보를 기반으로 Docker 이미지를 생성함

### Dockerfile 기본 구성
- Dockerfile은 에디터 등으로 작성할 수 있는 텍스트 형식의 파일
- 확장자 불필요
- 'Dockerfile'이라는 이름의 파일에 인프라 구성 정보를 코딩
- Dockerfile 이외의 파일명으로도 동작하며, 이때는 Dockerfile에서 이미지를 build할 때 파일명을 명시적으로 지정해야함
- 명령어는 대문자와 소문자 모두 상관없으나 통상적으로 대문자로 통일
- Dockerfile 형식
  ```
  명령어 값
  ```
- Dockerfile 명령어
  - |명령|설명|
    |:--|:--|
    |`FROM`|베이스 이미지 지정|
    |`MAINTAINER`|Dockerfile 생성자|
    |`RUN`|커맨드 실행|
    |`CMD`|데몬 실행|
    |`LABEL`|라벨 설정|
    |`EXPOSE`|포트 expose|
    |`ENV`|환경변수 설정|
    |`ADD`|파일 및 디렉터리 추가|
    |`COPY`|파일 복사|
    |`VOLUME`|볼륨 마운트|
    |`ENTRYPOINT`|데몬 실행|
    |`USER`|사용자 설정|
    |`WORKDIR`|작업 디렉터리 지정|
    |`ONBUILD`|build 완료 후 실행될 명령어|
- Dockerfile에서 코멘트 작성 방법 1
  ```
  # 코멘트
  명령어 값
  ```
- Dockerfile에서 코멘트 작성 방법 2
  ```
  # 코멘트
  명령어 값 # 코멘트
  ```

### Dockerfile 작성
- Dockerfile에는 '어떤 Docker 이미지로부터 Docker 컨테이너가 생성되었는가'에 대한 정보가 반드시 저장되어 있어야 함(베이스 이미지)
- FROM
  ```
  FROM [이미지명]
  FROM [이미지명]:[태그명]
  FROM [이미지명]@[Digest]
  ```
- CentOS7을 베이스 이미지로 한 Dockerfile
  ```
  # 베이스 이미지 설정
  FROM centos:centos7
  ```
- digest 확인
  ```
  $ docker images --digests asdf/dockersample
  ```
- digest를 입력한 Dockerfile
  ```
  $ FROM asdf/dockersample@sha256:1234567890...
  ```
- MAINTAINER
  ```
  MAINTAINER [Dockerfile작성자]
  ```
- Dockerfile 내용(MAINTAINER 명령의 예)
  ```
  # 베이스 이미지 설정
  FROM centos:latest

  # 작성자 정보
  MAINTAINER ASDF asdf@mail.com
  ```

### Dockerfile로 Docker 이미지 생성
- docker build
  ```
  $ docker build -t [생성할 이미지명]:[태그명][Dockerfile 경로]
  ```
- Dockerfile 생성
  ```
  $ mkdir sample && cd $_
  $ touch Dockerfile
  ```
- Dockerfile 내용
  ```
  # 베이스 이미지 설정
  FROM centos:centos7
  ```
- docker build 커맨드 실행
  ```
  $ docker build -t sample:1.0 /home/docker/sample
  ```
- 최초 실행 이후에는 Docker repository에서 베이스 이미지를 다운로드하지 않고 바로 생성
- 베이스 이미지와 IMAGE ID가 같으며 서로 다른 이름을 갖게 되나 그 실체는 같은 이미지임
- 파일명을 지정하여 docker build 커맨드 실행
  ```
  $ docker build -t sample -f Dockerfile.base
  ```
  - 파일명이 Dockerfile이 아닌 경우에는 Docker Hub에서 이미지가 자동 생성되지 않음
- 표준 입력으로 build
  ```
  $ docker build - < Dockerfile
  ```
    - 이 경우 ADD 명령어로 이미지 내에 파일을 추가할 수 없는 등 build에 필수적인 파일을 포함할 수 없음
- 압축 파일을 표준 입력으로 build
  ```
  $ tar tvfz docker.tar.gz

  $ docker build - < docker.tar.gz
  ```
  - tar 압축을 통해 Dockerfile과 build에 필수적인 파일을 표준 입력으로 지정할 수 있음
- 이미지 캐시
  - build 로그에 'Using cache'가 표시됨
  - 이미지 캐시가 필요 없을 때는 docker build 커맨드에서 --no-cache 옵션을 사용

### Docker 이미지 레이어 구조
- 생성된 여러 이미지는 레이어 구조를 가짐
- 네가지 명령을 가진 Dockerfile
  ```
  # STOP:0 CentOS(베이스 이미지)
  FROM centos:latest

  # STOP:1 Apache 설치
  RUN yum install -y httpd
  
  # STOP:2 파일 복사
  COPY index.html /var/www/html/
  
  # STOP:3 Apache 구동
  CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
  ```
- Docker는 이미지를 중첩하여 디스크 용량을 효율적으로 사용


## 커맨드 및 데몬 실행

### RUN(커맨드 실행)
- 컨테이너에서 FROM 명령어로 베이스 이미지를 지정하고, '애플리케이션과 미들웨어 설치 및 설정', '커맨드를 통한 환경 구축' 등 RUN 명령어로 여러 커맨드를 실행
- RUN
  ```
  RUN [실행하고자 하는 커맨드]
  ```
- Shell 형식으로 실행
  ```
  # httpd 설치
  RUN yum -y install httpd
  ```
- Exec 형식으로 실행
  - Shell 형식의 커맨드는 /bin/sh로 실행시키지만, Exec 형식의 경우 Shell을 거치지 않고 바로 실행시킴
  - 그러므로 커맨드 값에 $ HOME 등의 환경변수를 입력할 수 없음
  - Exec 형식에서는 실행하고자 하는 커맨드를 JSON 형식으로 입력
- Exec 형식으로 RUN 실행
  ```
  # httpd 설치
  RUN ["/bin/bash", "-c", "yum -y install httpd"]
  ```
- Dockerfile로 RUN 명령 실행
  ```
  # 베이스 이미지 설정
  FROM centos:latest

  # 작성자 정보
  MAINTAINER 0.1 your-name@your-domain.com

  # RUN 명령 실행
  RUN echo 안녕 Shell형식
  RUN ["echo", "안녕 Exec형식"]
  RUN ["/bin/bash", "-c", "echo '안녕 Exec 형식으로 bash 사용'"]
  ```
- RUN 명령 실행 로그
  ```
  $ docker build -t sample
  ```
  - 로그를 보면 Step과 출력된 내용을 통해 Dockerfile의 명령어가 한 행씩 실행되었음을 확인 가능
- 이미지 구성
  ```
  $ docker history samle
  ```
  - docker history 커맨드로 이미지를 생성할 때 어떤 커맨드가 실행되었는지를 확인
- 이미지 레이어는 127레이어로 제한
  - 개행을 통한 RUN 명령
    ```
    RUN yum -y install\
                httpd\
                php\
                php-mbstring\
                php-pear
    ```

### CMD(데몬 실행)
- 컨테이너에서 실행되는 커맨드
- Dockerfile에는 한 개의 CMD 명령어만 입력할 수 있으며, 만약 여러 개를 입력한 경우 마지막 커맨드만 적용됨
- CMD
  ```
  CMD [실행하고자 하는 커맨드]
  ```
- CMD 명령을 실행하는 방법 세가지
  - Shell 형식으로 실행
    ```
    # httpd 실행
    CMD /usr/sbin/httpd -D FOREGROUND
    ```
  - Exec 형식으로 실행
    ```
    # httpd 실행
    CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
    ```
  - ENTRYPOINT 명령어의 매개변수로 실행
    ```
    # 베이스 이미지 설정
    FROM centos:latest

    # 작성자 정보
    MAINTAINER 0.1 your-name@your-domain.com

    # httpd 설치
    RUN ["yum", "install", "-y", "httpd"]

    # httpd 실행
    CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
    ```

### ENTRYPOINT(데몬 실행)
- ENTRYPOINT
  ```
  ENTRYPOINT [실행하고자 하는 커맨드]
  ```
- ENTRYPOINT 명령을 실행하는 방법 두가지
  - Shell 형식으로 실행
    ```
    # httpd 실행
    ENTRYPOINT /usr/sbin/httpd -D FOREGROUND
    ```
  - Exec 형식으로 실행
    ```
    # httpd 실행
    ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
    ``` 
- ENTRYPOINT 명령과 CMD 명령의 차이
  - docker run 커맨드 실행 시점에서의 동장
  - CMD 명령은 컨테이너를 구동할 때 정의한 커맨드보다 docker run 커맨드의 값으로 입력된 새로운 커맨드가 우선 실행됨
  - 컨테이너 구동 시 특정 데몬을 강제적으로 실행시키고자 할 때에는 ENTRYPOINT를 사용
- Dockerfile에 ENTRYPOINT 명령이 여러 개 입력되어 있는 경우에는 가장 마지막 명령어가 실행됨

### ONBUILD(build 완료 후에 실행되는 명령)
- 이미지 내의 다음 build에서 실행되는 커맨드를 설정하는 명령
- ONBUILD
  ```
  ONBUILD [실행하고자 하는 커맨드]
  ```


## 환경 및 네트워크 설정

### ENV(환경변수 설정)
- ENV
  ```
  ENV [key] [값]
  ENV [key]=[값]
  ```
- ENV 명령으로 설정한 환경변수는 컨테이너 실행 시 docker run 커맨드의 env 옵션을 사용해 변경할 수 있음

### WORKDIR(작업 디렉터리 설정)
- WORKDIR
  ```
  WORKDIR [작업 디렉터리 경로]
  ```
- Dockerfile에 저장된 다음 명령을 실행하기 위한 작업 디렉터리를 설정할 수 있음
  - RUN, CMD, ENTRYPOINT, COPY, ADD
- WORKDIR은 Dockerfile에서 여러 번 사용할 수 있으며 작업 디렉터리의 경로를 상대 경로로 설정하면 이전 WORKDIR 명령의 경로를 기준으로 한 상대 경로로 설정됨
- ENV로 설정한 환경변수를 사용할 수 있음
  ```
  ENV DIRPATH /first
  ENV DIRNAME second
  WORKDIR $DIRPATH/$DIRNAME
  RUN ["pwd"]
  ```
  - /first/second 가 출력됨

### USER(사용자 설정)
- 이미지 실행 또는 Dockerfile 내의 RUN, CMD, ENTRYPOINT 명령어를 실행시킬 사용자를 설정
- USER
  ```
  USER [사용자명/UID]
  ```
- USER로 사용자를 설정하기 위해서는 RUN 명령에서 미리 사용자를 추가해놓아야 함
- USER 예
  ```
  RUN ["adduser", "asdf"]
  RUN ["whoami"]
  USER asdf
  RUN ["whoami"]
  ```

### LABEL(라벨 설정)
- LABEL
  ```
  LABEL key=값
  ```
- LABEL 예
  ```
  LABEL title="WebAPServerImage"
  LABEL version="1.0"
  LABEL description="This image is WebApplicationServer \
  for JaveEE."
  ```
- Docker 이미지 상세 확인
  ```
  $ docker inspect --format="{{ .Config.Labels }}" sample
  ```

### EXPOSE(포트 설정)
- EXPOSE <포트 번호>
  ```
  EXPOSE <포트 번호>
  $ EXPOSE 8080
  ```


## 파일 시스템 설정

### ADD(파일 및 디렉터리 추가)
- ADD
  ```
  ADD <호스트 파일 경로> <Docker 이미지 파일 경로>
  ADD ["호스트 파일 경로" "Docker 이미지 파일 경로"]
  ```
- ADD 예
  ```
  ADD host.html /docker_dir/
  ```
- WORKDIR과 ADD 예
  ```
  WORKDIR /docker_dir
  ADD host.html web/
  ```
- ADD로 원격 파일 추가
  ```
  ADD http://www.google.com/index.php /docker_dir/web/
  ```
  - 600 퍼미션으로 저장됨
- 만약 디렉터리 대신 파일(마지막에 /가 들어가지 않음)로 파일 시스템을 설정한 경우에는 URL에서 파일을 다운로드하여 입력한 파일명을 추가함
- 파일 시스템이 디렉터리(마지막에 /가 들어감)로 설정된 경우에는 URL로 입력한 파일명으로 추가됨
- 호스트 파일이 tar 압축파일 또는 gzip, bzip2 등의 압축파일인 경우에는 디렉터리로 풀림
- 원격 URL에서 다운로드한 리소스는 압축이 풀리지 않으므로 주의해야 함
- build할 때, 제외하고자 하는 파일이 있다면 <.dockerignore> 파일에 제외할 파일명 입력(.git 파일 등)
  - 제외할 파일 지정
    ```
    $ cat .dockerignore
    Dummyfile
    ```
  - Dockerfile 생성
    ```
    # 베이스 이미지 지정
    FROM centos:latest

    # dummyfile
    ADD dummyfile /tmp/dummyfile
    ```
  - 이미지 build
    ```
    $ docker build -t sample .
    ```

### COPY (파일 복사)
- COPY
  ```
  COPY <호스트의 파일 경로> <Docker 이미지 파일 경로>
  COPY ["호스트의 파일 경로" "Docker 이미지 파일 경로"]
  ```
- Dockerfile 저장 위치
  - Dockerfile은 빈 디렉터리에 넣어두고 이미지를 생성하는 것이 좋음
  - Dockerfile로 이미지를 만들면 docker build 커맨드는 Dockerfile을 포함한 디렉터리(서브 디렉터리 포함)를 모두 Docker 데몬으로 전송함
  - Docker build에 필요 없는 파일은 Dockerfile과 동일한 디렉터리에 두지 않도록 주의

### VOLUME(볼륨 마운트)
- VOLUME
  ```
  VOLUME ["/마운트 포인트"]
  ```
- VOLUME으로 마운트 포인트를 생성하고 호스트와 다른 컨테이너에서 볼륨을 외부 마운트함
- 예제
  - 로그 데이터만 저장하고 있는 컨테이너(log-container)와 웹 서버 기능을 가진 컨테이너(web-container)를 각각 생성하고, 웹 서버 로그를 로그 데이터용 컨테이너에 보존하기 위해 다음과 같은 순서로 Dockerfile, 이미지 및 컨테이너를 생성함
  - 로그용 이미지 생성
    - 로그를 저장하기 위하여 컨테이너의 기반이 되는 이미지를 생성
    - Dockerfile에서 VOLUME으로 로그가 저장될 위치를 지정
    - Apache httpd 로그 관리를 위하여 default 로그 저장소인 /var/log/httpd를 입력
    - 로그용 이미지 생성
      ```
      # Docker 이미지
      FROM centos:latest

      # 작성자 정보
      MAINTAINER 0.1 your-name@your-domain.com

      # 볼륨 생성
      RUN ["mkdir" , "/var/log/httpd"]
      VOLUME /var/log/httpd
      ```
    - 로그용 이미지 build
      ```
      $ docker build -t log-image .
      ```
  - 로그용 컨테이너 구동
    - 앞 단계에서 생성한 로그용 이미지인 'log-image'를 기반으로 다음 커맨드를 통해 컨테이너를 구동
    - name 옵션을 통해 컨테이너명을 'log-container'로 함
    - 로그용 컨테이너 구동
      ```
      $ docker run -it --name log-container log-image
      ```
  - 웹서버용 이미지 생성
    - 웹 서버용 이미지 생성
      ```
      # Docker 이미지
      FROM centos:latest
      # 작성자 정보
      MAINTAINER 0.1 your-name@your-domain.com
      # Apache httpd 설치
      RUN ["yum", "-y", "install", "httpd"]
      # Apache httpd 실행
      CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
      ```
    - 웹 서버용 이미지 build
      ```
      $ docker build -t web-image
      ```
  - 웹 서버용 컨테이너 구동
    - name 옵션으로 컨테이너명을 'web-container'로 하고 호스트 브라우저에서 동작을 확인하기 위하여 호스트와 컨테이너의 포트를 80번으로 설정
    - 웹 서버용 컨테이너 구동
      ```
      $ docker run -d --name web-container \
                -p 80:80 \
                --volumes-from log-container web-image
      ```
    - 동작을 확인하기 위하여 브라우저에서 호스트 Address 80번 포트(http://192.168.99.100/)에 액세스하면 Apache HTTP Server가 구동되는 것을 확인 가능
  - 로그 확인
    - 로그용 컨테이너에 액세스
      ```
      $ docker start -ia log-container
      ```
    - 구동 중인 컨테이너에 액세스하여 /var/log/httpd 디렉터리 안을 확인해보면 log-container에 로그가 수집되고 있는 것을 확인할 수 있음
    - 브라우저를 통해 웹 서버에 액세스해보면 로그 업데이트된 것 확인 가능
  - Docker 플러그인(Logging Driver)를 사용하면 컨테이너 로그를 syslogd와 Treasure Data가 제공하는 오픈소스 로그 수집 툴인 Fluentd에 전송할 수 있음


## Docker 이미지 자동 생성 및 공개
- Automated Build 흐름
  - Docker Hub에는 GitHub, Bitbucket과 연계하여 Dockerfile에서 Docker 이미지를 자동으로 생성하는 'Automated Build' 기능이 있음
- GitHub에 공개
  - 생성한 Dockerfile을 GitHub에 공개
  - repository 생성, Dockerfile 공개
  - 반드시 Dockerfile이라는 이름으로 repository에 공개되어야 함
- Docker Hub 링크 설정
- Dockerfile build
- Docker 이미지 확인