---
title: Restful 하게 Url 만들기
layout: default
nav_order: 4.5
has_children: true
parent: 공부 기록
---
# 메서드 URL 설정에 대한 어려움

평소 프로젝트를 하면 엔드포인트에 신경을 쓰지 않는 일이 많았다. 코드양이 조금만 늘어나면  
엔드포인트가 섞이고 겹치는 일이 발생했고, 오류의 원인이 되는 일도 많았다.  
이런 일이 자주 발생하면서 어떻게 하면 Restful한 엔드포인트를 만들 수 있을까 고민이 많았다.
---
# 해결책

> 마이크로서비스 책을 공부 하던 중, 위와 같은 문제에 명쾌한 해답을 주는 예시가 나와있었다.

--- 
# 예시
```java
package com.optimagrowth.licensingservice.controller;

import com.optimagrowth.licensingservice.model.License;
import com.optimagrowth.licensingservice.service.LicenseService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * Please explain the class!!
 *
 * @author : sebin
 * @fileName : LicenseController
 * @since : 10/27/23
 */
@RequestMapping("value=v1/organization/{organizationId}/license")
@RestController
@RequiredArgsConstructor
public class LicenseController {
  private final LicenseService licenseService;

  @GetMapping("/{licenseId}")
  public ResponseEntity<License> getLicense(
      @PathVariable("organizationId") String organizationId,
      @PathVariable("licenseId") String licenseId
  ) {
    License license = licenseService.getLicense(licenseId, organizationId);

    return ResponseEntity.ok(license);
  }

  @PutMapping
  public ResponseEntity<String> updateLicense(
      @PathVariable("organizationId") String organizationId,
      @RequestBody License request
  ) {
    return ResponseEntity.ok(licenseService.updateLicense(request, organizationId));
  }

  @PostMapping
  public ResponseEntity<String> createLicense(
      @PathVariable("organizationId") String organizationId,
      @RequestBody License request
  ) {
    return ResponseEntity.ok(licenseService.createLicense(request, organizationId));
  }

  @DeleteMapping(value = "/{licenseId}")
  public ResponseEntity<String> deleteLicense(
      @PathVariable("organizationId") String organizationId,
      @PathVariable("licenseId")
  )
}

```

> - 조회 : GET/조회대상의기본키Id
> - 생성: Post || 생성 관련 정보 파라미터로 전달받기
> - 삭제 : Delete/조회대상의기본키Id
> - 업데이트 : PUT || 업데이트 관련 정보(업데이트 대상 기본키 등 정보 포함) 파라미터로 전달받기