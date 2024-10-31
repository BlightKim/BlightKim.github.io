---
title: 우분투 22.04에 npm 설치하기
parent: 프론트엔드
layout: default
---

# 들어가며

젠킨스 서버에서 `nextjs`를 빌드하려고 했더니 node와 npm 설치를 안한 사실을 깜빡했다

나중에 서버 설정 시 참고하기 위해 기록으로 남겨둔다

# NVM을 사용하여 Node.js 및 npm 설치

>  NVM은 사용자별로 여러 Node.js 버전을 관리 할 수 있게 해준다

- NVM 스크립트를 다운받는다

```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

# 설치 후 실행
source ~/.bashrc
```

- 설치가 완료되었으면 NVM 버전을 출력해본다

```shell
nvm -v

0.39.1
```

정상적으로 출력되는 것을 확인 할 수 있다

- 설치 가능 리스트 가져오기

```shell
nvm list-remote

        v22.5.0
        v22.5.1
        v22.6.0
        v22.7.0
        v22.8.0
        v22.9.0
       v22.10.0
       v22.11.0   (Latest LTS: Jod)
        v23.0.0
        v23.1.0
...이하 생략...
```

- 특정 버전의 Node.js 설치하기

```shell
nvm install 20
```

- 특정 버전의 Node.js 사용하기

```shell
nvm use 20
```

- Node.js 버전 출력하기

```shell
node -v

v20.18.0
```

정상적으로 Node.js 가 설치된 것을 확인 할 수 있다

