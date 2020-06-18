---
layout: default
title: Docker Installation & Commands
parent: Docker
nav_order: 3
---

# Docker Installation & Commands
{: .no_toc }


Docker의 설치 방법과 기본 커맨드
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## Docker의 설치 방법과 동작 확인

### Windows에서의 설치

### Linux에서의 설치

### Mac OS에서의 설치

### Docker의 'Hello world'
- docker run
  ```
  docker run <Docker 이미지명> <커맨드>
  ```
- Hello world 실행
  ```
  $ docker run ubuntu:latest /bin/echo 'Hello world'
  ```
- 커맨드를 실행하면 Docker 컨테이너의 베이스가 되는 Ubuntu 이미지가 로컬 환경에 있는지 확인
- 만약 로컬 환경에 없다면 Docker repository에서 이미지를 다운로드
- 최초로 Docker 이미지를 다운로드할 때는 시간이 걸리지만 이후에는 로컬 환경에 다운로드된 Docker 이미지를 기반으로 Docker 컨테이너가 실행됨
- 로컬 환경에 다운로드한 Docker 이미지를 로컬 캐시라 함

### Docker Kitematic을 통한 GUI로 Docker 동작 확인
- Docker Kitematic: 로컬 환경에 Docker 환경을 구축하여 컨테이너를 GUI로 관리하는 툴
- Docker Toolbox를 설치하면 Docker kitematic도 설치됨
- Kitematic을 사용하기 위해서는 Docker Hub에 미리 계정을 등록해야 함

## Docker 이미지 실행

### Docker Hub
- Docker 공식 repository
  - GitHub와 Bitbutket 등의 소스 코드 관리 툴과 연계하여 코드를 build하는 기능
  - PaaS 서비스인 AWS Elastic Beanstalk와 Google Compute Engine 등과 연계하여 애플리케이션을 deploy하는 기능
  - 애플리케이션 이미지를 관리하는 기능
- Docker Hub를 통해 물리 서버, 가상 서버, 클라우드까지 애플리케이션을 배포할 수 있음
- 공식 Docker 이미지 외에도 사용자가 독자적으로 생성한 Docker 이미지를 공개할 수 있음
- 공식 Docker 이미지는 'Official'을 선택하여 확인
- Repo Info 탭: 버전 정보, 주의 사항, 지원하는 Docker 버전 등을 기록한 Docker 이미지의 상세한 내용 확인
- Tags 탭: 베이스가 되는 OS의 버전이 기록된 태그 정보
- Docker 이미지 지정
  ```
  이미지명[:태그명]
  centos:7
  ```

### docker pull(이미지 다운로드)
- docker pull
  ```
  docker pull [옵션] <이미지명>[:태그명]
  ```
- CentOS Docker 이미지 다운로드
  ```
  $ docker pull centos:7
  ```
- CentOS 모든 태그의 Docker 이미지 다운로드
  ```
  $ docker pull -a centos
  ```
- 다운로드 URL을 지정하여 CentOS Docker 이미지를 다운로드
  ```
  $ docker pull registry.hub.docker.com/centos:7
  ```

### docker images(이미지 목록 출력)
- docker images
  ```
  docker images [옵션] [repository명]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-a`, `--all=false`|모든 이미지 표시|
    |`--digests=false`|digets표시|
    |`-a`, `--no-trunc=false`|모든 결과 표시|
    |`-q`, `--quiet=false`|Docker 이미지 ID만 표시|
- Docker repository에 업로드한 이미지는 한눈에 식별하기 쉽도록 digest가 부여됨
- digest 표시
  ```
  $ docker images --digests centos:7
  ```
- DCT(Docker Content Trust) 기능을 사용하여 Docker 이미지의 진위 여부를 확인할 수 있음
  - DCT 기능 enable
    ```
    $ export DOCKER_CONTENT_TRUST=1
    ```
  - DCT 기능 disable
    ```
    $ export DOCKER_CONTENT_TRUST=0
    ```
  - enable한 뒤, `docker pull` 커맨드를 사용하여 이미지를 다운로드하면 이미지를 검증함

### docker inspect(이미지 세부 정보 확인)
- docker inspect
  ```
  docker inspect [옵션] <컨테이너 또는 이미지의 이름, ID>
  ```
