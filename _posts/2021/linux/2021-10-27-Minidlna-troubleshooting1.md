---
title: miniDLNA 구동 후 접속 기기에서 파일이 보이지 않을경우
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- linux
toc: true
toc_sticky: true
toc_label: 목차
description: miniDLNA 구동 후 접속 기기에서 파일이 보이지 않을 경우 혹은 miniDLNA 상태가 에러인경우
article_tag1: miniDLNA
article_tag2: linux
article_tag3: 
article_section: linux
meta_keywords: minidlna, linux
last_modified_at: '2021-10-27 25:00:00 +0800'
---

### miniDLNA ?

손쉽게 미디어 서버를 구축할 수 있는 miniDLNA 라는 서버가 있으며, DLNA/UPnP 를 지원하는 스마트 TV 나 그에 대응되는 기기가 있을 경우 동일 네트워크 상에서 손쉽게 영상 플레이 등이 가능합니다.

현재 이문서는 miniDLNA 설치를 다루지는 않으며, 파일이 보이지 않을경우의 트러블 슈팅만 언급하겠습니다.

### 파일이 보이지 않을 경우에는 status 확인

miniDLNA 명령어 옵션 중 Status  를 확인할 수 있는 status 옵션이 있습니다. 해당 옵션으로 실행 하면 miniDLNA 의 상태를 확인 할 수 있습니다. (service minidlna status)

![minidlna status]({{ site.baseurl }}/assets/images/posts/2021/20211027_minidlna1.png)

파일이 보이지 않을 경우에 고려해봐야할 점은 네트워크 설정, miniDLNA 구동 여부 등을 먼저 확인해봐야하지만 위의 경우에는 동영상 파일이 존재하는 위치의 디렉토리의 권한 접근 문제가 발생하여 에러가 발생하고 있는 중 입니다.

### 권한 설정 변경

해당 디렉토리의 권한을 chmod 명령어로 변경해도 무방할 것으로 보이나, 여기서는 miniDLNA 데몬의 실행시 권한을 변경할 것 입니다.

흔히 포트나 영상파일이 존재하는 미디어 디렉토리를 설정하는 /etc/minidlna.conf 설정 파일 외에 miniDLNA 구동시 참조하는 초기 스크립트 관련 파일이 존재하는데 해당 파일에서 USER 와 GROUP 으로 표시되어있는 실행권한을 root 로 변경합니다.

> sudo vi /etc/minidlna

![minidlna init config]({{ site.baseurl }}/assets/images/posts/2021/20211027_minidlna2.png)

설정 변경 후 minidlna 을 재시작 한후 status 를 확인하면 다음과 같이 정상적으로 실행 중임을 알 수 있으며, 동일네트워크의 DLNA 을 접근할 수 있는 기기를 확인하면 미디어 디렉토리에 존재하는 동영상을 볼 수 있습니다.

![minidlna result]({{ site.baseurl }}/assets/images/posts/2021/20211027_minidlna3.png)
