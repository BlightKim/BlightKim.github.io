---
title: Flyway로 스프링부트 DB 이력 관리하기
parent: 백엔드
layout: default
---

* flyway 란 ?

> Flyway는 데이터베이스 마이그레이션을 쉽게 추적하고,
>
> 필요한 변경 사항을 자동으로 적용하거나 롤백할 수 있도록 해준다.

* flyway 라이브러리 추가

```groovy
dependencies {
    // ... 이하 생략 ...
    
    // flyway 추가
    implementation 'org.flywaydb:flyway-core'
    // flyway - mysql
    implementation 'org.flywaydb:flyway-mysql'
}
```

* flyway sql 저장 디렉토리 생성

경로는 디폴트로 `src/main/resources/db/migration`

- **sql 파일명 규칙**

1. Prefix : V, U, R 중 하나 (일반적으로 V 사용)
2. Version : 버전정보 ( 정수/소수/날짜 가능 )
3. Separator : 언더바 2개 __
4. Description : 파일 설명

파일명 예시

- `V1__init_table.sql`
- `V20220104__insert_data.sql`

설정 파일 추가

```yaml
# application.yaml
  flyway:
    # Flyway 활성화
    enabled: true
    # 기존 데이터베이스가 있을 때, 첫 번째 마이그레이션을 기준으로 설정하여, 스키마를 최신 상태로 간주하게 합니다. 이미 스키마가 존재하는 경우 유용합니다.
    baseline-on-migrate: true
    # 베이스라인 버전을 1로 설정합니다. 이는 Flyway가 마이그레이션을 시작할 기준 버전입니다.
    baseline-version: 1
    # 지정된 위치에 마이그레이션 파일이 없는 경우 실패하게 합니다. 이를 통해 마이그레이션 파일의 위치 누락 문제를 감지할 수 있습니다.
    fail-on-missing-locations: true   
```

---

### 주의 사항

- 마이그레이션을 sql 파일은 수정하면 안된다(에러 발생)
- DB 변경사항으로 인해 수정 작업 시 버전업을 한 sql 파일을 생성 및 적용
