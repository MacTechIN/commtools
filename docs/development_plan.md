# 개발 계획

이 문서는 `commtool` 앱을 실제 코드로 구현할 때 사용할 파일명, 함수명, 변수명, 사용처, 용도를 함께 기록하는 개발 기준 문서입니다. 아직 실제 앱 정의 문서인 `docs/app_dev_def.md`가 비어 있으므로, 아래 구조는 MVP 웹앱을 기준으로 한 기본 설계안입니다. 앱 정의가 확정되면 도메인명과 기능명을 이 문서에 맞춰 갱신합니다.

## 0. 도입

`commtool` 개발은 요구사항 문서와 실제 코드 사이의 간격을 줄이는 방식으로 진행합니다. 기능을 먼저 크게 나열한 뒤 구현 중에 파일 구조를 정하는 방식은 초기에는 빠르게 보이지만, 화면, API, 데이터 처리, 검증, 테스트가 뒤섞이기 쉽습니다. 이 문서는 개발 전에 사용할 파일명, 함수명, 변수명, 사용처를 먼저 정의하여 구현 기준을 고정하는 데 목적이 있습니다.

도입 기준은 다음과 같습니다.

- 문서에 기록된 파일명은 실제 구현 시 기본 생성 대상입니다.
- 문서에 기록된 함수명은 구현 시 우선 사용하며, 변경이 필요하면 변경 이유를 문서에 남깁니다.
- 문서에 기록된 변수명은 화면, API, 서비스, 저장소 계층에서 같은 의미로 사용합니다.
- `items`는 임시 도메인명이며, 실제 앱 정의가 확정되면 전체 코드와 문서에서 일괄 교체합니다.
- API 응답, 오류 처리, 검증 방식은 공통 유틸리티로 통일합니다.
- 기능 추가는 화면부터 만들지 않고 타입, 스키마, 서비스, API, 화면 순서로 검토합니다.

개발 착수 전 반드시 확인할 문서는 다음입니다.

| 문서 | 확인 목적 | 현재 상태 |
| --- | --- | --- |
| `docs/app_dev_def.md` | 앱 목적, 사용자, 핵심 기능 확정 | 미작성 |
| `docs/requirements.md` | MVP 범위와 우선순위 확정 | 초안 |
| `docs/functional_spec.md` | 기능별 입력, 처리, 결과 확정 | 초안 |
| `docs/data_model.md` | 저장할 데이터와 관계 확정 | 초안 |
| `docs/api_spec.md` | API 요청/응답 계약 확정 | 초안 |
| `docs/ui_ux_flow.md` | 화면 흐름과 상태 정의 | 초안 |
| `docs/figma_make_ui_prompt.md` | Figma Make로 UI 초안 생성 | 작성됨 |

## 1. 전체 개발 흐름

```text
사용자
  -> 화면 컴포넌트
  -> 클라이언트 상태/폼 검증
  -> API 호출 함수
  -> 서버 라우트
  -> 서비스 함수
  -> 저장소 함수
  -> 데이터베이스
  -> 응답 반환
  -> 화면 상태 갱신
```

각 계층의 책임은 명확히 나눕니다.

- 화면 파일은 UI 표시와 사용자 입력 처리만 담당합니다.
- API 클라이언트 파일은 HTTP 요청과 응답 변환만 담당합니다.
- 서버 라우트 파일은 요청 파싱, 인증 확인, 응답 포맷을 담당합니다.
- 서비스 파일은 비즈니스 규칙을 담당합니다.
- 저장소 파일은 데이터 저장소 접근만 담당합니다.

## 2. 추천 프로젝트 구조

