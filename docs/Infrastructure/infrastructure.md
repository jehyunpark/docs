---
layout: default
title: Infrastructure
nav_order: 3
---

# Infrastructure
{: .no_toc }


애플리케이션 동작에 필요한 하드웨어와 OS 및 미들웨어 등
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 구성 요소

### 시스템 요구 사항
- 기능 요건
  - 필요한 시스템 기능을 정의한 것
  - 시스템과 소프트웨어에서 무엇이 가능한지를 정리하여 프로그래밍으로 해결
- 비기능 요건
  - 시스템 성능, 신뢰성, 확장성, 운영성, 보안 등 기능요건 이외의 모든 요건

### 기본 구성 요소
- 하드웨어
- 네트워크
- OS
- 미들웨어

### 인프라의 종류
- 온프레미스
- 퍼블릭 클라우드
- 프라이빗 클라우드
- 적합한 인프라 종류
  - 클라우드에 적합한 케이스
    - 트래픽 변화가 많은 시스템
    - 재해에 대비하기 위해 해외에 백업을 구축하고자 하는 시스템
    - 되도록 빨리 동작해야 하는 시스템
      - 신규 서비스를 빠르게 제공
      - 새로운 회사 설립
      - 비즈니스 계획에서부터 시스템 릴리즈까지 최대한 기간을 단축해야 하는 서비스
  - 온프레미스에 적합한 케이스
    - 높은 가용성이 요구되는 시스템
    - 높은 기밀성이 요구되는 데이터를 다루는 시스템
    - 특수 요건의 시스템
    - 총 비용이 높은 시스템

### 인프라 구축 및 운영 프로세스
- 시스템 구축 계획 및 요구 사항 정의 단계
- 인프라 설계 단계
- 인프라 구축 단계
- 운영 단계

## 네트워크 및 하드웨어

### 네트워크 Address
- Mac Address(물리 Address, 이더넷 Address)
  - 네트워크 인터페이스 카드와 무선 LAN 칩 등 네트워크 부품에 물리적으로 할당되어 있는 48bit 주소
  - 앞의 24bit는 네트워크 부품 메이커를 식별하는 번호, 뒤의 24bit는 각 메이커에 할당된 고유 번호
  - 16진수 표기법으로 나타내며 앞에서부터 2byte씩 나누어 표기
  - OSI 7 Layer(OSI 7 계층)의 Layer 2의 DataLink Layer에서 사용됨
- IP Address
  - IPv4 - 8bit씩 4개로 구분된 32bit (ex. 192.168.1.1)
    - IPv4에서는 2^32(약 42억)까지만 한 네트워크에 접속할 수 있기 때문에 사내 네트워크는 임의의 주소를 할당하여 Private Address를 사용하며 인터넷으로 나갈 때는 Global Address와 Private Address 등을 변환(NAT)할 수 있는 장비를 설치하여 운영
  - IPv6 - 128bit로 구성
  - Global Address는 전 세계에서 중복되지 않아야 하므로 NIC(Network Information Center)에서 할당

### OSI 7 Layer와 통신 프로토콜
- 국제표준화기구(ISO)에서 정한 컴퓨터 통신을 계층 구조로 나눈 것
- Layer 1~4의 하위층, Layer 5~7의 상위층
- Application Layer(Layer 7)
  - 애플리케이션에 특화된 프로토콜
  - 웹의 HTTP와 메일을 전송하는 SMTP 등
- Presentation Layer(Layer 6)
  - 데이터 표현 형식 정의
  - 데이터 저장 방식과 압축, 문자 코드 등 
- Session Layer(Layer 5)
  - 커넥션이 이루어지는 타이밍과 데이터 전송타이밍 정의
  - 세션은 애플리케이션 사이에 일어나는 request와 response에서 구성
- Transport Layer(Layer 4)
  - 데이터 전송을 제어하는 계층
  - 오류를 감지, 재전송
  - TCP, UDP
  - 상대방과 통신할 때 데이터를 확식하게 보내는 역할
- Network Layer(Layer 3)
  - 서로 다른 네트워크 사이에서 이루어지는 통신을 위한 계층
  - 대표적인 프로토콜: IP Address
  - 라우터, L3 스위치
- Data Link Layer(Layer 2)
  - 같은 네트워크 내(동일 세그먼트)에 있는 노드 사이의 통신을 위한 계층
  - MAC Address에 따라 데이터 전송
  - L2 스위치
- Physical Layer(Layer 1)
  - 통신 장비의 물리적, 전기적인 특성에 대한 계층

