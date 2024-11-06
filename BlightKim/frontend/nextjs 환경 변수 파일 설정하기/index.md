---
title: nextjs 환경 변수 파일 설정하기
parent: 프론트엔드
layout: default
---

* `.env` 파일 종류
    * 개발 환경용 : `.env.local`
    * 운영 환경용 : `.env.production`

    
* `.env` 파일 생성 후 API URL 설정


```shell
# .env.local
NEXT_PUBLIC_API_URL=http://localhost:8080/api
```


* `Axios` 인스턴스에서 환경 변수 사용 하기

```typescript
const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  // 기타 설정
});
```