```text
commtool/
  docs/
    app_dev_def.md
    requirements.md
    functional_spec.md
    architecture.md
    data_model.md
    api_spec.md
    ui_ux_flow.md
    development_plan.md
    test_plan.md
    deployment_ops.md
  src/
    app/
      layout.tsx
      page.tsx
      loading.tsx
      error.tsx
      dashboard/
        page.tsx
      settings/
        page.tsx
      api/
        health/
          route.ts
        auth/
          login/
            route.ts
          logout/
            route.ts
        items/
          route.ts
        items/
          [itemId]/
            route.ts
    components/
      AppShell.tsx
      HeaderBar.tsx
      SideNav.tsx
      EmptyState.tsx
      ErrorMessage.tsx
      LoadingState.tsx
      ItemForm.tsx
      ItemList.tsx
      ItemDetail.tsx
    features/
      items/
        itemTypes.ts
        itemSchema.ts
        itemService.ts
        itemRepository.ts
        itemApi.ts
        useItems.ts
    lib/
      apiResponse.ts
      auth.ts
      env.ts
      logger.ts
      validation.ts
    styles/
      globals.css
  tests/
    itemService.test.ts
    itemApi.test.ts
```

`items`는 실제 도메인이 정해지기 전 임시 이름입니다. 예를 들어 앱의 핵심 대상이 "프로젝트", "메시지", "고객", "문서"라면 `items`를 각각 `projects`, `messages`, `customers`, `documents`로 바꿉니다.

## 3. 주요 파일별 역할

| 파일명 | 사용처 | 용도 |
| --- | --- | --- |
| `src/app/layout.tsx` | 모든 화면 | 공통 HTML 구조, 전역 스타일, 메타데이터 설정 |
| `src/app/page.tsx` | 첫 진입 화면 | 로그인 전 안내 또는 대시보드 진입 화면 |
| `src/app/dashboard/page.tsx` | 로그인 후 메인 화면 | 핵심 데이터 목록과 주요 액션 표시 |
| `src/app/settings/page.tsx` | 설정 화면 | 사용자 설정, 앱 설정 관리 |
| `src/app/api/health/route.ts` | 배포/모니터링 | 서버 상태 확인 API |
| `src/app/api/auth/login/route.ts` | 로그인 요청 | 사용자 인증 처리 |
| `src/app/api/auth/logout/route.ts` | 로그아웃 요청 | 세션 종료 처리 |
| `src/app/api/items/route.ts` | 목록 조회/생성 API | 핵심 데이터 목록 조회와 생성 |
| `src/app/api/items/[itemId]/route.ts` | 단건 조회/수정/삭제 API | 특정 데이터 상세 처리 |
| `src/components/AppShell.tsx` | 모든 로그인 후 화면 | 헤더, 사이드바, 본문 레이아웃 구성 |
| `src/components/HeaderBar.tsx` | 상단 영역 | 앱 이름, 사용자 메뉴, 주요 액션 표시 |
| `src/components/SideNav.tsx` | 좌측 내비게이션 | 화면 이동 메뉴 표시 |
| `src/components/EmptyState.tsx` | 데이터가 없는 화면 | 빈 상태 메시지와 다음 액션 표시 |
| `src/components/ErrorMessage.tsx` | 오류 발생 영역 | 오류 메시지와 재시도 액션 표시 |
| `src/components/LoadingState.tsx` | 로딩 영역 | 데이터 로딩 중 상태 표시 |
| `src/components/ItemForm.tsx` | 생성/수정 화면 | 핵심 데이터 입력 폼 |
| `src/components/ItemList.tsx` | 목록 화면 | 핵심 데이터 리스트 렌더링 |
| `src/components/ItemDetail.tsx` | 상세 화면 | 핵심 데이터 상세 정보 표시 |
| `src/features/items/itemTypes.ts` | 앱 전체 | 핵심 데이터 타입 정의 |
| `src/features/items/itemSchema.ts` | 폼/API 검증 | 입력값 검증 스키마 정의 |
| `src/features/items/itemService.ts` | 서버 로직 | 비즈니스 규칙 처리 |
| `src/features/items/itemRepository.ts` | 서버 로직 | 데이터 저장소 접근 |
| `src/features/items/itemApi.ts` | 클라이언트 로직 | 브라우저에서 서버 API 호출 |
| `src/features/items/useItems.ts` | 화면 컴포넌트 | 목록/생성/수정/삭제 상태 관리 |
| `src/lib/apiResponse.ts` | 모든 API route | 성공/실패 응답 포맷 통일 |
| `src/lib/auth.ts` | API route, 화면 보호 | 로그인 사용자 확인과 권한 검사 |
| `src/lib/env.ts` | 서버 시작 시 | 환경 변수 로드와 검증 |
| `src/lib/logger.ts` | 서버 로직 | 오류와 주요 이벤트 기록 |
| `src/lib/validation.ts` | API route | 요청 본문 검증 공통 처리 |

