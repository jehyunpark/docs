---
layout: default
title: Docker Overview
parent: Docker
nav_order: 2
---

# Docker Overview
{: .no_toc }


Docker Overview
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## Docker의 특징
- 컨테이너 가상화 환경에서 애플리케이션을 관리하고 실행하기 위한 오픈소스 플랫폼
- Linux 위에서 동작
- Go 언어로 만들어짐
- 2013년부터 Docker사에서 개발하기 시작
- 온프레미스 환경에서도 동작
- 동작하는 클라우드 환경
  - Amazon EC2
  - Google Cloud Platform
  - IBM SoftLayer
  - Rackspace Cloud

### 이식성
- 시스템 개발 시, 운영 환경에서 업무 애플리케이션을 구동하기 위해 필수적인 요소
  - 업무 애플리케이션 실행 모듈(프로그램 본체)
  - 미들웨어와 라이브러리
  - OS 및 네트워크 등 인프라 환경 설정
- Docker에서는 인프라 환경을 컨테이너로 관리
  - 애플리케이션 실행에 필수적인 모든 파일 및 디렉터리를 통째로 컨테이너에 담는 것
  - 이 컨테이너의 기반이 되는 Docker 이미지를 Docker Hub에서 공유
- Docker 이미지 생성
  - 자신이 개발한 웹 애플리케이션 실행에 필요한 모든 것을 포함한 Docker 이미지를 생성
  - 이 이미지는 컨테이너의 템플릿이 되며 이를 기반으로 컨테이너를 동작시킬 수 있음
  - 이미지는 Docker를 설치할 수 있는 환경이라면 어디에서든 동작할 수 있기 때문에 '개발 및 테스트 환경에서는 동작하지만 운영 환경에서는 동작하지 않는' 리스크를 줄일 수 있음
- 이식성(portability)
  - 한번 만들어두면 어디에서든 동작하는 소프트웨어의 특성
  - 개발한 애플리케이션을 온프레미스 환경에 마이그레이션하는 것뿐만 아니라 온프레미스 환경 -> 클라우드, 클라우드 -> 클라우드 사이 등 시스템 요구 사항과 예산에 맞게 실행 환경을 선택할 수 있음

### 상호운용성
- 상호운용성(interoperability)
  - 여러 조직이나 시스템과 연계하여 사용할 수 있는 소프트웨어의 특성
  - AWS EC2를 통해 Docker 실행 환경 운영 서비스를 제공
  - Jenkins와 연계하면 테스트를 자동화할 수 있음
  - GitHub와 연계하여 GitHub상에서 관리되는 Dockerfile을 Docker Hub와 연계한 뒤 자동으로 빌드하면, Docker 컨테이너의 기반이 되는 Docker 이미지를 생성할 수 있음
  - Google은 컨테이너 통합 관리를 위한 Kubernetes 프레임워크를 오픈소스로 공개

### Docker 전용 Linux 배포판
- Docker 전용 Linux 배포판의 특징은 '경량'과 '고속'
- 대표적인 Docker 전용 Linux 배포판
  - RHEL Atomic Host(Red Hat Enterprise Linux Atomic Host)
  - Project Atomic
  - Snappy Ubuntu Core
  - Core OS


## Docker의 기본 기능

### Docker 이미지 생성
- 애플리케이션 실행에 필요한 프로그램, 라이브러리, 미들웨어와 OS, 네트워크 설정 등을 하나로 모아 'Docker 이미지'를 생성
- Docker 이미지
  - 실행 환경에서 동작하는 컨테이너의 기반
  - 애플리케이션 실행에 필요한 파일이 담긴 디렉터리
  - Docker 커맨드를 사용하면 이미지를 tar 파일로 만들 수 있음
  - Docker 이미지는 Docker 커맨드를 사용하여 수동으로 생성 가능
  - Dockerfile을 통해 자동으로도 생성 가능
- 중첩 사용 가능
  - OS용 이미지에 웹 애플리케이션용 이미지를 겹쳐 새로운 이미지를 생성 가능
  - Docker에서는 구성 변경이 일어난 부분을 따로 관리

### Docker 컨테이너 동작
- Docker는 Linux상에서 컨테이너 단위로 서버를 구동시킴
- 이미 동작하고 있는 OS상에서 프로세스를 실행시키는 것과 거의 비슷한 속도로 빠르게 기동
- 하나의 Linux 커널을 여러 컨테이너가 공유
- 컨테이너 내에서 동작하는 프로세스를 하나의 그룹으로 관리하고 그룹별로 각각 다른 파일 시스템과 호스트명, 네트워크 등이 할당
- 서로 다른 그룹인 경우, 프로세스와 파일에 액세스 할 수 없음
- 컨테이너를 독립된 공간으로 관리하며 이를 실현하기 위하여 Linux 커널 기능(namespace, cgroups 등)이 사용됨

