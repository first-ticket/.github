<div align="center">
  
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/13385620-923f-4f30-9fe4-03b16d246917" />

# 🎫 First Ticket

**공정성 · 안정성 · 신뢰성을 갖춘 MSA 기반 공연 티켓 예매 플랫폼**

좋아하는 아티스트의 티켓팅, 정각에 접속했는데 이미 매진된 경험.  
First Ticket은 **매크로 없이, 정각 접속자 모두가 동등하게 경쟁하는** 티켓팅을 목표로 설계되었습니다.

<br/>

개발 기간: 2026.04.16 ~ 2026.05.20

[![Repositories](https://img.shields.io/badge/Repositories-View-181717?style=for-the-badge&logo=github)](https://github.com/orgs/first-ticket/repositories)

</div>

---

## 📑 목차

- [💎 핵심 가치](#-핵심-가치)
- [🎯 주요 기능](#-주요-기능)
- [🏛️ 설계 원칙](#️-설계-원칙)
- [👥 팀원](#-팀원-contributors)
- [🛠️ 기술 스택](#️-기술-스택)
- [🧩 서비스 구성](#-서비스-구성-repositories)
- [🏗️ 아키텍처](#️-아키텍처)
- [🚀 Local 실행 가이드](#-local-실행-가이드)

---

## 💎 핵심 가치

| | |
|:---|:---|
| ⚖️ **공정성** | 매크로 없이, 정각 접속자 모두가 동등하게 경쟁 |
| 🛡️ **안정성** | 동시 접속 및 요청 폭증 상황에서도 서버 다운 없이 유지 |
| 🔒 **신뢰성** | 결제 실패 · 취소 시에도 예매와 좌석이 자동으로 복원 |

## 🎯 주요 기능

## 🏛️ 설계 원칙

| 영역 | 적용 방식 |
|---|---|
| **MSA 아키텍처** | 비즈니스 도메인 단위 서비스 분리 + DDD 적용 |
| **트래픽 처리 안정성** | nGrinder · k6 기반 부하 테스트로 동시 접속 안정 처리 검증 |
| **대기열 시스템** | Redis Sorted Set + Hash + String + Set 기반 대기열로 공정한 선착순 처리 |
| **이벤트 드리븐 통신** | Kafka + Outbox/Inbox 패턴으로 서비스 간 느슨한 결합 및 이벤트 유실 방지 |
| **동시성 제어** | Redisson 분산 락 + Redis TTL 기반 좌석 선점으로 중복 예매 차단 |
| **독립 배포** | AWS ECS Fargate + GitHub Actions 기반 서비스별 독립 CI/CD |
| **관찰성** | Prometheus · Grafana · Zipkin 통합 관찰성 + Slack 임계치 자동 알림 |

---

## 👥 팀원 (Contributors)

| 이름 | 포지션 | 주요 담당 | GitHub |
|---|---|---|---|
| **김하진** | 팀장 | 대기열 도메인, Config Server, ECS 배포 + CI/CD | [@rlaxxwls13](https://github.com/rlaxxwls13) |
| **조하은** | 팀원 | 좌석 도메인, 공통 모듈, 모니터링 | [@haeun228](https://github.com/haeun228) |
| **신단비** | 팀원 | 프로그램 도메인, 공연장 도메인 | [@sweetRainShin](https://github.com/sweetRainShin) |
| **김두리** | 팀원 | 결제 도메인 | [@DDoori](https://github.com/DDoori) |
| **나웅철** | 팀원 | 예매 도메인 | [@No-366](https://github.com/No-366) |
| **박동진** | 팀원 | 인증·인가, Gateway, Eureka | [@straycat405](https://github.com/straycat405) |

<details>
<summary>📋 상세 기여 내역 보기</summary>

#### 김하진
- 대기열 도메인 설계 및 구현 (Redis Sorted Set/Hash/역인덱스, Lua Script 원자성, FIFO tie-breaker, Adaptive Polling)
- Spring Cloud Config Server 구축 (Git + Basic Auth + `{cipher}` 암호화)
- ECS Fargate 배포 인프라 + GitHub Actions OIDC 기반 CI/CD

#### 조하은
- 좌석 도메인 (Redisson 분산락, Redis TTL Hold, JDBC batchUpdate, Redis 캐시)
- 공통 모듈 설계 (`common` / `common-jpa` / `common-messaging` — Outbox/Inbox/멱등 AOP)
- Prometheus + Grafana 모니터링 및 Slack 알림 파이프라인

#### 신단비
- 프로그램·공연장 도메인 (상태 머신, 가격등급, 시간 겹침 이중 방어 — 비관적 락 + tsrange exclusion)
- Provider 패턴으로 외부 서비스 격리, Outbox 기반 Kafka 발행 설계
- 전체 계층 테스트 코드 (Testcontainers + PostgreSQL), REST Docs 문서화

#### 김두리
- 토스페이먼츠 서버 승인 방식 연동, 결제 상태 머신 (`PENDING/FAILED/FINAL_FAILED/SUCCESS/REFUNDED`)
- 비관적 락 + Idempotency Key + 금액 대조 검증
- Outbox/Inbox 패턴 + DLQ로 결제 이벤트 유실 방지

#### 나웅철
- 예매 도메인 + Saga Orchestration (`PENDING → PAID → CONFIRMED / CANCEL_REQUESTED → CANCELED`)
- Redisson `@DistributedLock` AOP, 입장 토큰/세션 토큰 분리 검증
- QueryDSL 동적 페이지네이션, 외부 서비스 Feign Client 설계

#### 박동진
- 인증 시스템 (Keycloak 26 + JWT 이중 토큰 + Redis Lua CAS Token Rotation + Blacklist)
- API Gateway (`AuthorizationHeaderFilter`, Caffeine L1 캐시, Circuit Breaker)
- Eureka Server (Caffeine 응답 캐시, 환경별 Self-Preservation 분리)
- HOST 권한 신청/승인 Flow + Partial Unique Index 이중 방어

</details>

---

## 🛠️ 기술 스택

#### Language & Framework
![Java](https://img.shields.io/badge/Java-21-007396?style=flat-square&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5.13-6DB33F?style=flat-square&logo=springboot)
![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-2025.0.2-6DB33F?style=flat-square&logo=spring)
![Spring Data JPA](https://img.shields.io/badge/Spring_Data_JPA-6DB33F?style=flat-square&logo=spring)
![QueryDSL](https://img.shields.io/badge/QueryDSL-0769AD?style=flat-square)
![Keycloak](https://img.shields.io/badge/Keycloak-26-4D4D4D?style=flat-square&logo=keycloak)
![Resilience4j](https://img.shields.io/badge/Resilience4j-2B7CD3?style=flat-square)
![Flyway](https://img.shields.io/badge/Flyway-CC0200?style=flat-square&logo=flyway)

#### Database & Messaging
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka_(KRaft)-231F20?style=flat-square&logo=apachekafka)
![Toss Payments](https://img.shields.io/badge/Toss_PG-0064FF?style=flat-square&logo=tossPayments)

#### Test
![JUnit5](https://img.shields.io/badge/JUnit_5-25A162?style=flat-square&logo=junit5)
![nGrinder](https://img.shields.io/badge/nGrinder-2B82BD?style=flat-square)
![k6](https://img.shields.io/badge/k6-7D64FF?style=flat-square&logo=k6)
![JMeter](https://img.shields.io/badge/JMeter-D22128?style=flat-square&logo=apache)
![Testcontainers](https://img.shields.io/badge/Testcontainers-2496ED?style=flat-square&logo=docker)

#### Infra & DevOps
![AWS ECS](https://img.shields.io/badge/AWS_ECS_Fargate-FF9900?style=flat-square&logo=amazonaws)
![AWS RDS](https://img.shields.io/badge/AWS_RDS-527FFF?style=flat-square&logo=amazonrds)
![AWS ALB](https://img.shields.io/badge/AWS_ALB-FF9900?style=flat-square&logo=amazonaws)
![AWS ECR](https://img.shields.io/badge/AWS_ECR-FF9900?style=flat-square&logo=amazonaws)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

#### Monitoring
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana)
![Zipkin](https://img.shields.io/badge/Zipkin-FF6B6B?style=flat-square)

#### Collaboration
![Notion](https://img.shields.io/badge/Notion-000000?style=flat-square&logo=notion)
![Slack](https://img.shields.io/badge/Slack-4A154B?style=flat-square&logo=slack)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github)
![drawio](https://img.shields.io/badge/draw.io-F08705?style=flat-square&logo=diagrams.net)

---

## 🧩 서비스 구성 (Repositories)

> 도메인 단위 MSA. 각 서비스는 독립 배포 · 독립 확장 가능합니다.

| 서비스 | 역할 | 레포 |
|---|---|---|
| 🚪 **Gateway Server** | 단일 진입점, JWT 서명 검증, Caffeine L1 캐시, Circuit Breaker | [Gateway Server](https://github.com/first-ticket/gateway-server) |
| 🧭 **Eureka Server** | 서비스 디스커버리 | [Eureka Server](https://github.com/first-ticket/eureka-server) |
| ⚙️ **Config Server** | 중앙 집중 설정 관리 (Git + 암호화) | [Config Server](https://github.com/first-ticket/config-server) |
| 👤 **User Service** | 회원, 인증(Keycloak), Token Rotation, HOST 권한 | [User Service](https://github.com/first-ticket/user-service) |
| 🏛️ **Venue Service** | 공연장 · 구역 · 좌석 구조 관리 | [Venue Service](https://github.com/first-ticket/venue-service) |
| 🎭 **Program Service** | 공연/스케줄/가격등급 등록 및 상태 머신 | [Program Service](https://github.com/first-ticket/program-service) |
| 🎟️ **Queue Service** | Redis 기반 대기열, Adaptive Polling, 예매 입장 토큰 발급 | [Queue Service](https://github.com/first-ticket/queue-service) |
| 💺 **Booking Service** | 좌석 선점(Redisson) + 예매 Saga 오케스트레이터 | [Booking Service](https://github.com/first-ticket/booking-service) |
| 💳 **Payment Service** | Toss Payments 연동, 결제 상태 머신, Outbox | [Payment Service](https://github.com/first-ticket/payment-service) |
| 📦 **Common Modules** | `common` / `common-jpa` / `common-messaging` | [common](https://github.com/first-ticket/common), [common-jpa](https://github.com/first-ticket/common), [common-messaging](https://github.com/first-ticket/common-messaging) |

---

## 🏗️ 아키텍처

### 티켓팅 전체 흐름

```
① 대기열 진입       사용자 → Queue Service       (Redis Sorted Set + Hash)
② 입장 확인         사용자 폴링 → Queue Service   (Adaptive Polling, EntryToken JWT)
③ 예매 세션 생성    사용자 → Booking Service     (EntryToken 검증 + 블랙리스트)
④ 좌석 선점        사용자 → Booking Service     (Redisson 분산락 + TTL 10분)
⑤ 예매 생성        사용자 → Booking Service     (Saga 시작)
⑥ 결제             Booking → Payment Service    (Toss PG 서버 승인)
   └─ 성공/실패에 따라 Kafka 이벤트 발행 → 보상 트랜잭션
```

### 인증 흐름

```
클라이언트
 │  Authorization: Bearer <AccessToken>
 ▼
API Gateway
 │  Keycloak 공개키로 서명 검증
 │  검증 성공 → X-User-Id, X-User-Role 헤더 추가
 ▼
각 마이크로서비스 (헤더 신뢰, 자체 재검증 없음)
```

### 프로그램 & 공연장 흐름

주최자가 `공연장 → 프로그램(DRAFT) → 스케줄 → 가격 등급`을 순차 등록하고  
판매 시작(`ON_SALE`) 시점에 예매 서비스·대기열 서비스가 Kafka로 동기화
```
주최자 (HOST / ADMIN)
├─ ①  공연장 등록           → Venue Service
│      └─ 구역(SEATED/STANDING/FREE) 포함 시 좌석 자동 생성
│
├─ ②  프로그램 등록 (DRAFT)  → Program Service
│      └─ Kafka: program.created    → 대기열 초기화
│
├─ ③  스케줄 등록            → Program Service
│      └─ 비관적 락 + tsrange exclusion 으로 공연장 시간 겹침 차단
│
├─ ④  가격 등급 등록         → Program Service
│      └─ 좌석 타입별 sectionId 규칙 검증 (Feign)
│
└─ ⑤  판매 시작 (DRAFT → ON_SALE)
└─ Kafka: schedule.created     → 예매 서비스가 BookingSeat 생성
└─ Kafka: program.time.updated → 대기열 서비스가 openAt·closeAt 등록
```

### 인프라 설계도
<img width="8905" height="5315" alt="image" src="https://github.com/user-attachments/assets/f9042817-847b-4a90-a748-2bdd8cfbb7d8" />


### ERD
<img width="15295" height="9855" alt="image" src="https://github.com/user-attachments/assets/d47d9f6c-f3e7-4f72-99a3-fe827177ef6e" />

---

## 🚀 Local 실행 가이드

> 자세한 실행 가이드는 각 레포의 README 참조

```bash
# 1. 인프라 컴포넌트 기동 (PostgreSQL, Redis, Kafka, Keycloak)
docker compose up -d

# 2. 인프라 서비스 (순서 중요)
./gradlew :eureka-server:bootRun     # 1순위
./gradlew :config-server:bootRun     # 2순위
./gradlew :gateway-server:bootRun    # 3순위

# 3. 도메인 서비스
./gradlew :user-service:bootRun
./gradlew :program-service:bootRun
./gradlew :venue-service:bootRun
./gradlew :queue-service:bootRun
./gradlew :booking-service:bootRun
./gradlew :payment-service:bootRun
```

**환경 요구사항**
- Java 21
- Docker & Docker Compose
- Gradle 8.x

---

<div align="center">

**🎫 First Ticket** · MSA Ticketing Platform

</div>