## 4. 함수명과 용도

### `src/lib/apiResponse.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `createSuccessResponse(data, message)` | API route | 성공 응답을 `{ data, message }` 형식으로 통일 |
| `createErrorResponse(code, message, status)` | API route | 실패 응답을 `{ error: { code, message } }` 형식으로 통일 |

### `src/lib/auth.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `getCurrentUser(request)` | API route, 서버 컴포넌트 | 현재 요청의 로그인 사용자를 조회 |
| `requireUser(request)` | 보호 API route | 로그인하지 않은 요청을 차단 |
| `hasPermission(user, action, resource)` | 서비스 함수 | 사용자 권한 확인 |

### `src/lib/env.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `getEnv()` | 서버 코드 | 환경 변수를 읽고 필수값 누락 여부를 검증 |

### `src/lib/validation.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `parseJsonBody(request)` | API route | 요청 body를 JSON으로 파싱 |
| `validateRequest(schema, input)` | API route | 스키마 기준으로 입력값을 검증 |

### `src/features/items/itemRepository.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `findItemsByUserId(userId)` | `itemService.ts` | 특정 사용자의 목록 데이터 조회 |
| `findItemById(itemId)` | `itemService.ts` | 단일 데이터 조회 |
| `createItemRecord(input)` | `itemService.ts` | 새 데이터 저장 |
| `updateItemRecord(itemId, input)` | `itemService.ts` | 기존 데이터 수정 |
| `deleteItemRecord(itemId)` | `itemService.ts` | 기존 데이터 삭제 |

### `src/features/items/itemService.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `listItems(user)` | API route | 권한 확인 후 목록 조회 |
| `getItemDetail(user, itemId)` | API route | 권한 확인 후 상세 조회 |
| `createItem(user, input)` | API route | 입력값과 권한 확인 후 생성 |
| `updateItem(user, itemId, input)` | API route | 소유권과 입력값 확인 후 수정 |
| `deleteItem(user, itemId)` | API route | 소유권 확인 후 삭제 |

### `src/features/items/itemApi.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `fetchItems()` | `useItems.ts`, 화면 컴포넌트 | 목록 API 호출 |
| `fetchItemDetail(itemId)` | 상세 화면 | 상세 API 호출 |
| `requestCreateItem(input)` | 생성 폼 | 생성 API 호출 |
| `requestUpdateItem(itemId, input)` | 수정 폼 | 수정 API 호출 |
| `requestDeleteItem(itemId)` | 목록/상세 화면 | 삭제 API 호출 |

### `src/features/items/useItems.ts`

| 함수명 | 사용처 | 용도 |
| --- | --- | --- |
| `useItems()` | `ItemList.tsx`, `dashboard/page.tsx` | 목록 데이터와 로딩/오류 상태 관리 |
| `useItemDetail(itemId)` | `ItemDetail.tsx` | 단일 데이터 상태 관리 |

## 5. 주요 변수명과 용도

| 변수명 | 선언 위치 | 사용처 | 용도 |
| --- | --- | --- | --- |
| `currentUser` | `auth.ts`, API route | 서비스 함수 호출 전 | 현재 로그인한 사용자 정보 |
| `userId` | API route, repository | 데이터 조회 조건 | 사용자 소유 데이터 필터링 |
| `itemId` | 상세/수정/삭제 route | 상세 조회, 수정, 삭제 | URL에서 받은 핵심 데이터 ID |
| `items` | 목록 화면, hook | 리스트 렌더링 | 조회된 핵심 데이터 배열 |
| `selectedItem` | 상세 화면 | 상세 렌더링 | 사용자가 선택한 단일 데이터 |
| `itemInput` | 폼, API route | 생성/수정 요청 | 사용자가 입력한 데이터 |
| `validatedInput` | API route | 서비스 함수 호출 | 검증이 끝난 안전한 입력값 |
| `isLoading` | hook, 컴포넌트 | 로딩 표시 | 비동기 요청 진행 여부 |
| `errorMessage` | hook, 컴포넌트 | 오류 표시 | 사용자에게 보여줄 오류 메시지 |
| `requestId` | API route, logger | 로그 추적 | 요청 단위 추적 ID |
| `env` | `env.ts` | 서버 코드 | 환경 변수 묶음 |

