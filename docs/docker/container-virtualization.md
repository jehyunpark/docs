---
layout: default
title: Container Virtualization
parent: Docker
nav_order: 1
---

# Container Virtualization
{: .no_toc }


컨테이너 가상화 기술
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## 가상화 기술

### 가상 환경
- 가상 환경: 별도의 물리적인 하드웨어 대신 가상 OS 위에 애플리케이션을 구동하는 것
- 가상화 기술
  - 호스트 가상화
  - 하이퍼바이저 가상화
  - 컨테이너 가상화

### 호스트 가상화
- 하드웨어 위에 베이스가 되는 호스트 OS를 설치하고 그 위에 가상화 소프트웨어를 설치한 뒤 게스트 OS를 구동하는 가상화 기술
- 가상화 소프트웨어를 설치하여 간편하게 가상환경을 구축할 수 있기 때문에 개발 환경 구축에 많이 사용
- 호스트OS 상에서 게스트 OS가 동작하기 때문에 오버헤드가 클 수밖에 없음
- 종류
  - Oracle VM VirtualBox
  - VMWare Player

### 하이퍼바이저 가상화
- 하드웨어 위에 가상화 전문 소프트웨어인 '하이퍼바이저'를 설치하고 하드웨어와 가상 환경을 제어
- 호스트 OS 없이 하드웨어를 직접 제어하기 때문에 효율적으로 리소스를 사용할 수 있음
- 종류
  - Hyper-V
  - XenServer

### 컨테이너 가상화
- 호스트OS 상에서 논리적으로 구역(컨테이너)을 나눠 애플리케이션 동작을 위한 라이브러리와 애플리케이션 등을 컨테이너 안에 넣고, 개별 서버처럼 사용하는 것
- 오버헤드가 적어 가볍고 빠름
- Docker는 컨테이너 가상화 기술을 사용하기 때문에 다른 가상화에 비해 가볍고 빠르게 동작하는 것이 특징


## 컨테이너 가상화 기술의 역사

### 2002년~FreeBSD Jail
- 오픈소스 UNIX인 FreeBSD의 가상화 기술
- Jail은 영어로 '(감옥 등에)투옥하다'라는 의미로, FreeBSD 시스템을 Jail이라는 독립된 작은 단위로 분할할 수 있음
- 프로세스 분할
  - 같은 Jail에서 동작하는 프로세스에만 액세스할 수 있도록 프로세스를 분산
  - Jail에서 실행 중인 프로세스는 Jail 이외의 프로세스에는 영향을 미치지 않음
- 네트워크 분할
  - Jail에는 IP Address가 개별적으로 할당되며 여러 Address를 동시에 할당할 수도 있음
- 파일 시스템 분할
  - Jail에서 사용하는 파일 시스템을 분할하여 조작할 수 있는 커맨드와 파일을 제한
  - 관리자 권한 범위가 Jail 내에 제한되어 있기 때문에 시스템 관리자는 셧다운 등의 시스템 전체에 대한 조작 권한을 받는 대신 일반 사용자에게 관리자 권한을 주는 것이 특징

### 2005~Solaris Containers
- Oracle의 상용 UNIX인 Solaris의 가상화 기술
- Solaris Zone 기능
  - 하나의 OS 공간을 가상화로 분할하여 여러 OS가 동작하는 것처럼 보이게 하는 소프트웨어 파티션 기능
  - Solaris 환경을 독립된 여러 개의 Zone으로 분할하여 한대의 물리 서버 내에 최대 8192개의 가상 Solaris 환경을 구축할 수 있음
  - 기반이 되는 OS 영역을 'global zone', 분할된 가상 Zone을 'non-global zone'이라 하며 non-global zone에서 업무 애플리케이션이 동작
  - non-global zone은 서로 완전하게 분리되어 있기 때문에 non-global zone에서 동작하는 프로세스테 또 다른 non-global zone이 액세스 할 수 없음
- Solaris 리소스 매니저 기능
  - non-global zone에서 CPU와 메모리 등 하드웨어 리소스를 배분하는 리소스 관리 기능
  - 중요도가 높은 시스템에 리소스를 우선 할당할 수 있음
- Docker와 무척 유사
  - Solaris Containers는 한 대의 물리 서버를 가상화하여 애플리케이션 실행 환경을 Zone으로 분할한 뒤 각각 제어