- docker inspect 커맨드로 세부 정보 출력
  ```
  $ docker inspect centos
  ```
  - 커맨드 실행 결과는 JSON 형식으로 표시됨
- OS 상세 정보 확인
  ```
  $ docker inspect --format="{{ .Os}}" centos
  ```
- image 상세 정보 확인
  ```
  $ docker inspect --format="{{ .ContainerConfig.Image }}" centos
  ```

### docker tag(이미지 태그 설정)
- Docker Hub에 생성된 이미지를 등록할 때는 다음의 규칙에 맞게 이미지명을 붙임
  ```
  <Docker Hub 사용자명>/이미지명:/[태그명]
  ```
- 이미지 태그에는 일반적으로 버전명을 설정
  ```
  $ dockder tag httpd:2.4 asdf/webserver:1.0
  ```
  - 대상 이미지: httpd:2.4
  - 사용자명: asdf
  - 이미지명: webserver
  - 태그: 1.0
- 태그를 설정한 이미지와 본래 이미지의 [IMAGE ID]는 같다.
  - 각 이미지의 이름은 다르나 이미지를 rename한 것 뿐임

### docker search(이미지 검색)
- docker search
  ```
  docker search [옵션] <검색 키워드>
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`--automated=false`|Automated Build만 표시|
    |`--no-trunc=false`|모든 결과 표시|
    |`--filter=stars=0`|특정 개수 이상의 별 수|
- Docker Hub에 공개된 Docker 이미지 검색
  ```
  $ docker search centos
  ```
- 인기 있는 Docker 이미지 검색
  ```
  $ docker search --filter=stars=3 centos
  ```

### docker rmi(이미지 삭제)
- docker rmi
  ```
  docker rmi [옵션] <이미지명>
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-f`, `--force=false`|이미지 강제 삭제|
    |`--no-prune=false`|태그가 없는 부모 이미지를 삭제하지 않음|
- Docker 이미지 삭제
  ```
  $ docker rmi nginx
  ```

### docker login(Docker Hub에 로그인)
- docker login
  ```
  docker login [옵션] [서버명]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-u`, `--username=""`|사용자명|
    |`-p`, `--password=""`|패스워드|
    |`-e`, `--email=""`|이메일 주소|
- Docker Hub에 로그인
  ```
  $ docker login
  Username: 등록한 사용자명
  Password: 등록한 패스워드
  Email: 등록한 이메일 주소
  Login Succeded
  ```

### docker logout(Docker Hub에서 로그아웃)
- docker logout
  ```
  docker logout [서버명]
  ```
- 서버명을 입력하지 않으면 Docker Hub에 액세스
- 로컬 환경에 Docker repository가 있는 경우에는 서버명을 입력

### docker push(이미지 업로드)
- docker push
  ```
  docker push <이미지명>[:태그명]
  ```
- Docker Hub에 업로드하는 이미지명은
  ```
  <Docker Hub 사용자명>/이미지명[:태그명]
  ```
- docker push 커맨드로 업로드
  ```
  $ docker push asdf/webserver:1.0
  ```


## Docker 컨테이너 생성, 구동, 중지

### Docker 컨테이너 라이프 사이클

#### docker create(컨테이너 생성)
- 이미지로 컨테이너 생성
- docker create 커맨드를 실행하면 이미지에 포함된 Linux 디렉터리 및 파일 집합의 스냅샷을 만듬
- 스냅샷: 특정 시간에 스토리지 내부에 존재하는 파일과 디렉터리를 저장한 것
- docker create 커맨드는 컨테이너를 생성하는 것 뿐, 컨테이너를 구동하지는 않음
- 컨테이너를 구동할 수 있는 준비 상태를 만드는 것

#### docker run(컨테이너 생성 및 구동)
- 이미지에서 컨테이너를 생성하여 컨테이너상에서 프로세스를 구동

#### docker start(컨테이너 구동)
- 중지 상태인 컨테이너를 구동

#### docker stop(컨테이너 중지)
- 구동 중인 컨테이너를 중지
- 컨테이너를 삭제할 때는 docker stop 커맨드로 중지시킨 후 삭제

#### docker rm(컨테이너 삭제)
- 중지되어 있는 컨테이너를 삭제

#### docker ps(컨테이너 상태 확인)
- 컨테이너의 상태를 확인

### docker run(컨테이너 생성 및 실행)
- docker run
  ```
  docker run [옵션] <이미지명>[:태그명] [값]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-a`, `--attach=[STDIN | STDOUT | STDERR]`|표준 입력(STDOUT), 표준 출력(STDOUT), 표준 에러 출력(STDERR)을 연결|
    |`--cidfile="파일명"`|컨테이너 ID를 파일로 출력|
    |`-d`, `--detach="false"`|컨테이너를 생성하여 백그라운드에서 실행|
    |`-i`, `--interactive="false"`|컨테이너 표준 입력 열기|
    |`-t`, `--tty="false"`|tty(단말 디바이스)를 사용|
    |`--name`|컨테이너명|

- `/bin/cal` 커맨드를 실행한 캘린더 표시
  ```
  $ docker run -it --name "test1" centos /bin/cal
  ```
- `/bin/bash` 실행
  ```
  $ docker run -it --name "test2" centos /bin/bash
  ```

### docker run(컨테이너 백그라운드 실행)
- docker run
  ```
  docker run [옵션] <이미지명>[:태그명] [값]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-d`, `--detach`|백그라운드에서 실행|
    |`-u`, `--user="사용자명"`|사용자명을 입력|
    |`--restart=[no | on-failure | on-failure:횟수n | always]`|커맨드 실행 결과에 따라 재기동|
    |`--rm`|커맨드 실행 완료 후 컨테이너 자동 삭제|
- docker run으로 백그라운드 실행
  ```
  docker run -d contos /bin/ping localhost
  ```
- /bin/ping 커맨드 실행으로 컨테이너 백그라운드 구동
  ```
  docker run -d contos /bin/ping localhost
  ```
- 백그라운드에서 실행되고 있는지의 여부를 확인하기 위해서는 docker logs 커맨드를 사용
- 컨테이너 로그 확인
  ```
  $ docker logs -t 4bd7e5f77
  ```
- 실행이 완료된 뒤 컨테이너를 자동으로 삭제하고자 할 때에는 rm 옵션을 사용
- 커맨드 실행 결과에 따라 컨테이너를 재구동시키고자 할 때에는 restart 옵션을 사용
- restart 옵션
  - |값|설명|
    |:--|:--|
    |`no`|재구동하지 않음|
    |`on-failure`|종료 status가 0이 아닌 경우 재구동|
    |`on-failure:횟수n`|종료 status가 0이 아닌 경우 n번 재구동|
    |`always`|항상 재구동|
- 항상 재구동
  ```
  $ docker run -it --restart=always centos /bin/bash
  ```
  - /bin/bash가 종료되어도 컨테이너를 재구동 함

### docker run(컨테이너 네트워크 설정)
- docker run
  ```
  docker run [옵션] <이미지명>[:태그명] [값]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`--add-host=[호스트명:IP Address]`|컨테이너의 /etc/hosts에 호스트명과 IP Address를 설정|
    |`--dns=[IP Address]`|DNS 서버의 IP Address를 설정|
    |`--expose=[포트 번호]`|포트 번호 할당|
    |`--mac-address=[MAC Address]`|컨테이너의 MAC Address 설정|
    |`--net=[bridge | none | container:<name|id> | host]`|컨테이너의 네트워크 설정|
    |`-h`,`--hostname="호스트명"`|컨테이너의 호스트명 설정|
    |`-P`,`--publish-all=[true | false]`|임의의 포트를 컨테이너에 할당|
    |`-p [호스트 포트 번호]:[컨테이너 포트 번호]`|호스트와 컨테이너의 포트를 매핑|
    |`--link=[컨테이너명:alias]`|다른 컨테이너에서 액세스 시 이름 설정|