## 6. API 처리 흐름

### 목록 조회: `GET /api/items`

```text
src/app/api/items/route.ts
  -> requireUser(request)
  -> listItems(currentUser)
  -> findItemsByUserId(currentUser.id)
  -> createSuccessResponse(items, "ok")
```

사용 함수:

- `requireUser(request)`: 인증되지 않은 사용자를 차단합니다.
- `listItems(currentUser)`: 목록 조회 규칙을 적용합니다.
- `findItemsByUserId(currentUser.id)`: 저장소에서 사용자 데이터를 조회합니다.
- `createSuccessResponse(items, "ok")`: 응답 형식을 통일합니다.

### 생성: `POST /api/items`

```text
src/app/api/items/route.ts
  -> requireUser(request)
  -> parseJsonBody(request)
  -> validateRequest(createItemSchema, body)
  -> createItem(currentUser, validatedInput)
  -> createItemRecord(createInput)
  -> createSuccessResponse(createdItem, "created")
```

사용 변수:

- `body`: 파싱 전 요청 데이터입니다.
- `validatedInput`: 스키마 검증이 완료된 데이터입니다.
- `createdItem`: 저장소에 생성된 결과 데이터입니다.

### 수정: `PATCH /api/items/[itemId]`

```text
src/app/api/items/[itemId]/route.ts
  -> requireUser(request)
  -> validateRequest(updateItemSchema, body)
  -> updateItem(currentUser, itemId, validatedInput)
  -> updateItemRecord(itemId, updateInput)
  -> createSuccessResponse(updatedItem, "updated")
```

### 삭제: `DELETE /api/items/[itemId]`

```text
src/app/api/items/[itemId]/route.ts
  -> requireUser(request)
  -> deleteItem(currentUser, itemId)
  -> deleteItemRecord(itemId)
  -> createSuccessResponse({ itemId }, "deleted")
```

## 7. 화면 처리 흐름

### 대시보드 화면

```text
src/app/dashboard/page.tsx
  -> AppShell
  -> ItemList
  -> useItems()
  -> fetchItems()
  -> GET /api/items
```

사용 변수:

- `items`: 목록 렌더링에 사용합니다.
- `isLoading`: `LoadingState` 표시 여부를 결정합니다.
- `errorMessage`: `ErrorMessage`에 전달합니다.

### 생성/수정 폼

```text
ItemForm.tsx
  -> itemInput 상태 변경
  -> 클라이언트 검증
  -> requestCreateItem(itemInput) 또는 requestUpdateItem(itemId, itemInput)
  -> API 응답 성공 시 목록 갱신
```

사용 변수:

- `itemInput`: 폼 입력값입니다.
- `fieldErrors`: 필드별 오류 메시지입니다.
- `submitLabel`: 생성/수정 버튼 문구입니다.
- `onSubmit`: 폼 제출 시 실행할 콜백입니다.

## 8. 개발 단계별 작업

### Phase 0. 정의 확정

- [ ] `docs/app_dev_def.md`에 앱 목적, 사용자, 핵심 기능 작성
- [ ] `docs/requirements.md`에서 MVP 범위 확정
- [ ] `docs/functional_spec.md`에서 기능 ID와 기능명 확정
- [ ] `items` 임시 도메인명을 실제 도메인명으로 변경

### Phase 1. 프로젝트 기반 구성

- [ ] `src/app/layout.tsx` 생성
- [ ] `src/app/page.tsx` 생성
- [ ] `src/styles/globals.css` 생성
- [ ] `src/lib/env.ts` 생성
- [ ] `src/lib/apiResponse.ts` 생성
- [ ] `src/lib/logger.ts` 생성

