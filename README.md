![Image](https://github.com/user-attachments/assets/02e1be0a-19c9-44f4-9c22-52e76f718a8e)  

CI/CD 파이프라인을 자동화하고, 무중단 배포 전략을 적용하며, 토큰 인증 방식을 결합하여 안전한 인증 및 인가 보안 시스템을 제공한다.

<br>

## Development
- Nginx를 활용한 Blue/Green 방식의 전략적 무중단 배포 구축
- GitHub Actions를 활용한 CI/CD 파이프라인 구축
- Spring Security와 RESTful API를 활용한 JWT 인증 시스템
  - Access Token과 Refresh Token을 사용한 JWT 보안 시스템 구현
  - Refresh Token Rotation + Redis 구현
  - Blacklist와 ReplayAttack 감지 구현

<br>

## 개발일지
<ul>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/project-planning-and-design" target="_blank">1 - 프로젝트 기획 및 설계</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/ci-cd-design/" target="_blank">2 - CI/CD 설계</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/private-rds-issue/" target="_blank">3 - private RDS 접근하기</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/overview-of-spring-security-structure/" target="_blank">4 - Spring Security 구조, 흐름</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/secret-key-and-jwt-issuance-test/" target="_blank">5 - jwt 암호화와 secret key 생성 방법</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/why-did-i-use-jwt-tokens/" target="_blank">6 - 왜 JWT 토큰을 사용했을까?</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/access-token-and-refresh-token/" target="_blank">7 - Access Token과 Refresh Token</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/access-token-and-refresh-token-flow/" target="_blank">8 - Access Token과 Refresh Token 동작 흐름</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/refresh-token-rotation-blacklist-implementation-replay-attack-detection-system" target="_blank">9 -Refresh Token Rotation, Blacklist, ReplayAttack 감지 시스템 구현</a>
  </li>
  <li>
    <a href="https://songbyhyeok.github.io/ci-cd-zero-downtime-auth/building-zero-downtime-deployment-with-nginx" target="_blank">10 - Nginx를 활용한 무중단 배포 구축기</a>
  </li>
</ul>

<br>

## Structure Diagram
<img src="/images/2025-03-09-ci-cd-design-1.png" alt="empty"
                    style="width: 80%; height: auto;">
<img src="/images/zero-downtime-deployment.png" alt="empty"
                    style="width: 60%; height: auto;">
<img src="/images/2025-03-23-overview of spring security structure-2.png" alt="empty"
                    style="width: 100%; height: auto;">

<br>

## Flowchart Diagram
<img src="/images/2025-04-07-implementation of jwt security system using access token and refresh token - 1.png" alt="empty"
                    style="width: 100%; height: auto;">

<br>

## 기술 스택
### Back-end
<table border="1">
  <tr>
    <td>Java 17(Gradle)</td>
    <td>Spring(Boot, Security, JPA)</td>
    <td>CI/CD(GitHub Actions, AWS(EC2, RDS), Ubuntu, Docker, Redis, Nginx)</td>
    <td>MySQL 8.0</td>
    <td>RESTful API</td>
    <td>Postman</td>
  </tr>
</table>

<br>

## Directory Structure
```
+---main
|   +---java
|   |   \---com
|   |       \---project
|   |           \---cicd_auth
|   |               |   CicdAuthApplication.java
|   |               |
|   |               +---config
|   |               |       DataSourceConfig.java
|   |               |       RedisConfig.java
|   |               |       SecurityConfig.java
|   |               |       SshTunnelingConfig.java
|   |               |
|   |               +---controller
|   |               |       AdminController.java
|   |               |       HealthCheckController.java
|   |               |       JoinController.java
|   |               |       MainController.java
|   |               |       ReissueController.java
|   |               |
|   |               +---dto
|   |               |       CustomUserDetails.java
|   |               |       JoinDTO.java
|   |               |
|   |               +---entity
|   |               |       Member.java
|   |               |       RefreshEntity.java
|   |               |
|   |               +---jwt
|   |               |       CustomLogoutFilter.java
|   |               |       JWTFilter.java
|   |               |       JWTUtil.java
|   |               |       LoginFilter.java
|   |               |
|   |               +---repository
|   |               |       MemberRepository.java
|   |               |       RefreshRepository.java
|   |               |
|   |               +---service
|   |               |       CustomUserDetailsService.java
|   |               |       JoinService.java
|   |               |       ReissueService.java
|   |               |
|   |               \---utils
|   |                       CookieUtils.java
|   |                       RedisUtil.java
|   |
|   \---resources
|           application-dev.yml
|           application-prod.yml
|           application.yml
|
\---test
    \---java
        \---com
            \---project
                \---cicd_auth
                        CicdAuthApplicationTests.java
```

<br>

## 규칙
### branch rule
<table border="1">
  <thead>
    <tr>
      <th>Type</b></th>
      <th>Description</th>      
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>main</td>
      <td>최종 배포 버전의 코드가 유지되는 브랜치</td>            
    </tr>
    <tr>
      <td>feature</td>
      <td>새로운 기능 개발을 위한 독립적인 브랜치로, 개발 후 main에 병합</td>            
    </tr>
  </tbody>
</table>

### commit rule
<table border="1">
  <thead>
    <tr>
      <th>Type</b></th>
      <th>Description</th>      
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>feature</b></td>
      <td>새로운 기능 추가</td>      
    </tr>
    <tr>
      <td><b>fix</b></td>
      <td>버그 수정</td>      
    </tr>
    <tr>
      <td><b>refactor</b></td>
      <td>코드 리팩토링</td>      
    </tr>
    <tr>
      <td><b>test</b></td>
      <td>테스트 코드</td>      
    </tr>
  </tbody>
</table>

### github issue rule
![image](https://github.com/user-attachments/assets/15aa6f57-a2d2-4f4b-b6f1-a2d9ef80f39c)  

* **GitHub Flow** <br>
이 전략은 브랜치 구조와 규칙이 직관적이고 간단하여 소규모 개인 사이드 프로젝트에 적합하다. 또한, PR 방식의 자동화 시스템이 release 브랜치 역할을 대체할 수 있어, CI/CD를 활용한 자동화된 배포와 결합하면 더 유연하고 효율적인 개발이 가능하다.