### 방화벽
- 내부 네트워크와 외부와의 통신을 제어하고 내부 네트워크를 안전하게 지키기 위한 기술
- 패킷 필터링 방식
  - 통신하는 패킷을 포트 번호와 IP Address 기반으로 필터링하는 방법
  - 80(http), 443(https)만 여는 등 특정한 룰을 정하면 이 룰에 따라 패킷을 필터링
  - Network Layer(Layer 3), Transport Layer(Layer 4) 에서 동작
  - 패킷 필터 룰: ACL(Access Control List)
- 애플리케이션 게이트웨이 방식
  - 패킷 대신 애플리케이션 프로토콜 레벨에서 외부와 통신하며 제어하는 방법
  - 일반적으로 프록시 서버

### 라우터와 L3 스위치
- 라우터: 서로 다른 네트워크를 연결하기 위한 통신 장비
  - 경로 선택 기능
    - 정적 경로(Static Route)
      - 라우터에 설정되어 있는 라우팅 테이블 기반으로 결정
    - 동적 경로(Dynamic Route)
      - 라우팅 프로토콜에서 설정
- L3 스위치
  - 라우팅을 하드웨어에서 처리하기 때문에 빠르게 동작
  - 접속할 수 있는 이더넷 포트 수가 많아 업무 시스템 등에 넓게 사용

### 서버
- CPU
  - 프로그램 연산과 처리 등을 수행하는 전자 회로 부품
  - Core: 주요 연산 회로
  - 대부분 multicore
  - 동적 캐시 사용: 메모리와의 처리 속도 차를 줄이기 위해
- Memory
  - CPU가 직접 액세스할 수 있는 기억장치
- Storage
  - 데이터베이스의 데이터를 저장하는 디바이스, 보조기억장치

## OS

### Linux
- Linux 커널
  - OS의 중심이란 의미
  - 메모리 관리, 파일 시스템, 프로세스 관리, 디바이스 제어 등
- Linux 배포판
  - Debian
    - Debian
    - KNOPPIX
    - Ubuntu
  - Red Hat
    - Fedora
    - Red Hat Enterprise Linux
    - CentOS
    - Vine Linux
  - Slackware
    - openSUSE
    - SUSE Linux Enterprise
  - etc
    - Arch Linux
    - Gentoo Linux
    - Google Chrome OS

### Linux 커널
- 하드웨어 제어를 담당하는 OS의 코어 기능
- C언어와 어셈블리 언어로 구성
- 디바이스 관리
  - 하드웨어(CPU, 메모리, 디스트, 입출력 디바이스 등)를 디바이스 드라이버 소프트웨어로 제어
- 프로세스 관리
  - 명령을 실행할 때 프로그램 파일에 저장된 내용을 읽어 메모리에 뿌린 뒤 이를 실행
  - 이렇게 실행된 프로그램이 프로세스
- 메모리 관리
  - 프로그램 및 데이터를 물리 메모리에 적절하게 분배
  - 실행이 끝난 프로세스의 메모리 영역을 반납
  - 메모리에는 용량 제한이 있어 물리적인 용량을 넘어선 프로그램이나 데이터가 진행되면 하드디스크와 같은 보조기억장치의 가상 메모리 영역이 사용됨
  - 이러한 가상 메모리 영역을 스와프라고 함
- 쉘(Shell)
  - 사용자 명령을 커맨드에 입력하고 이를 Linux 커널에 전달
  - 실행 가능한 범위
    - 애플리케이션 실행, 정지, 재실행
    - 환경변수 관리
    - 커맨드 이력 관리(커맨드 히스토리)
    - 커맨드 실행 결과 표시 및 파일 출력
  - 종류
    - bash
    - csh
    - tcsh
    - zsh

### Linux 파일 시스템
- Linux에서 하드디스크와 USB 메모리, CD, DVD 등 데이터에 액세스하기 위한 시스템
- VFS(Virtual File System: 가상 파일 시스템)을 사용하여 데이터에 편리하게 액세스함
- VFS는 각 디바이스를 파일로 취급
- 주요 Linux 파일 시스템
  - ex2
  - ex3
  - ex4
  - tmpfs
  - UnionFS
  - ISO-9660
  - NFS(Network File System)

