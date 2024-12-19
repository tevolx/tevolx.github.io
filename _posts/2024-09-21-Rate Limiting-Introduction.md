---
title: "[Rate Limiting] Introduction"
date: 2024-09-22 12:00:00 +/-TTTT
categories: [Rate Limiting]
tags: [Rate Limiting]
---
### **정의**
Rate Limiting이란 서비스의 가용성을 유지하기 위해 네트워크, 서버, 혹은 기타 리소스에 대한 요청을 제어하는 기술이다.

<br>

### **사용 이유**
1. **Brute Force 공격, DDoS 공격등 과도한 트래픽으로부터 서비스를 보호**하기 위해 사용한다. 서버에는 일정 가용량이 존재하는데, 이를 넘어서면 과부하 상태가 되어 서버에 성능 저하가 오거나 다운이 될수 있다. 이렇게 과도한 트래픽을 제한하고 서비스의 안정성과 신뢰성을 보장하기 위한 전략으로 사용한다. 
2. **Resource 사용을 효과적으로 제어**하기 위해 사용한다. 특정 사용자나 그룹에 요청량이 많을시 해당 사용자가 서비스 자원을 독점할 수 있는데 이를 방지하고 리소스의 무분별한 사용과 남용을 막아 서비스 비용내에서 안정적으로 서비스하기 위해 사용한다.
3. **Rate Limit 정책을 수립하여 Rate에 대한 과금을 부과하는 비즈니스 모델로 활용하기 위해 사용**한다. 사용량별로 과금 정책을 수립하여 사용량에 따른 과금을 부과할수 있다. 한가지 예시로 [OpenAI Rate Limiting](https://platform.openai.com/docs/guides/rate-limits) 정책을 살펴보면 티어별로 과금 정책이 다른것을 확인해볼수 있다. 

<br>

### **구현**
#### 1. 알고리즘
Rate Limit을 위한 알고리즘은 아래와 같이 5가지로 분류할수 있는데, 서비스의 사용 및 트래픽 패턴에 따라 혹은 수립된 비즈니스 모델에 따라 적절한 알고리즘 선택이 필요하다.
- Leaky Bucket Algorithm
- Token Bucket Algorithm
- Fixed Window Counter
- Sliding Window Log
- Sliding Window Counter

#### 2. 구현 방식 
`Spring Cloud Gateway`나 `Nginx`에서 제공해주는 기능을 사용하거나 `Bucket4J`나 `Resillience4J`와 같은 라이브러리를 사용하여 구현할수 있다. 프레임워크나 라이브러리에서 제공해주는 기능에는 Rate Limit 알고리즘이 정해진 경우가 있어 이를 이용하여 서비스 정책에 맞는 Rate Limit 구현이 어려우면 커스텀하게 구현하는 방식을 선택할수 있을것 같다.
- [Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway/gatewayfilter-factories/requestratelimiter-factory.html)
- [Nginx](https://blog.nginx.org/blog/rate-limiting-nginx)
- [Bucket4J](https://www.baeldung.com/spring-bucket4j)
- [Resillience4J](https://resilience4j.readme.io/docs/ratelimiter)
- 커스텀하게 구현

#### 3. 고려사항
- 분산환경에서의 구성
: 분산환경에서는 Rate Limit시 데이터의 Inconsistency에 대한 문제가 있을수 있는데, 이를 해결하기 위해 `Redis`와 같은 Centralized Data Storage를 사용할 수 있다.
: 또한 분산환경에서 Rate Limit시 Race Condition과 같은 문제가 발생할수 있다. Centralized Data Storage로 `Redis`를 사용하는 경우, 이를 해결하기 위해 [`Lua Script`](https://redis.io/docs/latest/develop/interact/programmability/eval-intro/)를 활용할수 있다. `Redis`에 내장된 `Lua Script Engine`을 이용하여 서버에서 `Lua Script`를 실행할수 있는데, `Lua Script`로 작성한 로직을 실행하면 해당 연산에 대해 원자성이 보장되어 Race Condition 문제를 해결할 수 있다. `Spring Cloud Gateway`에서 Rate Limit이 구현된 방식을 살펴보면 [Lua Script를 활용](https://github.com/spring-cloud/spring-cloud-gateway/blob/main/spring-cloud-gateway-server/src/main/resources/META-INF/scripts/request_rate_limiter.lua)했음을 알 수 있다.
- Rate Limit 초과시 처리 및 응답 헤더 구성
: Rate Limit이 초과된 경우 보통 [429 Too Many Request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) 에러를 반환해주게 되는데, [`Github`](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api?apiVersion=2022-11-28)와 같이 경우에 따라서는 [403 Forbidden](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403) 에러를 내려주는 경우도 있는것으로 보인다. 에러를 내려주지 않고 [Queueing 등을 하는 경우](https://blogs.halodoc.io/taming-the-traffic-redis-and-lua-powered-sliding-window-rate-limiter-in-action/)도 있는 것으로 보이는데 서비스 정책에 맞게 초과된 경우에 대해 처리를 하면 될것 같다.
: 또한 Rate limit이 적용된 API에 대해서는 응답 헤더에 최대 사용량, 현재까지 사용량, 사용량 초기화까지 시간 등의 정보를 추가해줄수 있다. 관련하여 [`OpenAI`](https://platform.openai.com/docs/guides/rate-limits/rate-limits-in-headers), [`Meta`](https://developers.facebook.com/docs/graph-api/overview/rate-limiting/#--) 등에서 응답하는 예시를 살펴보면 좋을것 같다.

<br>

---

<br>

##### Reference
- [everything-you-need-to-know-about-rate-limiting-for-apis](https://medium.com/@bijit211987/everything-you-need-to-know-about-rate-limiting-for-apis-f236d2adcfff)
- [about-rate-limit-algorithm](https://www.mimul.com/blog/about-rate-limit-algorithm/)
- [ratelimit-algorithms](https://smudge.ai/blog/ratelimit-algorithms)
- [ratelimit이란?](https://etloveguitar.tistory.com/126)