### Docker 이미지 공개 및 공유
- Docker 레지스트리에서 통합적으로 관리할 수 있음
- 공식 이미지 외에도 개인이 개발한 이미지를 Docker Hub에서 자유롭게 공개하여 모두가 공유할 수 있음
- Docker 커맨드를 통해 Docker Hub에 로그인하여 레지스트리상에서 이미지를 검색하고 업로드 및 다운로드 할 수 있음
- Automated Build: GitHub 위에서 Dockerfile을 관리하고 Docker 이미지를 자동으로 생성하여 Docker Hub에 공개

### Docker 컴포넌트
- 코어 기능을 제공하는 'Docker Engine'을 중심으로, 이미지를 생성 -> 공개 -> 컨테이너 실행 을 위한 여러가지 컴포넌트 제공
- 주요 컴포넌트
  - Docker Engine
  - Docker Registry
  - Docker Compose
  - Docker Swarm
  - Docker Kitematic
  - Docker Machine
- Docker Toolbox 제공
  - Docker Client, Docker Machine, Docker Compose, Docker Kitematic, VirtualBox 셋업

#### Docker Engine(Docker의 코어 기능)
- Docker 이미지 생성과 컴포넌트 구동 등을 위한 Docker의 코어 기능
- Docker 커맨드 실행 및 Dockerfile을 통한 이미지 생성 등을 수행

#### Docker Registry(이미지 공개 및 공유)
- 컨테이너의 기반이 되는 Docker 이미지를 공개 및 공유하기 위한 레지스트리 기능
- Docker Hub도 Docker Registry를 사용

#### Docker Compose(여러 컨테이너를 통합 관리)
- 여러 컨테이너의 구성 정보를 코드로 정의하고 커맨드를 통해 애플리케이션 실행 환경을 구성하는 컨테이너 통합 관리 툴

#### Docker Swarm(클러스터 관리)
- 여러 Docker 호스트를 클러스터화하기 위한 툴
- Manager는 클러스터 관리와 API를 제공하며 Node는 Docker 컨테이너를 실행

#### Docker Kitematic(Docker의 GUI 툴)
- Docker 이미지 생성과 컴포넌트 구동 등을 위한 Docker의 GUI 툴
- 그래픽된 UI를 통해 컨테이너를 관리

#### Docker Machine(Docker 실행 환경 구축)
- 클라우드 환경에 Docker 실행 환경을 커맨드로 자동 생성하기 위한 툴


## Docker의 동작 구조

### 컨테이너를 구분하는 구조(namespace)
- Docker는 컨테이너라는 독립된 환경을 만들고 이를 나누어 애플리케이션 실행 환경을 만드는데 이처럼 컨테이너를 나눌 때 Linux 커널의 namespace 기능을 사용
- Linux 커널의 namespace 기능은 Linux 오브젝트에 이름 붙이는 것을 통하여 다음에서 설명하는 6가지 독립된 환경을 구축할 수 있음

#### PID namespace
- PID는 Linux에서 각 프로세스에 할당된 고유한 ID를 의미
- PID namespace는 PID와 프로세스를 분리함
- namespace가 서로 다른 프로세스는 서로 액세스할 수 없음

#### Network namespace
- 네트워크 디바이스, IP Address, 포트 번호, 라우팅 테이블, 필터링 테이블 등 네트워크 리소스를 namespace 별로 할당할 수 있음
- 이 기능을 통해 호스트OS 위에서 사용 중인 포트가 있어도 컨테이너 안에서 같은 번호의 포트를 사용할 수 있음

#### UID namespace
- UID(user ID), GID(Group ID)를 namespace 별로 독립하여 가질 수 있음

#### MOUNT namespace
- 마운트란 컴퓨터에 접속한 기기와 기억장치를 OS에 인식시키는 것
- MOUNT namespace는 마운트를 조작하여 namespace 내에 파일 시스템 트리를 만듬
- namespace 내의 마운트는 호스트OS와 다른 namespace에서 액세스할 수 없음

#### UTS namespace
- namespace별로 호스트명과 도메인명을 독자적으로 가질 수 있음