### Phase 2. 데이터와 API 구현

- [ ] `src/features/items/itemTypes.ts` 생성
- [ ] `src/features/items/itemSchema.ts` 생성
- [ ] `src/features/items/itemRepository.ts` 생성
- [ ] `src/features/items/itemService.ts` 생성
- [ ] `src/app/api/health/route.ts` 생성
- [ ] `src/app/api/items/route.ts` 생성
- [ ] `src/app/api/items/[itemId]/route.ts` 생성

### Phase 3. 화면 구현

- [ ] `src/components/AppShell.tsx` 생성
- [ ] `src/components/HeaderBar.tsx` 생성
- [ ] `src/components/SideNav.tsx` 생성
- [ ] `src/components/ItemList.tsx` 생성
- [ ] `src/components/ItemForm.tsx` 생성
- [ ] `src/components/ItemDetail.tsx` 생성
- [ ] `src/app/dashboard/page.tsx` 생성

### Phase 4. 상태 관리와 클라이언트 API

- [ ] `src/features/items/itemApi.ts` 생성
- [ ] `src/features/items/useItems.ts` 생성
- [ ] 목록 조회 로딩/오류/빈 상태 연결
- [ ] 생성/수정/삭제 후 목록 갱신 처리

### Phase 5. 테스트와 품질 보강

- [ ] `tests/itemService.test.ts` 작성
- [ ] `tests/itemApi.test.ts` 작성
- [ ] API 오류 응답 테스트
- [ ] 입력 검증 테스트
- [ ] 권한 없는 요청 차단 테스트

### Phase 6. 배포 준비

- [ ] `docs/deployment_ops.md` 기준 환경 변수 정리
- [ ] `/api/health` 운영 환경 확인
- [ ] 배포 전 테스트 실행
- [ ] GitHub에 최종 커밋과 push

## 9. 작업 관리 기준

- P0: 앱 실행, 핵심 데이터 CRUD, 기본 인증, 배포에 반드시 필요한 작업
- P1: 검색, 필터, 상세 권한, 사용성 개선
- P2: 고급 설정, 통계, 외부 연동, 자동화

## 10. 리스크와 대응

| 리스크 | 영향 | 대응 |
| --- | --- | --- |
| `app_dev_def.md` 미작성 | 실제 기능명과 도메인명이 불명확함 | 먼저 앱 정의를 작성한 뒤 `items` 명칭을 교체 |
| 인증 방식 미정 | API와 화면 보호 흐름 변경 가능 | 초기에 `auth.ts` 경계만 만들고 구현은 교체 가능하게 유지 |
| 데이터베이스 미정 | repository 구현 변경 가능 | `itemRepository.ts`에 저장소 접근을 모아 변경 범위 제한 |
| API 응답 형식 불일치 | 클라이언트 오류 처리 복잡 | `apiResponse.ts`로 응답 형식 통일 |
| 폼 검증 중복 | 화면과 서버 규칙 불일치 | `itemSchema.ts`를 공통 기준으로 사용 |

## 11. 개발 검토

### 11.1 현재 계획 평가

현재 계획은 MVP 웹앱 개발을 시작하기 위한 기본 구조로 사용할 수 있습니다. 화면, API, 서비스, 저장소, 공통 유틸리티가 분리되어 있어 기능이 늘어나도 변경 범위를 추적하기 쉽습니다. 특히 `itemService.ts`와 `itemRepository.ts`를 분리한 점은 데이터베이스가 바뀌거나 외부 API 연동이 추가될 때 수정 범위를 줄이는 데 유리합니다.

다만 `docs/app_dev_def.md`가 아직 비어 있으므로 실제 개발 착수 전에는 다음 항목이 확정되어야 합니다.

- 앱이 해결할 문제
- 주요 사용자
- MVP에 반드시 들어갈 핵심 기능
- 핵심 데이터의 실제 이름
- 인증 필요 여부
- 데이터 저장 방식
- 배포 대상 환경

### 11.2 구조 검토

