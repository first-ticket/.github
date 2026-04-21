---
name: "▶️ test"
about: 테스트 코드 작성
title: "[test][서비스명]"
labels: fix
assignees: ''

---

## 📌 개요
<!--
  "테스트가 없어서"는 충분한 이유입니다. 
  다만 어떤 기능의 어떤 케이스를 검증하려는지는 명확히 적어주세요.
  예) HostRequestStatus 상태 전이 로직이 도메인으로 이동되었으나
      유효/무효 전이 케이스에 대한 단위 테스트가 없는 상태입니다.
-->
- 테스트를 추가하거나 개선하는 이유
- 테스트 대상 기능 설명

## 🧩 테스트 범위
- [ ] 통합 테스트
- [ ] Service 단위 테스트
- [ ] Repository 테스트
- [ ] 외부 API Mock 테스트
- [ ] 예외 케이스 검증

## 🧪 기타
<!--
  정상 케이스만큼 예외/경계값 케이스가 중요합니다.
  given / when / then 형식으로 작성하면 구현 시 바로 코드로 옮기기 쉽습니다.
  예) given: 이미 APPROVED 상태인 HostRequest
      when : reject() 호출
      then : InvalidStatusTransitionException 발생
-->
- 추가 설명이 필요하면 작성해주세요