#### IPS namespace
- 프로세스 간의 통신(IPS) 오브젝트를 namespace별로 가질 수 있음
- IPC: 공유 메모리와 세마포어 및 메세지 큐의 System V 프로세스 간 통신 오브젝트
- 세마포어: 프로세스에 필요한 자원 관리에 이용되는 배타 제어 방식
- 메세지 큐: 여러 프로세스 사이에서 비동기 통신이 이루어질 때 사용하는 큐잉 방식

### 리소스 관리 구조(cgroup)
- Docker는 여러 컨테이너에서 물리 머신의 리소스를 공유하여 사용함
- Linux 커널 기능인 cgroup을 이용하여 리소스 할당 등의 관리를 수행
- Linux에서는 프로그램을 '프로세스'로 실행하며 이는 하나 이상의 thread 단위로 동작함
- cgroup은 프로세스 및 thread를 그룹화하여 이를 관리하는 기능. 이를 통해 호스트OS의 CPU, 메모리와 같은 리소스를 그룹별로 제한할 수 있음
- cgroup의 주요 서브시스템
  - cpu: CPU 사용량 제한
  - cpuacct: CPU 사용량 통계 정보 제공
  - cpuset: CPU와 메모리 배치 제어
  - memory: 메모리와 스와프 사용량 제한
  - devices: 디바이스 액세스 허가 및 제한
  - freezer: 그룹에 속한 프로세스 정지 및 재개
  - net_cls: 네트워크 제어 태그 추가
  - blkio: 블록 디바이스 입출력량 제어

### 네트워크 구성(가상 bridge 및 가상 NIC)
- Docker의 컨테이너는 서버의 물리 NIC와 별도로 각 컨테이너마다 가상 NIC가 할당되어 있음
- 가상 NIC는 docker0이라는 가상 bridge에 접속하여 컨테이너끼리 통신
- docker0은 Docker 데몬을 기동한 후에 생성되며 172.17.42.1 주소가 할당됨
- Docker에서 컨테이너를 구동하면 컨테이너에 172.17.0.0/16 subnet mask를 가진 private IP Address가 eth0에 자동으로 할당됨
- eth0에는 호스트OS에 생성된 가상 NIC(vethxxx)가 페어로 할당
- veth는 OSI 7 계층 Layer 2의 가상 네트워크 인터페이스로서 페어된 NIC끼리 터널링 통신을 수행

### Docker 컨테이너 간의 통신
- 동일한 호스트상의 Docker 컨테이너는 구동 시 private Address가 자동으로 할당되므로 컨테이너끼리 통신하기 위하여 '링크 기능'을 사용함
- 이를 사용하면 한 호스트 상에 여러 컨테이너가 동작하는 경우, 컨테이너의 alias명을 통해 서로 다른 컨테이너에 접속할 수 있음
- 컨테이너를 구동할 때, 통신하려는 컨테이너 이름을 지정하여 링크하면 해당 정보가 /etc/host에 환경변수로 저장되어 직접 통싱 가능
- Docker는 일반적으로 1컨테이너 1프로세스로 운영됨
- 웹 서보용 컨테이너와 DB 서버용 컨테이너에서 데이터를 연계하거나 각종 서버용 컨테이너의 로그 파일을 로그용 컨테이너에 출력하는 방식으로 사용 가능
- 링크 기능을 사용한 통신은 동일 호스트, 즉 가상 bridge docker0에 접속한 컨테이너끼리만 가능
- 멀티 호스트 환경에서는 링크 기능을 이용한 통신이 불가능하므로 주의

### Docker 컨테이너와 외부 네트워크 통신
- Docker 컨테이너가 외부 네트워크와 통신할 때에는 가상 bridge docker0와 호스트OS의 물리 NIC에서 패킷을 전송해야 함. 이 때, Docker에서는 NATP 기능을 사용하여 접속
- NAPT(Network Address Port Translation)
  - 하나의 IP Address를 여러 컴퓨터에서 공유하는 기술로서 IP Address와 포트 번호를 변환하는 기능
  - private IP Address와 global IP Address를 상호 변환하는 기술로, TCP 및 UDP 포트 번호까지 동적으로 변환하기 때문에 여러 머신에서 한 global IP Address로 접속할 수 있음