| 검토 항목 | 판단 | 이유 |
| --- | --- | --- |
| 폴더 구조 | 적합 | 화면, 기능, 공통 코드가 분리되어 유지보수에 유리함 |
| API 구조 | 적합 | REST 형태의 목록/상세/생성/수정/삭제 흐름을 표현하기 쉬움 |
| 도메인 구조 | 조건부 적합 | `items`는 임시 이름이므로 실제 도메인 확정 후 변경 필요 |
| 인증 구조 | 보완 필요 | 인증 방식이 미정이므로 `auth.ts`는 인터페이스 중심으로 먼저 작성 필요 |
| 데이터 계층 | 적합 | repository 경계를 두면 DB 변경에 대응하기 쉬움 |
| 테스트 구조 | 보완 필요 | 서비스 테스트 외에 API route와 주요 화면 테스트 추가 검토 필요 |
| 배포 구조 | 보완 필요 | 사용 플랫폼이 정해진 뒤 환경 변수와 빌드 명령 확정 필요 |

### 11.3 코드 도입 순서 검토

가장 안전한 도입 순서는 다음과 같습니다.

1. `docs/app_dev_def.md`를 작성하여 실제 도메인명을 확정합니다.
2. `src/features/items/itemTypes.ts`를 먼저 만들고 데이터 타입을 고정합니다.
3. `src/features/items/itemSchema.ts`에서 생성/수정 입력값 검증을 정의합니다.
4. `src/lib/apiResponse.ts`, `src/lib/validation.ts`, `src/lib/logger.ts`를 작성합니다.
5. `src/features/items/itemRepository.ts`를 임시 메모리 저장소 또는 실제 DB 기준으로 구현합니다.
6. `src/features/items/itemService.ts`에서 권한, 소유권, 입력 규칙을 처리합니다.
7. `src/app/api/items/route.ts`와 `src/app/api/items/[itemId]/route.ts`를 구현합니다.
8. `src/features/items/itemApi.ts`와 `src/features/items/useItems.ts`를 구현합니다.
9. `src/components/ItemList.tsx`, `src/components/ItemForm.tsx`, `src/app/dashboard/page.tsx`를 구현합니다.
10. 테스트를 추가하고 배포 환경을 연결합니다.

이 순서를 따르면 UI를 만들기 전에 데이터 계약과 API 흐름이 먼저 고정됩니다. 그 결과 화면 구현 중에 요청/응답 구조가 흔들릴 가능성이 줄어듭니다.

### 11.4 파일명, 함수명, 변수명 검토 기준

| 구분 | 기준 | 예시 |
| --- | --- | --- |
| 파일명 | 역할이 드러나는 명사형 사용 | `itemService.ts`, `apiResponse.ts` |
| 컴포넌트명 | PascalCase 사용 | `ItemList`, `ItemForm` |
| 함수명 | 동사로 시작 | `createItem`, `fetchItems`, `validateRequest` |
| 변수명 | 의미가 드러나는 명사 사용 | `currentUser`, `validatedInput`, `errorMessage` |
| ID 변수 | 대상명 + `Id` 사용 | `userId`, `itemId` |
| boolean 변수 | `is`, `has`, `can`, `should` 사용 | `isLoading`, `hasPermission` |
| API 함수 | 네트워크 요청임을 드러냄 | `requestCreateItem`, `fetchItemDetail` |
| 저장소 함수 | 데이터 접근 동작을 드러냄 | `findItemById`, `createItemRecord` |

### 11.5 개발 착수 조건

아래 조건을 만족하면 실제 코드 작성에 들어갈 수 있습니다.

- [ ] `docs/app_dev_def.md`에 앱 목적과 핵심 기능이 작성되어 있다.
- [ ] `items`를 대체할 실제 도메인명이 정해져 있다.
- [ ] MVP에서 필요한 화면 목록이 정해져 있다.
- [ ] 생성/수정/삭제가 필요한 데이터 필드가 정해져 있다.
- [ ] 인증 필요 여부가 정해져 있다.
- [ ] 저장소 방식이 정해져 있다.
- [ ] 로컬 실행 방식과 배포 대상이 정해져 있다.

### 11.6 보완 권장 사항

