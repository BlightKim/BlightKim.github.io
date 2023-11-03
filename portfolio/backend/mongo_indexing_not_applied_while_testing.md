---
title: MongoDB 테스트컨테이너에서 Indexing 안되는 문제
layout: default
nav_order: 4.5
has_children: true
parent: 공부 기록
---
# 문제 발생

테스트 컨테이너를 활용하여 MongoDB를 테스트 중에 entity에 Index가 적용되지 않는 문제가 발생했다.  
entity에도 @Indexing(unique="true") 라고 명시를 했음에도 적용되지 않았다.

---
## ProductEntity 클래스

```java
package se.magnus.microservices.core.product.persistence;

import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.Version;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.mapping.Document;

@Document(collection = "products")
public class ProductEntity {

  @Id private String id;

  @Version private Integer version;

  @Indexed(unique = true)
  private int productId;

  private String name;
  private int weight;

  public ProductEntity() {}

  public ProductEntity(int productId, String name, int weight) {
    this.productId = productId;
    this.name = name;
    this.weight = weight;
  }

  public String getId() {
    return id;
  }

  public void setId(String id) {
    this.id = id;
  }

  public Integer getVersion() {
    return version;
  }

  public void setVersion(Integer version) {
    this.version = version;
  }

  public int getProductId() {
    return productId;
  }

  public void setProductId(int productId) {
    this.productId = productId;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getWeight() {
    return weight;
  }

  public void setWeight(int weight) {
    this.weight = weight;
  }
}
```

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
## PersistenceTests 클래스
```java

@DataMongoTest
class PersistenceTests extends MongoDbTestBase{

  @Autowired
  private ProductRepository repository;

  private ProductEntity savedEntity;

  @BeforeEach
  void setupDb() {
    repository.deleteAll();

    ProductEntity entity = new ProductEntity(1, "n", 1);
    savedEntity = repository.save(entity);

    assertEqualsProduct(entity, savedEntity);
  }

  @Test
  void duplicateError() {
      ProductEntity entity = new ProductEntity(savedEntity.getProductId(), "n", 1);
      repository.save(entity);
  }

  private void assertEqualsProduct(ProductEntity expectedEntity, ProductEntity actualEntity) {
    assertEquals(expectedEntity.getId(), actualEntity.getId());
    assertEquals(expectedEntity.getVersion(), actualEntity.getVersion());
    assertEquals(expectedEntity.getProductId(), actualEntity.getProductId());
    assertEquals(expectedEntity.getName(), actualEntity.getName());
    assertEquals(expectedEntity.getWeight(), actualEntity.getWeight());
  }
}

```
---

# 발생 원인

MongoDB는 3.0 이후부터 컬렉션 라이프 사이클과 퍼포먼스에 영향을 주는 것을 막기 위해,   
기본 설정으로 Auto-index creation이 disable 되었다고 한다.
---

# 문제해결
> 따라서 문제 해결을 위해서는 test/resources에 application.yml 설정파일을 생성하고
> ```yaml
> spring:
>   data:
>     mongodb:
>       auto-index-creation: true
> ```
> 다음과 같이 입력해줘야 한다.

---
# 결과
```java
org.springframework.dao.DuplicateKeyException: Write operation error on server localhost:52296. Write error: WriteError{code=11000, message='E11000 duplicate key error collection: test.products index: productId dup key: { productId: 1 }', details={}}.

	at org.springframework.data.mongodb.core.MongoExceptionTranslator.translateExceptionIfPossible(MongoExceptionTranslator.java:102)
	at org.springframework.data.mongodb.core.MongoTemplate.potentiallyConvertRuntimeException(MongoTemplate.java:2808)
	at org.springframework.data.mongodb.core.MongoTemplate.execute(MongoTemplate.java:574)
	at org.springframework.data.mongodb.core.MongoTemplate.insertDocument(MongoTemplate.java:1450)
	at org.springframework.data.mongodb.core.MongoTemplate.doInsert(MongoTemplate.java:1246)
	at org.springframework.data.mongodb.core.MongoTemplate.insert(MongoTemplate.java:1172)
```

> 정상적으로 Exception이 출력된다.