- 컨테이너 포트 매핑
  ```
  $ docker run -d -p 8080:80 httpd
  ```
- 컨테이너 DNS 서버
  ```
  $ docker run --dns=192.168.1.1 httpd
  ```
- MAC Address 설정
  ```
  $ docker run -it --mac-address="92:d0:c6:0a:29:33" centos
  ```
- docker run 커맨드로 호스트명과 IP Address 설정
  ```
  $ docker run -id -add-host=test.com:192.168.1.1 centos
  ```
- 호스트명 설정
  ```
  $ docker run -it --hostname=www.test.com --add-host=node1.test.com:192.168.1.1 centos
  ```
- net 옵션
  - |값|설명|
    |:--|:--|
    |`bridge`|bridge 접속(default) 사용|
    |`none`|네트워크에 접속하지 않음|
    |`container:[name | id]`|다른 컨테이너의 네트워크를 사용|
    |`host`|컨테이너가 호스트OS의 네트워크를 사용|

### docker run(리소스를 설정하여 컨테이너 생성 및 실행)
- docker run
  ```
  docker run [리소스 옵션] <이미지명>[:태그명] [값]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-c`, `--cpu-shares=0`|CPU 리소스 분배(default=1024)|
    |`-m`, `--memory=[메모리 사용량]`|메모리 사용량 제한(단위는 b, k, m, g 등)|
    |`-v`, `--volume=[호스트 디렉터리]:[컨테이너 디렉터리]`|호스트와 컨테이너의 디렉터리 공유|