현재 문서는 개발을 시작하기 위한 구조 문서로는 충분하지만, 실제 구현 전에 다음 문서를 함께 보완하는 것이 좋습니다.

- `docs/app_dev_def.md`: 앱의 목적과 기능 정의를 가장 먼저 작성합니다.
- `docs/data_model.md`: `Item` 대신 실제 엔티티 이름과 필드를 작성합니다.
- `docs/api_spec.md`: 실제 엔드포인트 이름과 요청/응답 예시를 작성합니다.
- `docs/ui_ux_flow.md`: 첫 화면, 목록 화면, 생성/수정 화면, 설정 화면의 흐름을 확정합니다.
- `docs/test_plan.md`: MVP 핵심 시나리오 기준으로 테스트 항목을 구체화합니다.

## 12. Git 커밋과 버전 관리

이 프로젝트는 작업 이력을 추적하고 되돌릴 수 있도록 Git 커밋 단위를 작게 유지합니다. 커밋은 "무엇을 바꿨는지"뿐 아니라 "왜 바꿨는지"를 나중에 확인할 수 있어야 합니다.

### 12.1 기본 작업 흐름

```text
작업 전 상태 확인
  -> 파일 수정
  -> 변경 내용 확인
  -> 관련 파일만 stage
  -> 의미 있는 메시지로 commit
  -> 원격 저장소에 push
```

사용 명령:

```bash
git status
git diff
git add <file>
git commit -m "type: summary"
git push
```

각 명령의 용도는 다음과 같습니다.

| 명령 | 사용 시점 | 용도 |
| --- | --- | --- |
| `git status` | 작업 전/후 | 현재 브랜치, 수정 파일, stage 상태 확인 |
| `git diff` | 커밋 전 | 아직 stage하지 않은 변경 내용 검토 |
| `git diff --staged` | 커밋 직전 | 커밋에 들어갈 변경 내용 최종 검토 |
| `git add <file>` | 커밋 준비 | 커밋에 포함할 파일 선택 |
| `git commit -m "message"` | 작업 단위 완료 | 변경 이력을 로컬 저장소에 기록 |
| `git push` | 커밋 후 | 로컬 커밋을 GitHub 원격 저장소에 업로드 |

### 12.2 커밋 단위 기준

커밋은 하나의 목적만 가지도록 나눕니다.

좋은 커밋 단위:

- 문서 구조 추가
- API 응답 유틸리티 추가
- 아이템 목록 조회 기능 추가
- 입력 검증 스키마 추가
- 로그인 오류 처리 수정
- 테스트 케이스 추가

피해야 할 커밋 단위:

- 문서 수정, API 구현, UI 수정, 테스트 추가를 한 커밋에 모두 포함
- 여러 기능을 한 번에 구현
- 자동 포맷 변경과 기능 변경을 같은 커밋에 포함
- 임시 파일이나 민감 정보를 포함

### 12.3 커밋 메시지 규칙

커밋 메시지는 다음 형식을 사용합니다.

```text
type: summary
```

예시:

```text
docs: add development plan
feat: add item list api
fix: handle empty item title
test: add item service tests
refactor: split item repository
chore: configure formatter
```

사용할 `type` 기준:

| type | 의미 | 예시 |
| --- | --- | --- |
| `docs` | 문서 변경 | `docs: add git workflow` |
| `feat` | 새 기능 추가 | `feat: add item creation api` |
| `fix` | 버그 수정 | `fix: validate missing item id` |
| `test` | 테스트 추가/수정 | `test: add item service tests` |
| `refactor` | 동작 변경 없는 구조 개선 | `refactor: move validation helper` |
| `style` | 포맷, CSS, 문법 정리 | `style: update dashboard spacing` |
| `chore` | 설정, 빌드, 기타 작업 | `chore: add lint config` |

메시지 작성 기준:

- 영어 소문자 동사 또는 명사 중심으로 짧게 작성합니다.
- 마침표는 붙이지 않습니다.
- 한 커밋이 한 가지 목적을 갖도록 메시지를 작성합니다.
- `update`, `change`, `fix stuff`처럼 의미가 넓은 표현은 피합니다.

