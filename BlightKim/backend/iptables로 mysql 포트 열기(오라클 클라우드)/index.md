---
title: iptables로 mysql 포트 열기(오라클 클라우드)
parent: 백엔드
layout: default
---

# 들어가며

오라클 클라우드 서버에 mysql을 도커로 배포하고, 로컬에서 오라클 클라우드의 mysql에 접속하려고 한다


- 오라클 클라우드 방화벽 설정 정보를 확인한다

```shell
iptables -nL
```
현재 INPUT (내부 -> 외부) 설정은 다음과 같다

```shell
Chain INPUT (policy DROP)
target     prot opt source               destination
ACCEPT     0    --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     1    --  0.0.0.0/0            0.0.0.0/0
ACCEPT     0    --  0.0.0.0/0            0.0.0.0/0
ACCEPT     17   --  0.0.0.0/0            0.0.0.0/0            udp spt:123
ACCEPT     6    --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
REJECT     0    --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited
```

규칙마다 번호가 나오게 하려면

```shell
sudo iptables -nL --line-numbers
```

`--line-numbers`를 추가해줘야 한다

`--line-numbers`를 추가해주면`

```shell
Chain INPUT (policy DROP)
num  target     prot opt source               destination
1    ACCEPT     0    --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
2    ACCEPT     1    --  0.0.0.0/0            0.0.0.0/0
3    ACCEPT     0    --  0.0.0.0/0            0.0.0.0/0
4    ACCEPT     17   --  0.0.0.0/0            0.0.0.0/0            udp spt:123
5    ACCEPT     6    --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
6    REJECT     0    --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited
```

다음과 같이 num 목록이 나오는 것을 볼 수 있다

```shell
iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```

mysql 포트를 허용해준다
