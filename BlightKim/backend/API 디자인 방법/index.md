---
title: API 디자인 방법
parent: 백엔드
layout: default
---

# API 설계 전 간단한 질문

- `무엇을` - 사용자는 무엇을 할 수 있는가 ?
- `어떻게` - 사용자가 어떻게 하는가 ?
- `입력` - 각 단계별로 무엇이 필요한가 ?
- `응답` - 무엇을 응답받는가 ?

이 질문을 통해 API로 무엇을 할 수 있는지 대략 알 수 있다

- 심화 예제
    1. 쇼핑 시 무엇을 할까 ?
    2. 상품을 구입한다
    3. 상품은 어떻게 구매 ?
    4. 쇼핑 카트에 담고 결재 한다

> **상품 구입**
>
> 1. 상품을 카트에 담는다
>   1. 상품과 쇼핑 카트가 필요하다(입력)
>   2. 받는 것이 없다
>   3. 상품을 어떻게 카트에 담을까 ?
>       1. 상품을 카트에 담기 전 검색한다 (어떻게)
>           * 비정형 쿼리를 사용한다 (입력)
>               * 어디서 오는걸까 ? API 사용자가 제공
>           * 쿼리에 맞는 상품을 반환받는다(출력)
>               * 이 상품은 어떻게 쓰일까 ? 사용자가 하나를 선택해 카트에 담는다
>           
> 
> 
> 2. 결재한다
>   1. 쇼핑카트가 필요하다
>   2. 주문서를 받는다
>       1. 주문서를 가지고 뭘 할까 
>           * 사용자는 주문을 관리할 수 있다
>           
> 
>  **프로세스**
> 
> 1. 상품 구입을 두 단계의 목표로 분리
> 2. 입력에 대한 질문 (무엇이 필요한가)
> 3. 출력에 대한 질문 (무엇을 반환받는가)
> 4. 누락된 목표가 있는가 ?


# API 목표 캔버스

1. 누가 - API 사용하는 사용자들 나열
2. 무엇을 - API로 사용자들이 할 수 있는 것 나열
3. 어떻게 - 무엇을 단계별로 분해해 나열
4. 입력 - 각 단계 진행하기 위해 필요한 요소와 요소의 원천 나열
5. 출력 - 사용처, 각 단계의 반환과 쓰임새 나열
6. 목표 - 명시적이고 간결하게 입력

{:important}
> 한번에 모든 것을 해결하지 마라 하나씩 차근차근 !

데이터베이스의 모델이 외부로 노출되는 경우 보통 안 좋은 아이디어다

비즈니스 로직을 노출하면 소비자 뿐 아니라 공급자에게도 악 영향을 끼친다



# Rest (Representational State Transfer)

- /products/{productId}
    - 입력 파라미터 : productId
    - 상품 중에 {productId} 라는 상품

- REST API 는 HTTP 프로토콜을 완전히 의존한다
  - `GET` HTTP 메서드는 리소스를 찾아서 가져오는 것을 의미

# 리소스를 프로그래밍적 표현으로 변경하기

* 기능적 요구사항 분석
  1. 리소스 관계를 통해 식별
  2. 액션과 파라미터, 반환값 식별
  3. 리소스 경로 디자인
  4. HTTP 이용한 리퀘스트 액션


* 무엇을 위한 API 목표 캔버스

| 누가      | 무엇을                       | 어떻게                   | 입력(액션)                                         | 출력(사용정보)                          | 목표                              |
|-----------|------------------------------|--------------------------|---------------------------------------------------|----------------------------------------|-----------------------------------|
| 관리자    | 카탈로그를 관리한다.         | 상품을 추가한다.         | 카탈로그 API, 상품 선택(사진 추가)                 | 추가된 상품(사진/이름, 설명, 가격)      | 상품을 카탈로그에 추가한다.         |
| 관리자    | 상품 정보를 가져온다.         |                          | 상품(목록, 추가)                                  | 상품 목록(사진/이름, 설명, 가격)        | 상품을 가져온다.                   |
| 관리자    | 상품 정보를 수정한다.         |                          | 상품(기존있기, 검색, 수정)                        | 상품 정보 수정됨                        | 상품을 수정한다.                   |
| 관리자    | 상품을 제거한다.              |                          | 상품(기존있기, 검색, 추가)                        | 상품이 제거됨                          | 상품을 제거한다.                   |
| 사용자    | 상품을 검색한다.              |                          | 카탈로그 API, 비정확한 쿼리(사용자 작성)           | 비정확한 쿼리를 통해 카탈로그에서 상품을 검색한다. | 상품을 검색한다.                   |


