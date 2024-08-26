---
title: git pull 사용시 allow unrelated histories 옵션 사용하기
date: 2024-08-26 12:00:00 +0800
categories: [git]
tags: [git]
---

IntelliJ에서 미리 프로젝트를 구성해두고 bitbucket에 연결하며 git pull 명령 수행시 아래와 같은 에러가 발생했다. <br>

```
fatal: refusing to merge unrelated histories
```

git pull은 fetch와 merge를 포함하는 명령어이기에 로그상 merge 과정에서 문제가 발생한 것을 알 수 있었다.<br>
이러한 현상은 merge하고자 했던 두 가지 프로젝트가 이름이 같은 브랜치를 가지고 있으며 브랜치간 history가 불일치하기에 발생한 것이었다. <br><br>
물리적으로는 서로 다른 프로젝트이지만 논리적으로는 같은 프로젝트로 다루고 싶었기에 방법을 찾게 되었다. <br><br>

```
git pull --allow-unrelated-histories
```

위의 명령어를 통해서 merge할 수 있었다. <br>
다만 기존 두 프로젝트의 commit log에 따라서 충돌이 발생한 파일이 있었다. <br>
때문에 충돌은 수동으로 해결해야 했고 해결된 뒤 새로운 commit을 작성하여 최종적으로 원하던 구성을 만들 수 있었다.<br><br>

환경설정 초기 단계에서 종종 발생할 수 있는 에러이기 때문에 꼭 필요한 경우가 아니라면 형상 관리 플랫폼에서 **`.gitignore`** / **`README.md`** 파일들은 따로 추가하지 않는 것이 더 빠르게 환경 구축할 수 있는 방법인 것 같다.
