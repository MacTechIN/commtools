# Figma Make UI 개발 프롬프트

이 문서는 `commtool` 앱의 UI를 Figma Make에서 생성하기 위한 프롬프트입니다. Figma Make에 아래 프롬프트를 붙여넣고, 생성된 화면을 기준으로 `docs/ui_ux_flow.md`, `docs/development_plan.md`, 실제 프론트엔드 컴포넌트를 갱신합니다.

현재 `docs/app_dev_def.md`가 비어 있으므로, 아래 프롬프트는 MVP 업무형 웹앱 기준으로 작성되었습니다. 실제 앱 정의가 확정되면 `items`, `Item`, `아이템` 표현을 실제 도메인명으로 바꿉니다.

## 사용 방법

1. Figma Make를 엽니다.
2. 아래 `프롬프트` 섹션 전체를 복사해 입력합니다.
3. 생성된 UI에서 화면 구성, 컴포넌트명, 상태 표현을 확인합니다.
4. 확정된 UI 기준으로 `docs/ui_ux_flow.md`와 `src/components/*` 구현 계획을 갱신합니다.

## 프롬프트

```text
Create a production-ready web app UI for a product named "commtool".

The app is an operational dashboard for managing core business records. The current placeholder domain is "items". Design it so the domain can later be renamed to projects, messages, customers, documents, or another business entity.

Primary goal:
Make the UI clear enough for developers to implement directly. Show screen structure, reusable components, user flows, empty states, loading states, error states, and form validation states.

Audience:
- Operators or internal users who repeatedly review, create, edit, and manage records.
- Users need speed, clarity, predictable navigation, and dense but readable information.

Design style:
- Clean, modern, practical SaaS dashboard.
- Avoid a marketing landing page.
- Avoid oversized hero sections.
- Avoid decorative gradients, blobs, or purely visual ornament.
- Use restrained color with strong hierarchy.
- Prefer high readability, compact spacing, and clear interaction states.
- Use an 8px or smaller border radius for cards and panels.
- Use familiar icons for navigation and actions.
- Keep text concise and implementation-oriented.

Required screens:

1. App shell / dashboard layout
- Left sidebar navigation.
- Top header bar.
- Main content area.
- Current user area.
- Primary action button for creating a new item.
- Navigation entries:
  - Dashboard
  - Items
  - Settings
- Header should include app name "commtool", page title, search input, and user menu.

2. Dashboard screen
- Summary metrics row:
  - Total items
  - Active items
  - Pending items
  - Recently updated
- Recent items table or list.
- Empty state if there are no records.
- Loading skeleton state.
- Error state with retry action.

3. Items list screen
- Data table optimized for scanning.
- Columns:
  - Name
  - Status
  - Owner
  - Updated
  - Actions
- Search input.
- Status filter.
- Sort control.
- Create item button.
- Row actions:
  - View
  - Edit
  - Delete
- Pagination or compact page controls.
- Show selected row state and hover state.

4. Item detail screen
- Page header with item name, status badge, updated timestamp.
- Detail sections:
  - Overview
  - Activity
  - Metadata
- Actions:
  - Edit
  - Delete
  - Back to list
- Show loading, not found, and permission error states.

5. Create/Edit item form
- Form fields:
  - Name
  - Description
  - Status
  - Owner
  - Tags
- Save and cancel actions.
- Field-level validation messages.
- Disabled submit state while saving.
- Success feedback.
- Error feedback.
- Unsaved changes confirmation pattern.

6. Settings screen
- User profile section.
- App preferences section.
- Danger zone section for destructive actions.
- Clear separation between normal settings and destructive actions.

Reusable components to show:
- AppShell
- HeaderBar
- SideNav
- EmptyState
- ErrorMessage
- LoadingState
- ItemList
- ItemDetail
- ItemForm
- StatusBadge
- SearchInput
- FilterMenu
- ConfirmDialog
- PaginationControls

Developer mapping:
Use component labels or frame names that match the intended React component names:
- AppShell
- HeaderBar
- SideNav
- EmptyState
- ErrorMessage
- LoadingState
- ItemForm
- ItemList
- ItemDetail

State requirements:
For each important screen, include these states where relevant:
- Default
- Loading
- Empty
- Error
- Validation error
- Saving
- Success

Responsive behavior:
- Provide desktop layout at 1440px width.
- Provide tablet layout around 768px width.
- Provide mobile layout around 390px width.
- On mobile, collapse sidebar into a menu button.
- Ensure table content becomes a readable card/list layout on mobile.
- Ensure button text does not overflow.

Accessibility:
- Use visible focus states.
- Use sufficient color contrast.
- Do not rely on color alone for status.
- Make form labels clear.
- Include accessible error messaging patterns.

Content tone:
- Professional and concise.
- Use practical UI labels, not marketing copy.
- Avoid tutorial text inside the app.

Deliverables:
- A complete desktop dashboard flow.
- A responsive mobile version.
- Component frames named for implementation.
- Screen states for developer handoff.
- Design tokens for color, typography, spacing, and radius.
```

## 생성 후 검토 체크리스트

- [ ] 화면명이 `Dashboard`, `Items`, `ItemDetail`, `Settings`처럼 구현 가능한 단위로 나뉘어 있다.
- [ ] 컴포넌트 프레임명이 `AppShell`, `HeaderBar`, `SideNav`, `ItemList`, `ItemForm`처럼 코드명과 맞다.
- [ ] 목록, 상세, 생성/수정 폼 흐름이 연결되어 있다.
- [ ] 로딩, 빈 상태, 오류 상태가 포함되어 있다.
- [ ] 모바일 화면에서 사이드바와 테이블이 자연스럽게 변형된다.
- [ ] 색상, 폰트, 간격, radius 같은 디자인 토큰을 확인할 수 있다.
- [ ] 개발자가 `src/components/*` 파일로 옮기기 쉬운 단위로 UI가 분리되어 있다.

## 코드 반영 기준

Figma Make 결과를 코드로 옮길 때는 다음 파일에 우선 반영합니다.

| Figma 화면/컴포넌트 | 코드 파일 | 반영 내용 |
| --- | --- | --- |
| AppShell | `src/components/AppShell.tsx` | 전체 레이아웃 |
| HeaderBar | `src/components/HeaderBar.tsx` | 상단 바, 검색, 사용자 메뉴 |
| SideNav | `src/components/SideNav.tsx` | 좌측 메뉴 |
| Dashboard | `src/app/dashboard/page.tsx` | 메인 대시보드 화면 |
| ItemList | `src/components/ItemList.tsx` | 목록 테이블/카드 |
| ItemDetail | `src/components/ItemDetail.tsx` | 상세 정보 화면 |
| ItemForm | `src/components/ItemForm.tsx` | 생성/수정 폼 |
| EmptyState | `src/components/EmptyState.tsx` | 빈 상태 표시 |
| LoadingState | `src/components/LoadingState.tsx` | 로딩 상태 표시 |
| ErrorMessage | `src/components/ErrorMessage.tsx` | 오류 상태 표시 |

## 후속 문서 반영

Figma Make 결과가 확정되면 다음 문서를 갱신합니다.

- `docs/ui_ux_flow.md`: 화면 목록, 사용자 흐름, 상태별 UI를 실제 결과로 갱신합니다.
- `docs/development_plan.md`: 생성된 컴포넌트명과 실제 구현 파일명을 맞춥니다.
- `docs/functional_spec.md`: UI에서 드러난 기능과 액션을 기능 명세에 반영합니다.
