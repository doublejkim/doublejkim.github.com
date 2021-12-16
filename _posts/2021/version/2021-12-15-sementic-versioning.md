---
title: 버전 표시도 스펙이 있을까? - Sementic Versioning (SemVer)
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- version
tag:
- version
toc: true
toc_sticky: true
toc_label: 목차
description: 버전 표시도 스펙이 있을까? - Sementic Versioning (SemVer)
article_tag1: version
article_tag2: spring
article_tag3: 
article_section: version
meta_keywords: version, spring
last_modified_at: '2021-12-15 18:00:00 +0800'
---

## 1. 버전 네이밍 기준

버전 관리에 대한 정의를 확인하던 중 Semantic Versioning 이라는 Spec 이 존재하는 것을 확인하여 요약을 하였다. 스펙이라고하면 거창할지 모르지만 결국 모든 스펙은 체계적으로 잘 구축해보자는 것이 목표이다. 이미 훌륭한 분이 [Semantic Versioning](https://semver.org/) 이라고하는 권고 스펙을 정의를 해두었다. [Semantic Versioning](https://semver.org/) 은 그라바타(Gravatars)의 창시자이자 깃헙(GitHub)의 공동창업자인 [톰 프레스턴-베르너](https://tom.preston-werner.com) 가 작성했으며, 흔히 오픈소스에서 사용하는 버전 체계의 기준이 되고 있다.

## 2. Semantic Versioning 의 포맷

![sementic versioning format]({{site.baseurl}}/assets/images/posts/2021/20211215_semver1.png)
흔히 볼 수 있는 버전 표기의 형태는 위와 같다. x, y, z 는 증가하는 자연수(음이 아닌 정수)형태로 표시되며, 절대로 0이 앞에 붙어서는 안된다 (예: 1.0.03 과 같은 버전 표기는 하지 말아야한다). 
그리고 위에서 표시된 **x, y, z 는 각각 Major, Minor, Patch 의 버전을 의미한다.**

### 2.1. Major

**프로그램의 기능적 변경이 이전 버전과 호환되지 않을 경우 Major 버전을 증가시킨다.** Major 버전이 증가된 경우에는  Minor와 Patch 버전은 0으로 리셋시켜야 한다. 예를들어 현재 프로그램의 버전이 1.2.5 이고 다음 버전이 현재버전과 호환되지 않는 기능적 추가나 변경이 일어났다면 새버전은 2.0.0 이 되어야 한다.

### 2.2. Minor

**프로그램이 이전 버전과 호환에 문제가 없는 상태에서 프로그램의 기능적 변경이나 추가가 이루어질 경우 Minor 버전을 증가시킨다**. 예를들어 현재 프로그램의 버전이 1.2.5 이고 다음 버전이 현재버전과 호환되는 상태에서 기능적 추가나 변경이 일어났다면 새버전은 1.3.0 이 되어야 한다.

### 2.3. Patch

프로그램이 **이전 버전과 호환에 문제가 없는 상태에서 Bug fix 등을 위한 변경이 이루어질 경우 Patch 버전을 증가**시킨다. 

### 2.4. Pre-Release 와 Build

Patch 버전 뒤에 프로그램의 Pre-release label 이나 Build number 같은 메타데이터를 위한 정보를 추가적으로 표시할 수 있다. 예를들어 *1.0.0-alpha.1* 이라고 표시된 버전의 경우 라면, 1.0.0 버전의 정식 배포를 앞둔 버전이며 내부적으로 alpha.1 이라고 라벨링 했다는 것을 알 수 있다.

Build metadata 는 Patch 버전 뒤에 더하기(+) 기호를 붙인 뒤에 마침표로 구분된 식별자를 덧붙여서 표현한다. 필요하다면 간단한 단어를 표기할 수 있다. (예 : 1.0.0-beta+exp.sha.5114f85)

참고로 Build metadata 는 우선순위에 영향을 주지 않는다.

## SemVer 규칙을 확인 할 수 있는 정규 표현식

[semver.org](https://semver.org/) 의 하단부 FAQ 에 보면 Sementic Versioning 에서 권고하는 규칙에 맞는 버전포맷인지 확인 할 수 있는 정규표현식과 링크를 볼 수 있으므로 관심이 있다면 참고해보자.

Capture Group 명을 지원하는경우 (major, minor, patch...)
[https://regex101.com/r/Ly7O1x/3/](https://regex101.com/r/Ly7O1x/3/)

```regex
^(?P<major>0|[1-9]\d*)\.(?P<minor>0|[1-9]\d*)\.(?P<patch>0|[1-9]\d*)(?:-(?P<prerelease>(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+(?P<buildmetadata>[0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

Capture Group 명을 지원하지 않는 경우

[https://regex101.com/r/vkijKf/1/](https://regex101.com/r/vkijKf/1/)

```regex
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

## 참고자료

* [https://semver.org/](https://semver.org/)

* [https://www.baeldung.com/cs/semantic-versioning](https://www.baeldung.com/cs/semantic-versioning)