# 경로를 포함한 리소스 표현

- REST 리소스의 경로는 계층적 구조와 타입을 표현해야 한다

> 1. `/catalog/{productId}` : 카탈로그의 아이템
> 2. `/products` : 상품의 콜렉션
> 3. `/products/{productId}` : 특정 상품

가장 일반적으로 쓰이는 포맷은 `/{콜렉션의 아이템 타입을 반영하는 복수형 이름}/{아이템의 id}`

- 상품 카탈로그에 상품을 추가하는 건 `<POST> /products`
  - 새로 생성된 리소스를 리턴하는 것이 좋음

- 카탈로그에서 비정형 질의를 이용해 상품을 조회하는 표현
  - `<GET> /products?{free-query}={free-query}`

- 상품 삭제 표현
  - `<DELETE> /products/{productId}`

- 상품 수정 표현
  - `<PATCH> /products/{productId}`
  -  리소스의 부분 수정 시 `PATCH` 사용
  -  입력 파라미터는 `리퀘스트 바디`로 전달

- 상품 교체 표현
  - `<PUT> /products/{productId}` 
  - 기존 리소스가 없는 경우 생성되고, 생성된 리소스를 리턴하는 것이 좋음

# API 데이터 디자인

1. `컨셉디자인` : 컨셉에서 속성이 무엇
2. `응답디자인` : 전부 반환될 필요 있나 ?
3. `파라미터 디자인` : 사용자가 전부 제공해야하는가 ?
4. `파라미터 제어` : 이 파라미터는 어디를 통해 들어오는가 ?

### 컨셉디자인

- 가장 중요한 속성은 바로 이름이다 
- 보다 자신을 잘 설명하는 이름이 좋은 이름
- 속성 항목
  1. 이름
  2. 타입
  3. 필수 여부
  4. 부가 설명

- 이름과 타입은 인터페이스를 디자인 할 때 가장 분명한 정보
- 이름과 타입만으로 불분명 할 때 부가설명이 유용

### Response 컨셉디자인

- 사용하는 사람 입장에서 필요한 정보만 남김

### 파라미터 컨셉 디자인

- 전부 사용자의 관심사항 인가 ?
- 정말 필요한가 ?
- 사용자가 이해할 수 있나 ?


# API 디자인 난관에 봉착했을 때 균형 유지하기

### 예시

카트를 결제하는 목표를 달성해야 프로세스가 끝남

1. 액션 그 자체 의미하는 리소스 만들어 보기
  * `/cart/check-out` or `/check-out-cart`
  * `cart.checkout()` 은 `/cart/check-out` 으로 표현할 수 있다

2. REST 모델에 가까운 해법 
  * 카트 결제 시 상태 변경 된다고 간주
  * 카트 리소스는 상태 속성 포함
  * `PATCH` 사용하여 상태 값 `CHECKING_OUT` 으로 변경

3. REST API 모델에 적합하게 하기
  * 카트 결제하기 > 주문 생성하기(목표 변경)
  * `<POST> /orders` (주문을 생성한다)

> 사용자 편의성과 규칙 준수 사이에서 균형을 잡아야 함

{:note}
> HTTP 메서드 `POST`는 유즈케이스에 적합한 다른 메서드가 없을 때 기본으로 사용하는 메서드

--- 

# `Restful` 하기 위한 조건

1. `클라이언트 / 서버 분리` : 모바일 어플리케이션의 동작 방식과 API 제공 서버의 동작 방식 분리
2. `Stateless` : 클라이언트의 어떤 컨텍스트도 서버의 세션에 저장하지 않는다
3. `캐시 가능성` : 리스폰스는 저장 가능 여부(클라이언트가 재사용할 수 있도록) 및 기간 표시
4. `코드 온 디멘드` : 서버는 필요 시 클라이언트에 선택적으로 실행 가능한 코드 전송
5. `레이어드 시스템` : 클라이언트는 서버와 상호작용 시 오직 서버만 알고, 인프라스트럭처는 숨겨야함


