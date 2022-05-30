---
title: (Kafka) Burrow 설치 - Apple chip(M1) 대응
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- kafka
tag:
- kafka
- burrow
- m1
- siliicon
toc: true
toc_sticky: true
toc_label: 목차
description: (Kafka) Burrow 설치 - Apple chip(M1) 대응
article_tag1: kafka
article_tag2: interface
article_tag3: 
article_section: kafka
meta_keywords: 
last_modified_at: '2022-05-29 00:50:00 +0800'
---

## 1. Burrow ?

Kafka 의 Consumer Lag 을 모니터링 할 수 있는 Tool 입니다. Burrow를 스터디하기위해 설치하던 도중 확인한 내용을 정리 했습니다.

참고 : https://github.com/linkedin/Burrow

## 2. go 설치

```shell
# go 가 설치 되어있지 않으면 go 설치
brew install go
```

> 참고
> brew 로 go 를 install 했다면 `/opt/homebrew/Cellar/go/{버전}` 이 설치 경로이며 해당경로를 GOPATH 에 등록하자. go 가 설치된 경로를 보고싶다면 `go env GOROOT`

## 3. Go path 등록

```shell
# zsh 을 사용하는 환경이라 가정하고 홈 폴더에서 zsh 설정파일 오픈
vi .zshrc

# GOPATH 등록
export GOPATH=/opt/homebrew/Cellar/go/1.18.1
```

위에서 `1.18.1` 부분은 다운받은 버전에 맞게 변경 합니다.

## 4. Burrow 설치

```shell
git clone https://github.com/linkedin/Burrow.git {다운로드할 폴더명}

# 위의 Burrow 소스를 clone 받은 폴더로 이동하여 다음 명령어 실행
go mod tidy

go install
```

## 5. Burrow 설치 중 Trouble shooting

```shell
# github.com/linkedin/Burrow/core
core/logger.go:172:2: undefined: internalDup2
```

Burrow 설치 중 위와 같은 에러 메시지를 만났다면? 아래 경로에 있는 파일을 open 하여 가장 상단부분을 다음과 같이 수정합니다.

**삭제할 항목**

```go
//go:build !windows && !arm64
// +build !windows,!arm64
```

**추가할 항목**

```go
//go:build !windows && !(linux && arm64)
// +build !windows
// +build !linux !arm64
```

위와 같이 수정 한 이후에 `go install` 을 다시 수행하면 `$GOPATH/bin` 에 Burrow 가 생성 되었음을 확인 가능합니다.

![](https://velog.velcdn.com/images/doublejkim/post/5e9a46db-51a9-4138-a76f-ed3d985a7ad7/image.png)



> 해당 문제는 2월경 linkedin 에서 인지한 문제로 보이며 차후 공식버전에서는 위와 같은 Trouble shooting 을 하지않아도 될 것으로 보입니다. (현재 2022년 5월 기준)



참고 : https://github.com/linkedin/Burrow/pull/743
