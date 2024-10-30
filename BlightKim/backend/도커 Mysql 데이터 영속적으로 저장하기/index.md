---
title: 도커 Mysql 데이터 영속적으로 저장하기
parent: 백엔드
layout: default
nav_order: 4.5
---
# 들어가며

Docker 컨테이너 내부에 저장되는 데이터는 Docker 컨테이너가 삭제 될 때 사라진다


# 해결 방법

- 도커에 호스트 볼륨을 연결한다

현재 호스트 볼륨 구조는 다음과 같다

```
.
├── docker-compose.yml
└── mysql
    └── db
```

- docker-compose.yml 에서 호스트 볼륨과 컨테이너를 연결한다

```yaml
version: '3'
services:
  db:
    image: mysql:8.0.33
    container_name: [컨테이너 이름]
    restart: always
    ports:
      - "3306:3306"
    command:
      - "--character-set-server=utf8mb4"
      - "--collation-server=utfmb4_unicode_ci"
    environment:
      MYSQL_ROOT_PASSWORD: [비밀번호]
      TZ: Asia/Seoul
    volumes:
      # 컨테이너와 호스트 볼륨 연결
      - ./mysql/db:/var/lib/mysql
```

- 실행 결과

```
.
└── db
    ├── #ib_16384_0.dblwr
    ├── #ib_16384_1.dblwr
    ├── #innodb_redo  [error opening dir]
    ├── #innodb_temp  [error opening dir]
    ├── auto.cnf
    ├── binlog.000001
    ├── binlog.index
    ├── ca-key.pem
    ├── ca.pem
    ├── client-cert.pem
    ├── client-key.pem
    ├── ib_buffer_pool
    ├── ibdata1
    ├── ibtmp1
    ├── mysql  [error opening dir]
    ├── mysql.ibd
    ├── mysql.sock -> /var/run/mysqld/mysqld.sock
    ├── performance_schema  [error opening dir]
    ├── private_key.pem
    ├── public_key.pem
    ├── server-cert.pem
    ├── server-key.pem
    ├── sys  [error opening dir]
    ├── undo_001
    └── undo_002
```

도커 컨테이너와 호스트 볼륨이 연결되어 mysql 관련 파일이 생성되었다.

