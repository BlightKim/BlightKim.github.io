---
title: Java Record에 대해 알아보자
layout: default
nav_order: 4.5
has_children: true
parent: 공부 기록
---
# Record란 ?
> 스프링에서 Configuration Property를 설정하면서 Record에 대해 알게되어 정리해본다.  
> Record는 불변 데이터를 보유하기 위한 특수한 종류의 클래스이며 java 14부터 추가되었다.  
> 기존의 getter / setter가 뒤섞여 난잡해진 코드를 간결하게 만들 수 있다.  
> Entity로 만들 수 있을 까 했지만 Entity는 프록시 등을 생성해야 하므로 Entity로 만들 수 없다
  
---

# 비교
- Record 미사용
  
```java
package se.magnus.microservices.composite.product.config;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.bind.ConstructorBinding;

@ConfigurationProperties(prefix = "api.common")
@ConstructorBinding
public class OpenApiConfiguration {

    private final String version;
    private final String title;
    private final String description;
    private final String termsOfService;
    private final String license;
    private final String licenseUrl;
    private final String externalDocDesc;
    private final String externalDocUrl;
    private final Contact contact;

    public OpenApiConfiguration(String version, String title, String description,
                                String termsOfService, String license, String licenseUrl,
                                String externalDocDesc, String externalDocUrl, Contact contact) {
        this.version = version;
        this.title = title;
        this.description = description;
        this.termsOfService = termsOfService;
        this.license = license;
        this.licenseUrl = licenseUrl;
        this.externalDocDesc = externalDocDesc;
        this.externalDocUrl = externalDocUrl;
        this.contact = contact;
    }

    // Getter 메소드


    public static class Contact {
        private final String name;
        private final String url;
        private final String email;

        public Contact(String name, String url, String email) {
            this.name = name;
            this.url = url;
            this.email = email;
        }

        // Getter 메소드

    }
}
```

- Record 사용
  

```java
package se.magnus.microservices.composite.product.config;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.bind.ConstructorBinding;
import org.springframework.validation.annotation.Validated;

@ConfigurationProperties(prefix = "api.common")
public record OpenApiConfiguration(String version, String title, String description,
                                   String termsOfService, String license, String licenseUrl,
                                   String externalDocDesc, String externalDocUrl, Contact contact) {


  public record Contact(String name, String url, String email) {

  }
} 
```
---
{: .important-title }
> 결론
>
> Entity를 제외한 불변 데이터 객체를 만들 때는 Record를 사용해보자  
> 코드의 양이 획기적으로 줄어든다.