- Docker에서는 Linux의 iptables를 NAPT에 사용
- 컨테이너를 구동할 때, 내부에서 사용하고 있는 포트를 가상 bridge docker0에 개방할 때 사용
  - 컨테이너 구동 시, 컨테이너 내의 웹 서버(http 데몬)가 사용하는 80번 포트를 호스트OS의 8080번 포트로 전송하도록 설정하였을 때, 외부 네트워크에서 호스트OS의 8080번 포트에 액세스하면 컨테이너 내의 80번 포트로 연결되는 구조
- NAT와 NATP의 차이
  - private IP address와 global IP Address를 변환하여 private IP Address가 할당된 컴퓨터가 인터넷에 접속할 수 있도록 하는 기술에 NAT와 NAPT(IP masquerade)가 있음
  - NAT
    - private IP Address가 할당된 클라이언트가 인터넷 상의 어떤 서버에 액세스할 때에 NAT라우터는 클라이언트의 private IP Address(192.168.0.1)를 NAT가 갖고 있는 global IP Address(54.xxx.xxx.xxx)로 변환하여 request를 전송
    - 반대의 경우에는 서버에서 NAT의 response를 global IP Address로 전송하고 이를 받은 NAT 라우터는 클라이언트의 private IP Address로 변환하여 전송
    - 이를 통해 private 네트워크상의 컴퓨터와 인터넷상의 서버가 통신할 수 있게 됨
    - 단, NAT는 global IP Address와 private IP Address를 1:1로 변환하기 때문에 동시에 여러 클라이언트가 액세스할 수 없음
  - NAPT(Network Address Port Translation)
    - NAPT는 private IP Address와 포트 번호까지 변환하는 기술
    - private IP Address를 global IP Address로 변환할 때, private IP Address별로 다른 포트 번호를 변환함
    - 클라이언트 A에서는 포트 번호 1500, 클라이언트 B에서는 포트 번호 1600 request 하는 경우
      - 인터넷상의 서버에서는 NAPT의 global IP Address의 서로 다른 포트 번호로 response를 보냄
      - NAPT는 이 포트 번호를 토대로 private IP Address로 변환할 수 있음
      - 이처럼 하나의 global IP Address에 여러 private IP Address를 변환할 수 있음

### Docker 이미지의 데이터 관리 구조
- Docker에서는 Copy on Write 방식으로 컨테이너 이미지의 변경을 관리
  - Copy on Write: 데이터를 바로 복사하지 않고 원본을 그대로 참조하여 원본 또는 복사본 데이터 중 하나가 변경될 때 빈 공간을 확보하여 데이터를 복사하는 구조
- Docker 이미지는 OS와 미들웨어의 디렉터리를 포함하기 때문에 용량이 매우 큼
- 용량이 한정된 물리 스토리지 영역을 효율적으로 사용하기 위하여 Copy on Write로 이미지 변경을 관리

#### Btrfs
- Oracle에서 2007년에 발표한 Linux용 Copy on Write vkdlf tltmxpa
- Docker는 Btrfs의 subvolume 기능과 snapshot 기능을 사용하여 컨테이너 이미지의 변경을 파일 시스템 층에서 관리

#### AUFS
- 서로 다른 파일 시스템의 파일과 디렉터리를 중첩하여 하나의 파일트리를 구성할 수 있는 파일 시스템
- 단, AUFS는 현재 Linux 커널 표준이 아니기 때문에 향후 어떻게 사용될지 알 수 없다.
- Mac OS X와 Windows에서 동작하는 Boot2Docker에서는 AUFS가 사용되고 있음

#### Device Mapper
- Linux 커널 2.6에 탑재된 Linux 블록 디바이스 드라이버와 이를 지원하는 라이브러리
- 파일 시스템의 블록 I/O와 디바이스 매핑을 관리
- Docker는 Device Mapper의 thin-provisioning 기능과 스냅샷(snapshot) 기능을 활용하고 있음
- 개별 파일 시스템에 의존하지 않으므로 다양한 환경에서 사용 가능
- Cent OS와 Fedora 등 Red Hat계 OSdhk Ubuntu 등에서 Docker를 사용할 때 Device Mapper가 쓰임

#### overlay
- Union filesystem의 하나로 파일 시스템에 또 다른 파일 시스템을 합치는 구조
- Linux 커널 3.18에 탑재된 기능으로, 읽기 전용 파일 시스템에 출력이 가능한 파일 시스템을 중첩하여 읽기 전용 파일 시스템의 디렉터리와 파일을 출력할 수 있게 만듬
- Dockersms 1.4.0에서 overlayfs에 대응
- 컨테이너 동작에 특화된 Linux 배포판인 CoreOS가 overlayfs를 채용