- CPU와 메모리 사용량 설정
  ```
  $ docker run --cpu-share=512 --memory=512m centos
  ```
  - Docker 리소스를 제한하는 기능은 Linuxdml cgroups 기능을 사용
- 디렉터리 공유
  ```
  $ docker run -v ~/asdf/webpage:/var/www/html httpd
  ```

### docker run(컨테이너 생성 및 구동 환경 설정)
- docker run
  ```
  docker run [환경설정 옵션] <이미지명>[:태그명] [값]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-e`, `--env=[환경변수]`|환경변수 설정|
    |`--env-file=[파일명]`|파일에서 환경변수 설정|
    |`--privileged=[true | false`|privileged 모드에서 구동(호스트의 커널 기능도 사용 가능)|
    |`--read-only=[true | false`|컨테이너의 파일 시스템을 read-only로 설정|
    |`-w`, `--workdir=[경로]`|컨테이너의 작업 디렉터리를 설정|
- 환경변수 설정
  ```
  $ docker run -it -e foo=bar centos /ban/bash
  ```
- 환경변수 일괄 설정
  ```
  $ cat env_list
  foo1=bar1
  foo2=bar2
  $ docker run -it --env-file=env_list centos /bin/bash
  ```
- 작업 디렉터리 설정
  ```
  $ docker run -it -w=/tmp/work centos /bin/bash
  ```

### docker ps(컨테이너 목록 확인)
- docker ps
  ```
  docker ps [옵션]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-a`, `--all=false`|구동, 중지 상태의 모든 컨테이너를 표시|
    |`--before=""`|입력한 컨테이너명 또는 ID보다 이전에 구동된 컨테이너를 표시|
    |`-f`, `--filter '[key]=[value]'`|목록에 표시할 컨테이너를 필터링|
    |`--format '[key]=[value]'`|목록에 표시할 포맷을 설정|
    |`-l`, `--latest=false`|마지막에 구동된 컨테이너를 표시|
    |`--no-trunc=false`|생략된 정보 없이 모두 표시|
    |`-q`, `--quiet=false`|컨테이너 ID만 표시|
    |`-s`, `--size=false`|파일 사이즈를 표시|
    |`--since=""`|입력한 컨테이너명 또는 ID보다 이후에 구동된 컨테이너를 표시|
- docker ps 커맨드 실행
  ```
  $ docker ps -a
  ```
- docker ps 커맨드에서 필터링 사용
  ```
  $ docker ps -a -f 'name=test1'
  ```
- STATUS의 종료 코드가 0인 목록
  ```
  $ docker ps -a -f 'exited=0'
  ```
- docker ps 커맨드의 출력 형식 지정
  ```
  $ docker ps -a --format "{{.ID}}: {{.Status}}"
  ```
- docker ps 커맨드를 표 형식으로 실행
  ```
  $ docker ps -a --format "table {{.ID}}\t{{.Status}}"
  ```

