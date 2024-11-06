---
title: Mysql 계정 생성 및 권한 설정
parent: 백엔드
layout: default
---

- `root` 계정으로 접속한다

```shell
sudo mysql -u -root -p

# 패스워드 입력 후 접속
```

- 스키마를 생성한다

```shell
create database [스키마 이름] default character set utf8mb4;
```

- 계정을 생성한다

```shell
create user '[계정]'@'%' identified by '[비밀번호]';
```

- 권한을 부여한다 

```shell
MariaDB [mysql]> grant all privileges on *.* to '계정'@'%';	
--> 특정 계정에 대해 모든 스키마 권한 부여
Query OK, 0 rows affected (0.000 sec)
```


- root 사용자의 호스트 제한 설정

```shell
-- 'root' 사용자의 접속 호스트를 'localhost'로 제한
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'your_root_password';
ALTER USER 'root'@'localhost' IDENTIFIED BY 'your_root_password';
```
