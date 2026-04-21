# first-ticket / .github

> **First-ticket** Organization의 GitHub 설정 및 이슈·PR 템플릿을 중앙 관리하는 레포지토리입니다.
> 이 repository에 등록된 템플릿은 **Organization 내 모든 서비스 레포에 자동 적용**됩니다.

---

## 📁 디렉토리 구조

```
## first-ticket Organization/.github repository 내부 구조

.github/
├── PULL_REQUEST_TEMPLATE.md     # PR 생성 시 자동 적용되는 템플릿
└── ISSUE_TEMPLATE/
    ├── feat.md                  # 새로운 기능 추가
    ├── fix.md                   # 버그 수정
    ├── refactor.md              # 리팩토링
    ├── docs.md                  # 문서 작업
    ├── chore.md                 # 빌드 설정, 의존성 업데이트
    └── test.md                  # 테스트 코드
```

---

## 🚀 사용 방법

별도 설정 없이 **이슈 또는 PR을 생성하면 자동으로 적용**됩니다.

⚠️ 주의 : 각 Microservice 안에 템플릿을 생성하면 공통 설정이 적용되지 않을 수 있습니다. 관련 문제 있을시 담당자에게 문의 바랍니다.

### 이슈 생성

1. 본인 서비스 repo → **Issues** 탭 → `New issue` 클릭
2. 작업 유형에 맞는 템플릿 선택
3. 항목을 작성 후 제출

> 이슈 제목 형식: `[유형][서비스명] 작업 내용`<br>
> ex) `[feat][user] 회원가입 API 구현`, `[fix][payment] 중복 결제 방지 로직 수정`

### PR 생성

1. `feature` → `develop` 브랜치로 PR 생성
2. 본문이 템플릿으로 채워짐 → 각 항목 수정 및 작성
3. Assignee(담당자)와 Label(라벨)을 반드시 지정!

> **타 서비스에 영향을 주는 변경**(Kafka 이벤트 구조, API 계약, X-User-Id 헤더 등)은
> PR의 `📚 추가 설명` 섹션에 반드시 명시하고 관련 담당자를 Reviewer로 지정해 주세요.

---

## 🏷️ 라벨 목록

이슈 템플릿의 자동 라벨 부착을 위해 아래 라벨이 레포별로 사전 등록되어 있어야 한다.

| 라벨 | 설명 |
|---|---|
| `💻 feature` | 새로운 기능 추가 |
| `🛠️ fix` | 버그 수정, 핫픽스 |
| `♻️ refactor` | 리팩토링 |
| `🔧 chore` | 빌드 설정, 의존성 업데이트 등 기타 작업|
| `📜 documentation` | 문서 작업 |
| `▶️ test` | 테스트 코드 추가/수정 |

> 새 서비스 레포 생성 후 Action에서 Sync Labels to All Repos Workflow를 실행하면 해당 레포와 라벨이 동기화됩니다. </br>
> 라벨 동기화 액션 링크 : https://github.com/first-ticket/.github/actions

---

## 🗂️ 서비스 레포 목록

| 서비스 | 레포 |
|---|---|
| User / Auth | [user-service](#) |
| Program / Venue | [program-service](#) |
| Queue | [queue-service](#) |
| Booking | [booking-service](#) |
| Seat | [seat-service](#) |
| Payment | [payment-service](#) |

> repo 링크는 생성 후 업데이트 해주세요.
> 새로운 repo 추가시 readme도 업데이트합니다.

---

## 🔍 리뷰 코멘트 규칙

| 태그 | 의미 | 머지 블로킹 여부 |
|---|---|---|
| `[must]` | 반드시 수정 필요 | ✅ 블로킹 |
| `[nit]` | 선택적 개선 제안 (nitpick) | ❌ 비블로킹 |

예)
- `[must] 이 비즈니스 로직은 Service가 아닌 Entity로 이동해야 합니다.`
- `[nit] 이 방식으로 변경하면 어떨까요?`