### docker stats(컨테이너 구동 확인)
- docker stats
  ```
  docker stats <컨테이너명 또는 ID>
  ```
- docker stats 실행
  ```
  $ docker stats apache nginx
  ```

### docker start(컨테이너 구동)
- docker start
  ```
  $ docker start [옵션]<컨테이너명 또는 ID>
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-a`, `--attach=false`|표준 출력, 표준 에러를 연결|
    |`-i`, `--interactive=false`|컨테이너 표준 입력을 연결|
- docker start
  ```
  $ docker start dbd4bbe0f470
  ```

### docker stop(컨테이너 중지)
- docker stop
  ```
  docker stop [옵션] <컨테이너명 또는 ID>
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-t`, `--time=10`|컨테이너 중지 시간을 지정(default는 10초)|
- docker stop 실행
  ```
  $ docker stop -t 2 dbd4bbe0f470
  ```

### docker restart(컨테이너 재시작)
- docker restart
  ```
  docker restart <옵션> <컨테이너명 또는 ID>
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-t`, `--time=10`|컨테이너 재시작 시간을 지정(default는 10초)|
- docker restart 실행
  ```
  $ docker restart -t 2 dbd4bbe0f470
  ```
- 컨테이너에서 실행되는 커맨드의 종료 상태(정상 종료인지 아닌지)에 따라 컨테이너를 자동으로 재시작하고자 하는 경우, `dockder run` 커맨드의 `--restart` 옵션을 사용

### docker rm(컨테이너 삭제)
- docker rm
  ```
  docker rm [옵션] <컨테이너명 또는 ID>
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-f`, `--force=false`|구동 중인 컨테이너를 강제 삭제|
    |`-v`, `--volumes=false`|할당된 볼륨을 삭제|
- docker rm 실행
  ```
  $ docker rm dbd4bbe0f470
  ```
- docker rm 커맨드로 일괄 삭제
  ```
  $ docker rm -f 'docker ps -a -q'
  ```

### docker pause/docker unpause(컨테이너 일시정지 및 재시작)
- docker pause
  ```
  docker pause <컨테이너명 또는 ID>
  ```
- docker pause 커맨드 실행
  ```
  $ docker pause nginx1
  ```
  - `docker ps`로 확인
- docker unpause로 일시정지된 프로세스 재시작
  ```
  $ docker unpause nginx1
  ```

### docker kill (컨테이너 강제 중지)
- docker kill
  ```
  docker kill [옵션] <컨테이너명 또는 ID>
  ```


## Docker 컨테이너 사용법

### docker attach(구동 중인 컨테이너 접속)
- docker attach 커맨드 실행
  ```
  $ docker attach test
  ```
  - 접속한 후
    - 컨테이너 종료: `ctrl+c`
    - 컨테이너 종료하지 않고 프로세스를 빠져나올 때: `ctrl+p`, `ctrl+q`

### docker exec(구동 중인 컨테이너의 프로세스 실행)
- docker exec
  ```
  $ docker exec [옵션] <컨테이너명 또는 ID> <커맨드> [값]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-d`, `--detach=false`|커맨드를 백그라운드에서 실행|
    |`-i`, `--interactive=false`|컨테이너 표준 입력 열기|
    |`-t`, `--tty=false`|tty(단말디바이스) 사용|
- docker exec 커맨드 실행
  ```
  $ docker exec -it nginx1 /bin/bash
  $ docker exec -it nginx1 /bin/ping localhost
  ```
- 중지된 컨테이너인 경우에는 docker start 사용

### docker top(구동 중인 컨테이너에서 실행 중인 프로세스 확인)
- docker top 커맨드 실행
  ```
  $ docker top nginx1
  ```

### docker port(구동 중인 컨테이너의 포트 상태 확인)
- docker port 커맨드 실행
  ```
  $ docker port nginx1
  ```
### docker rename(컨테이너명 변경)
- docker rename 커맨드 실행
  ```
  $ docker rename old new
  ```

### docker cp(컨테이너 내에서 파일 복사)
- docker cp
  ```
  docker cp <컨테이너명 또는 ID> : <컨테이너 내의 파일 경로> <호스트 디렉터리 경로>
  docker cp <호스트 파일> <컨테이너명 또는 ID> : <컨테이너 내의 파일 경로>
  ```
