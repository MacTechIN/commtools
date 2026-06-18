# 시스템 아키텍처

## 1. 기술 스택

| 영역 | 후보/선정 기술 | 비고 |
| --- | --- | --- |
| Frontend | Next.js + React + TypeScript | App Router 구조 기준 |
| Backend | Next.js Route Handlers | `/api/*` 엔드포인트 제공 |
| Database | 미정 | MVP 초기에는 repository 경계로 교체 가능하게 설계 |
| Authentication | 미정 | `src/lib/auth.ts` 경계 먼저 정의 |
| Styling | CSS Modules 또는 Tailwind CSS 후보 | Figma 디자인 토큰 확정 후 결정 |
| Hosting | 미정 | Vercel, Cloudflare, 자체 서버 중 선택 가능 |
| CI/CD | GitHub Actions 후보 | 테스트/빌드 자동화 |

## 2. 구조 개요

```text
Browser
  -> Next.js App Router pages
    -> React components
      -> feature hooks and API client
        -> /api route handlers
          -> service layer
            -> repository layer
              -> database or storage
```

## 3. 주요 모듈

| 모듈 | 책임 | 입력 | 출력 |
| --- | --- | --- | --- |
| `src/app/*` | 화면 라우팅과 페이지 구성 | URL, 사용자 액션 | 페이지 UI |
| `src/components/*` | 재사용 UI 컴포넌트 | props, 상태값 | 렌더링된 UI |
| `src/features/items/itemApi.ts` | 클라이언트 API 호출 | itemId, itemInput, query | API 응답 데이터 |
| `src/features/items/useItems.ts` | 화면 상태 관리 | 사용자 액션 | items, isLoading, errorMessage |
| `src/features/items/itemService.ts` | 비즈니스 규칙 | currentUser, input | 처리 결과 |
| `src/features/items/itemRepository.ts` | 데이터 저장소 접근 | 조회/저장 조건 | 저장 데이터 |
| `src/lib/apiResponse.ts` | API 응답 포맷 | data 또는 error | 표준 JSON 응답 |
| `src/lib/auth.ts` | 인증/권한 확인 | request, action, resource | currentUser 또는 권한 결과 |
| `src/lib/validation.ts` | 요청값 검증 | schema, input | validatedInput 또는 오류 |

## 4. 설계 원칙

- MVP에서는 단순한 구조를 우선합니다.
- 도메인 로직과 UI 로직을 분리합니다.
- API 응답 형식은 공통 유틸리티로 통일합니다.
- 데이터 저장소 접근은 repository에 모아 DB 변경에 대비합니다.
- 인증 구현은 `auth.ts` 경계에 모아 교체 가능하게 유지합니다.
- 보안 정보는 코드에 저장하지 않습니다.

## 5. 보안 고려사항

- 인증 토큰 저장 위치: 인증 방식 확정 후 결정합니다.
- 민감 정보 암호화: 환경 변수와 secret store 사용을 기본으로 합니다.
- 권한 검증 위치: API route 진입 시 `requireUser`, service layer에서 `hasPermission`으로 검증합니다.
- 요청 제한/남용 방지: 운영 배포 전 rate limit 또는 플랫폼 보호 기능을 검토합니다.

## 6. 확장 고려사항

- 사용자 증가 대응: 목록 API에 pagination, search, filter를 유지합니다.
- 데이터 증가 대응: DB index와 서버 측 pagination을 고려합니다.
- 기능 확장 지점: `features/items` 구조를 실제 도메인별 feature 폴더로 확장합니다.
- 외부 연동 대응: 외부 API는 service 또는 adapter 경계로 분리합니다.

## 7. 기술 결정 기록

| 날짜 | 결정 | 이유 | 대안 |
| --- | --- | --- | --- |
| 2026-06-18 | MVP 도메인을 `Item`으로 임시 정의 | 실제 도메인 확정 전에도 UI/API/데이터 구조 설계를 진행하기 위함 | 실제 도메인 확정까지 문서 작업 보류 |
| 2026-06-18 | service/repository 계층 분리 | DB와 비즈니스 규칙 변경 범위를 줄이기 위함 | route handler에 직접 로직 작성 |
