---
title: API 명세 포맷 학습하기
parent: 백엔드
layout: default
---
# API 명세 포맷이란 ?

> API의 세부사항 표현을 위한 데이터 포맷


# OAS(OpenAPI Specification) 소개

- OAS는 `yaml` 포맷으로 작성

# 쿼리 파라미터 묘사

사용자는 `<GET> /products?free-query={free-query}` 형태로 요청해야 함

```yaml
openapi: 3.0.0
info:
  title:
```