- 컨테이너에서 호스트로 파일 복사
  ```
  $ docker cp test:/etc/passwd /tmp/etc
  $ ls -al /tmp/etc
  ```
- 호스트에서 컨테이너로 파일 복사
  ```
  $ docker cp ./local.txt test:/tmp/test.txt
  ```

### docker diff(컨테이너 내에서 파일 변경 이력 확인)
- docker diff
  ```
  docker diff <컨테이너명 또는 ID>
  ```
- 변경 이력 구분
  - |구분|설명|
    |:--|:--|
    |A|파일 추가|
    |D|파일 삭제|
    |C|파일 변경|
- 사용자 추가
  ```
  # useradd newuser
  # exit
  ```
- docker diff 커맨드 실행
  ```
  $ docker diff test
  ```


## Docker 정보 확인

### docker version(Docker 버전 확인)
- docker version
  ```
  docker version
  ```
- docker version 커맨드 실행
  ```
  $ docker version
  ```

### docker info(Docker 실행 환경 확인)
- docker info 커맨드 실행
  ```
  $ docker info
  ```
- 구동 중인 컨테이너 수, 스토리지 드라이브 종류, Boot2Docker 버전 등
- 


## 컨테이너에서 이미지 생성

### docker commit(컨테이너에서 이미지 생성)
- docker commit
  ```
  docker commit [옵션] <컨테이너명 또는 ID> [이미지명[:태그명]]
  ```
- 주요 옵션
  - |옵션|설명|
    |:--|:--|
    |`-a`, `--author="~ "`|생성자|
    |`-m`, `--message="~ "`|메시지|
    |`-p`, `--pause=true`|컨테이너를 일시 중지한 후 commit|
- docker commit 커맨드 실행
  ```
  $ docker commit -a "ASDF" nginx1 asdf/webfront:1.0
  ```
- 이미지 상세 정보 확인
  ```
  $ docker inspect asdf/webfront:1.0
  ```

### docker export(컨테이너를 tar파일로 저장)
- docker export
  ```
  docker export <컨테이너명 또는 ID>
  ```
- docker export 커맨드 실행
  ```
  $ docker export webap > latest.tar
  ```
- 생성된 tar 파일 상세 확인
  ```
  $ tar -tvf latest.tar
  ```

### docker import(tar 파일에서 이미지 생성)
- docker import
  ```
  $ docker import <파일 또는 URL> - [이미지명[:태그명]]
  ```
- docker import 커맨드에서 지정 가능한 파일은 하나뿐이므로 `tar` 커맨드 등으로 디렉터리, 파일 등을 한데 모아야 함
- root 권한으로 실행하지 않으면 액세스 권한이 없는 파일이 포함되지 않는 등 문제가 발생할 수 있음
- docker import zjaosemdptj wlwjdgkf tn dlTsms vkdlf
  - tar
  - tar.gz
  - tgz
  - bzip
  - tar.xz
  - txz
- docker export 커맨드 실행
  ```
  $ cat latest.tar | docker import - webap:1.0
  ```
- 이미지 확인
  ```
  $ docker images
  ```

### docker save(이미지 저장)
- docker save
  ```
  docker save [옵션] <파일명> [이미지명]
  ```
- docker save 커맨드 실행
  ```
  $ docker save -o export.tar mongo
  ```

### docker load(이미지로 되돌리기)
- docker load
  ```
  $ docker load [옵션]
  ```
- docker load 실행
  ```
  $ docker load -i export.tar
  ```
- export/import와 save/load의 차이
  - export/import
    - 컨테이너를 export하면 컨테이너 동작에 필요한 파일이 모두 압축됨
    - 이 tar 파일에는 컨테이너의 루트 파일 시스템 자체가 들어있다고 할 수 있음
  - save/load
    - 이미지를 save하면 레이어 구조까지 포함한 형태로 압축됨
- 기반 이미지는 같아도 docker export와 docker save 커맨드의 내부 디렉터리 및 파일 구조가 다음
- 따라서 docker export로 압축된 파일은 docker import로, docker save로 압축된 파일은 docker load를 사용하여 이미지를 생성해야 함
