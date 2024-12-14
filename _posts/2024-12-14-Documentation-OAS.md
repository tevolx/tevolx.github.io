---
title: "[Documentation] Swagger와 OpenAPI"
date: 2024-12-14 12:00:00 +/-TTTT
categories: [Documentation]
tags: [Documentation,Swagger,OpenAPI]
---

### **Swagger & OpenAPI**
`Swagger 2.0`과  `OpenAPI 3.0`은 기계와 사람 모두가 읽을 수 있는 YAML 또는 JSON 형식 문서를 이용하여 RESTful HTTP API를 설명하는 API 사양(Specification)으로, `OpenAPI 3.0`은 `Swagger 2.0`의 최신 버전이며 서로 다른 사양은 아니다.

<br>

### **역사**

##### 2011년
Wordnik dictionary 팀은 `Swagger UI`, `Swagger Codegen` 등을 포함하는 `Swagger toolkit`의 일부로 `Swagger Specification`의 첫 버전을 만들었다.

##### 2014년
`Swagger toolkit`의 두번째 버전이 `Swagger 2.0 Specification`과 함께 출시되었다. 버전 2에는 JSON과 YAML에서 `Swagger 2.0` 문서 편집을 지원하는 `Swagger Editor`가 포함되었다. 개발자가 API에 대한 설명을 위해 `Swagger 2.0 Specification`을 사용하는 것이 이 시점부터 인기를 얻기 시작하였다.

##### 2015년
`Swagger 2.0 Specification`이 `Linux Foundation`에서 운영되는 `OpenAPI Initiative`에 기증된다.

##### 2017년
새로운 버전의 사양이 `OpenAPI Specification 3.0` 이라는 이름으로 출시되었다. RESTful HTTP API를 설명하는 것을 목표로 하는 다른 Specification이 있지만 사실상 `OpenAPI`는 오늘날 업계 표준이 되었다.

<br>

위 역사에서 볼수 있듯이 `Swagger Specification` 이 `OpenAPI Initiative`에 기증되면서 그 명칭이 `OpenAPI Specification`으로 변경되었고, 명칭 변경 이후 출시된 버전이 `OpenAPI Specification 3.0` 인 것이다. 본 포스트 게시일 기준 최신 버전은 [`OpenAPI Specification 3.1`](https://spec.openapis.org/oas/v3.1.0.html) 이다.

<br>

### **OpenAPI Tooling**

`Swagger Specification` 이 `OpenAPI Specification`으로 명칭이 변경되긴 하였지만, `Swagger`라는 명칭은 `OpenAPI Specification`을 구현하기 위한 도구를 표현하기 위해 쓰이고 있다. 해당 도구에는 위에서 언급된 `Swagger Editor`, `Swagger UI`, `Swagger Codegen` 등이 포함되어 있다.

`Swagger`에 포함된 도구들외에도 다른 [오픈 소스 및 도구들](https://tools.openapis.org/)이 공개 되어 있어 `OpenAPI Specification`을 사용하여 API를 생성하기 위해 다른 도구를 사용할수 있다.

<br>

---

<br>

##### Reference
- [openapi-vs-swagger](https://blog.postman.com/openapi-vs-swagger/)
- [difference-between-swagger-and-openapi](https://swagger.io/blog/api-strategy/difference-between-swagger-and-openapi/)