### Linux 디렉터리 구성
- 디렉터리에는 Linux 커널을 포함하여 각종 커맨드 및 설정 파일이 저장됨
- FHS(Filesystem Hierarchy Standard)로 표준화
- 주요 디렉터리
  - `/bin`: 기본 커맨드
  - `/boot`: OS 구동에 필요한 파일
  - `/dev`: 디바이스 파일
  - `/etc`: 설정 파일
  - `/home`: 사용자 홈 디렉터리
  - `/lib`: 공유 라이브러리
  - `/mnt`: 파일 시스템의 마운트 포인트용 디렉터리
  - `/media`: CD/DVD-ROM 등 마운트 포인트
  - `/opt`: 애플리케이션 소프트웨어 패키지
  - `/proc`: 커널과 프로세스 관련 정보
  - `/root`: 루트 사용자용 홈 디렉터리
  - `/sbin`: 시스템 관리용 커맨드
  - `/tmp`: temporary 디렉터리
  - `/srv`: 시스템 고유 데이터
  - `/usr`: 각종 프로그램 및 커널 소스를 저장하는 디렉터리
  - `/var`: 로그와 메일 등 가변 파일을 저장하는 디렉터리

### Linux 보안 기능
- 계정을 이용한 권한 설정
  - 'root', 일반 사용자, 시스템 계정
  - 그룹 설정 가능
- 네트워크 필터링
  - `iptables`: Linux가 갖고 있는 패킷 필터링 및 네트워크 Address 변환(NAT) 기능을 설정
- SELinux(Security-Enhanced Linux)
  - 미국국가안전보장국이 제공하는 Linux 커널에 강제 액세스 제어 기능을 추가
  - TE(Type Enforcement), RBAC(Role-based Access Control) 등으로 Linux를 제어
  - root 사용자에게 권한이 집중되는 것을 방지하여 보안이 강화된 시스템을 구축 가능

## 미들웨어
- OS와 업무 처리를 수행하는 애플리케이션 사이에 있는 소프트웨어

### 웹 서버 및 웹 애플리케이션 서버
- 웹 서버: 클라이언트 브라우저에서 HTTP request를 받아 웹 콘텐츠(HTML, CSS 등)을 response 하거나 다른 서버 프로그램을 호출하는 등의 기능을 가진 서버
  - Apache HTTP Server
  - Internet Information Services(IIS)
  - nginx
  - GlassFish
  - Apache Tomcat
  - IBM WebSphere Application Server

### 데이터베이스 서버
- 시스템이 생성하는 여러가지 데이터를 관리하기 위한 미들웨어
- 데이터베이스관리시스템: 데이터 검색, 등록, 변경, 삭제 등 기본 기능과 함께 트랜잭현 처리 등 포함
  - MySQL
  - PostgreSQL
  - Oracle Database
  - DB2
  - MongoDB

### 시스템 통합 운영 모니터링 툴
- Zabbix
- Hinemos
- JP1
- Senju
- Mackerel
- Datadog


## 인프라 구성관리

### 인프라 구성관리
- 인프라를 구성하는 하드웨어, 네트워크, OS, 미들웨어, 애플리케이션 구성 정보를 관리하고 최적의 상태를 유지하는 것
- 클라우드에서는 구축된 인프라를 변경하지 않고 파기한 뒤 새로 구축하는 것이 가능하므로 지금까지 큰 부담으로 느껴졌던 인프라 변경 이력 관리도 필요 없어졌으며 현재 가동 중인 인프라 상태만 관리하면 되는 환경으로 변하고 있음
- 이러한 인프라를 'immutable Infrastructure(불변 인프라)'라고 함

### Infrastructure as Code
- 프로그램 코드에 입력된 내용을 자동으로 설정하는 방법
- 실행자와 상관없이 같은 상태의 인프라 환경을 구축해내는 것에 유리
- 인프라 구성 정보를 코드로 관리해두면 변경 이력을 일원화하여 관리할 수 있음

### 대표적인 인프라 구성관리 툴
- 네트워크와 OS를 포함한 서버 구성을 자동으로 구축하여 관리
- 자동으로 OS와 미들웨어의 정의 파일을 작성
- 여러 대의 서버에 애플리케이션을 일괄적으로 deploy하거나 통합관리
- 종류
  - OS를 자동으로 부팅하는 툴(Bootstrapping)
    - KickStart, Vagrant 등
  - OS와 미들웨어를 자동으로 설정하는 툴(Configuration)
    - Chef, Ansible, Puppet
  - 여러 서버를 자동으로 관리할 수 있는 툴(Orchestration)
    - Serf
    - Capistrano
    - Kubernetes

## Reference
- Official
  - [완벽한 IT 인프라 구축을 위한 Docker](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788956747019&orderClick=JAx&Kc=)
- Unofficial