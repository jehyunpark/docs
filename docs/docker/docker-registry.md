---
layout: default
title: Docker Registry
parent: Docker
nav_order: 5
---

# Docker Registry
{: .no_toc }


Docker Registry - Docker 이미지 공유
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Private 레지스트리 구축 및 관리

### Docker 레지스트리 구축
- Docker Hub에 공개되어 있는 공식 이미지인 registry를 사용
- docker search 커맨드로 registry 확인
  ```
  $ docker search registry
  ```
- registry 이미지 다운로드
  ```
  $ docker pull registry:2.0
  ```
- 다운로드한 registry 이미지 확인
  ```
  $ docker images registry
  ```
- 컨테이너 구동
  ```
  $ docker run -d -p 5000:5000 registry:2.0
  ```
- 컨테이너 확인
  ```
  $ docker ps --format "{{.ID}}\t{{.Image}}\t{{.Ports}}
  ```

### 이미지 업로드
- 이미지 생성
  ```
  # Docker 이미지
  FROM centos:latest

  # 작성자 정보
  MAINTAINER 0.1 your-name@your-domain.com

  # Apache httpd 설치
  RUN ["yun", "-y", "install", "httpd"]

  # Apache httpd 실행
  CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
  ```
- Docker 실행
  ```
  $ docker build -t webserver .
  ```
- 태그명(private repository)
  ```
  docker tag [로컬 이미지명] [Docker repository의 ID address 또는 호스트명:포트 번호]/[이미지명]
  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
  ```
- 태그 설정 예
  ```
  $ $ docker tag webserver localhose:5000/httpd:1.0
  ```
- 이미지 업로드
  ```
  $ docker push localhost:5000/httpd
  ```
- 로컬 이미지 삭제
  ```
  $ docker rmi webserver
  $ docker rmi localhost:5000/httpd
  ```

### 이미지 다운로드
- 이미지 다운로드
  ```
  $ docker pull localhost:5000/httpd
  ```

## Amazon S3를 사용하여 이미지 공유

## Amazon ECR 사용하여 이미지 공유