### 12.4 브랜치 관리 기준

기본 브랜치는 `main`을 사용합니다. 작은 개인 프로젝트에서는 `main`에 직접 커밋할 수 있지만, 기능 단위가 커지면 별도 브랜치를 사용합니다.

브랜치 이름 형식:

```text
type/short-description
```

예시:

```text
docs/git-workflow
feat/item-api
fix/login-error
test/item-service
```

브랜치 작업 흐름:

```bash
git switch -c feat/item-api
git status
git add src/features/items/itemService.ts
git commit -m "feat: add item service"
git push -u origin feat/item-api
```

작업이 완료되면 GitHub에서 pull request를 만들고 `main`에 병합합니다. 혼자 작업하는 경우에도 pull request를 사용하면 변경 내용을 검토하고 기록하기 좋습니다.

### 12.5 버전 태그 기준

배포 가능한 상태가 되면 Git 태그로 버전을 남깁니다. 버전은 다음 형식을 사용합니다.

```text
vMAJOR.MINOR.PATCH
```

예시:

```text
v0.1.0
v0.2.0
v1.0.0
```

버전 증가 기준:

| 구분 | 의미 | 예시 |
| --- | --- | --- |
| `MAJOR` | 큰 구조 변경 또는 호환성 깨짐 | `v1.0.0`에서 `v2.0.0` |
| `MINOR` | 새 기능 추가 | `v0.1.0`에서 `v0.2.0` |
| `PATCH` | 버그 수정, 작은 개선 | `v0.1.0`에서 `v0.1.1` |

태그 생성 명령:

```bash
git tag v0.1.0
git push origin v0.1.0
```

현재 버전 목록 확인:

```bash
git tag
```

### 12.6 커밋 전 체크리스트

커밋 전에는 다음 항목을 확인합니다.

- [ ] `git status`로 변경 파일을 확인했다.
- [ ] `git diff`로 변경 내용을 검토했다.
- [ ] 관련 없는 파일을 커밋에 포함하지 않았다.
- [ ] 민감 정보, 토큰, 비밀번호가 포함되지 않았다.
- [ ] 문서 변경과 코드 변경이 필요하면 함께 업데이트했다.
- [ ] 테스트가 필요한 변경이면 테스트를 추가하거나 실행했다.
- [ ] 커밋 메시지가 변경 목적을 설명한다.

### 12.7 push 오류 대응

자주 발생하는 오류와 대응은 다음과 같습니다.

| 오류 | 원인 | 대응 |
| --- | --- | --- |
| `src refspec main does not match any` | 아직 커밋이 없음 | `git add`, `git commit` 후 다시 push |
| `Permission denied` 또는 `403` | GitHub 계정 권한 없음 | 권한 있는 계정으로 로그인하거나 저장소 권한 부여 |
| `Could not resolve host: github.com` | 네트워크 연결 문제 | 인터넷 연결 또는 실행 환경 네트워크 권한 확인 |
| `rejected non-fast-forward` | 원격에 내가 없는 커밋이 있음 | `git pull --rebase` 후 충돌 해결, 다시 push |

기본 복구 흐름:

```bash
git status
git pull --rebase
git push
```

충돌이 발생하면 충돌 파일을 수정한 뒤 다음 순서로 마무리합니다.

```bash
git add <resolved-file>
git rebase --continue
git push
```

### 12.8 이 프로젝트의 권장 커밋 순서

현재 문서 작업 이후 권장 커밋 순서는 다음과 같습니다.

1. 문서 템플릿 추가

```bash
git add docs
git commit -m "docs: add project planning documents"
```

2. 앱 정의 작성

```bash
git add docs/app_dev_def.md
git commit -m "docs: define app requirements"
```

3. 실제 코드 기반 생성

```bash
git add package.json src
git commit -m "chore: initialize app structure"
```

4. 핵심 기능 추가

```bash
git add src/features src/app/api
git commit -m "feat: add core item api"
```

5. 화면 구현

```bash
git add src/components src/app
git commit -m "feat: add item dashboard"
```

6. 테스트 추가

```bash
git add tests
git commit -m "test: add core feature tests"
```
