# Figma Make 전체 앱 UI 구현 프롬프트

이 문서는 `commtool` 앱 전체 기능을 Figma Make에서 UI 디자인으로 구현하기 위한 통합 프롬프트입니다. Figma Make에 `프롬프트` 섹션을 그대로 붙여넣어 사용합니다.

현재 `docs/app_dev_def.md`는 MVP 기능 정의서를 포함하고 있으며, 앱의 핵심 도메인은 임시로 `items`로 표현합니다. 실제 앱 도메인이 확정되면 `items`, `Item`, `아이템`, `records`를 실제 도메인명으로 바꿉니다.

## 목적

- 앱 전체 화면 구조를 Figma에서 먼저 명확히 만듭니다.
- 화면, 컴포넌트, 상태, 사용자 흐름을 개발자가 그대로 구현할 수 있게 정리합니다.
- `docs/development_plan.md`에 정의된 파일명과 컴포넌트명을 Figma 프레임명에 반영합니다.
- 데스크톱, 태블릿, 모바일 반응형 UI를 함께 생성합니다.

## 프롬프트

```text
Design a complete production-ready Figma UI for a web application named "commtool".

Context:
commtool is an operational web app for managing core business records. The current placeholder domain is "items". Design the UI so "items" can later be renamed to projects, messages, customers, documents, tasks, or another business entity without changing the overall structure.

The goal is not a marketing website. The goal is a real application interface that developers can implement directly.

Primary users:
- Internal operators
- Admin users
- Team members who repeatedly create, review, update, filter, and manage records

User needs:
- Fast scanning
- Clear navigation
- Predictable actions
- Reliable form validation
- Easy recovery from errors
- Mobile access for quick review and update

Design direction:
- Practical SaaS dashboard
- Professional, clear, calm, and implementation-ready
- Dense but readable layout
- Minimal decoration
- No landing page
- No hero section
- No decorative gradient blobs or abstract illustrations
- No oversized cards for simple data
- Use icons for navigation and actions
- Use an 8px or smaller radius for cards, panels, menus, and dialogs
- Use clear contrast and visible focus states
- Use design tokens for color, typography, spacing, border, shadow, and radius

Information architecture:
Create the following main areas:
1. AppShell
2. Dashboard
3. Items list
4. Item detail
5. Create item
6. Edit item
7. Settings
8. Error and empty states
9. Responsive mobile navigation

Frame naming requirements:
Name frames and components to match the implementation plan:
- AppShell
- HeaderBar
- SideNav
- DashboardPage
- ItemsPage
- ItemDetailPage
- ItemCreatePage
- ItemEditPage
- SettingsPage
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
- UserMenu
- Toast

Required layout:

1. AppShell
- Fixed or sticky left navigation on desktop.
- Collapsible navigation on tablet.
- Menu button with drawer navigation on mobile.
- Top header with page title, global search, create action, notification icon, and user menu.
- Main content area with consistent max width and spacing.
- Use clear active navigation state.

Navigation items:
- Dashboard
- Items
- Settings

2. DashboardPage
Purpose:
Give users a quick overview of operational status.

Include:
- Page title: Dashboard
- Primary action button: New item
- Summary metric row:
  - Total items
  - Active items
  - Pending items
  - Updated today
- Recent items section
- Activity section
- Quick filters or shortcuts
- Empty state when no items exist
- Loading skeleton
- Error state with retry button

Dashboard content examples:
- Item name
- Status
- Owner
- Last updated
- Recent activity message

3. ItemsPage
Purpose:
Allow users to search, filter, sort, create, view, edit, and delete records.

Include:
- Page title: Items
- SearchInput
- Status filter
- Owner filter
- Sort control
- New item button
- Data table on desktop
- Card list on mobile
- Bulk selection pattern if useful
- PaginationControls
- Row actions menu:
  - View
  - Edit
  - Delete

Desktop table columns:
- Checkbox
- Name
- Status
- Owner
- Tags
- Updated
- Actions

Mobile card fields:
- Name
- StatusBadge
- Owner
- Updated
- Primary action
- More menu

States to design:
- Default with sample rows
- Loading skeleton
- Empty list
- No search results
- API error with retry
- Row hover
- Row selected
- Delete confirmation dialog

4. ItemDetailPage
Purpose:
Show one record with enough detail for review and action.

Include:
- Back to Items button
- Item title
- StatusBadge
- Updated timestamp
- Owner
- Actions:
  - Edit
  - Delete
  - More
- Detail tabs or sections:
  - Overview
  - Activity
  - Metadata
- Activity timeline
- Related tags
- Delete confirmation dialog

States to design:
- Default
- Loading
- Not found
- Permission denied
- Delete confirmation
- Delete success toast

5. ItemCreatePage and ItemEditPage
Purpose:
Allow users to create or update a record with clear validation.

Use the same reusable ItemForm component.

Fields:
- Name
- Description
- Status
- Owner
- Tags
- Notes

Actions:
- Save
- Cancel
- Reset changes if editing

Validation states:
- Required name missing
- Description too long
- Invalid owner
- Save failed
- Saving disabled state
- Saving loading state
- Success toast
- Unsaved changes confirmation

Form layout:
- Desktop: centered form or two-column layout if metadata is separate.
- Mobile: single column.
- Keep labels visible.
- Show field-level error messages directly below fields.

6. SettingsPage
Purpose:
Allow account and app preferences management.

Sections:
- Profile
  - Name
  - Email
  - Avatar placeholder
- Preferences
  - Default view
  - Notification preference
  - Theme preference
- Data and workspace
  - Export data
  - Import data placeholder
- Danger zone
  - Delete workspace or reset data

States:
- Save success
- Save error
- Destructive action confirmation

7. Global states and feedback
Create reusable designs for:
- EmptyState
- LoadingState
- ErrorMessage
- Toast success
- Toast error
- ConfirmDialog
- Form validation
- Permission denied
- Not found

8. Design tokens
Create a small token page or style guide with:
- Colors:
  - Background
  - Surface
  - Border
  - Text primary
  - Text secondary
  - Primary action
  - Success
  - Warning
  - Danger
  - Focus ring
- Typography:
  - Page title
  - Section title
  - Body
  - Caption
  - Table header
- Spacing:
  - 4, 8, 12, 16, 24, 32
- Radius:
  - 4
  - 6
  - 8
- Shadows:
  - Use subtle shadows only for menus, dialogs, and overlays

9. Responsive requirements
Create designs for:
- Desktop: 1440px width
- Tablet: 768px width
- Mobile: 390px width

Responsive behavior:
- Desktop uses persistent SideNav.
- Tablet uses narrower SideNav or collapsible SideNav.
- Mobile uses top HeaderBar with menu button and drawer navigation.
- Tables become stacked cards on mobile.
- Form fields become single column on mobile.
- Buttons must not overflow.
- Text must wrap cleanly.
- Important actions remain reachable without horizontal scrolling.

10. Accessibility requirements
- Use visible keyboard focus states.
- Ensure color contrast is strong.
- Do not communicate status by color only.
- Use text labels with icons where meaning may be unclear.
- Form inputs must have visible labels.
- Error messages must be placed near the relevant field.
- Dialogs must have clear title, body, cancel, and confirm actions.

11. Developer handoff requirements
Include annotations or labels that help implementation:
- Component names
- Screen names
- State names
- Button action names
- Form field names
- API mapping notes where useful

Use these implementation mappings:
- AppShell -> src/components/AppShell.tsx
- HeaderBar -> src/components/HeaderBar.tsx
- SideNav -> src/components/SideNav.tsx
- DashboardPage -> src/app/dashboard/page.tsx
- ItemsPage -> src/app/items/page.tsx
- ItemDetailPage -> src/app/items/[itemId]/page.tsx
- ItemCreatePage -> src/app/items/new/page.tsx
- ItemEditPage -> src/app/items/[itemId]/edit/page.tsx
- ItemList -> src/components/ItemList.tsx
- ItemDetail -> src/components/ItemDetail.tsx
- ItemForm -> src/components/ItemForm.tsx
- EmptyState -> src/components/EmptyState.tsx
- LoadingState -> src/components/LoadingState.tsx
- ErrorMessage -> src/components/ErrorMessage.tsx

Use these action names:
- fetchItems
- fetchItemDetail
- requestCreateItem
- requestUpdateItem
- requestDeleteItem

Use these important variable names in labels or annotations:
- currentUser
- items
- selectedItem
- itemId
- itemInput
- isLoading
- errorMessage
- fieldErrors

12. Output expectations
Generate:
- A full desktop app flow
- Tablet adaptations
- Mobile adaptations
- Reusable component set
- State variants
- Design token page
- Developer handoff annotations

Make the UI feel like a real product ready for implementation, not a concept mockup.
```

## Figma 결과 검토 기준

- [ ] 전체 화면 흐름이 `Dashboard -> Items -> ItemDetail -> Create/Edit -> Settings`로 연결되어 있다.
- [ ] 각 화면에 기본, 로딩, 빈 상태, 오류 상태가 포함되어 있다.
- [ ] 컴포넌트명이 개발 계획의 파일명과 연결된다.
- [ ] 데스크톱, 태블릿, 모바일 화면이 모두 있다.
- [ ] 테이블이 모바일에서 카드형 목록으로 전환된다.
- [ ] 폼의 필수값, 오류, 저장 중, 성공 상태가 표현되어 있다.
- [ ] 삭제 같은 위험 액션에는 확인 다이얼로그가 있다.
- [ ] 디자인 토큰이 별도 페이지 또는 섹션으로 정리되어 있다.
- [ ] 개발자가 `src/components/*`와 `src/app/*`로 옮기기 쉬운 구조다.

## 코드 반영 기준

| Figma 화면/컴포넌트 | 코드 파일 | 반영 내용 |
| --- | --- | --- |
| AppShell | `src/components/AppShell.tsx` | 전체 앱 레이아웃 |
| HeaderBar | `src/components/HeaderBar.tsx` | 상단 바, 검색, 사용자 메뉴 |
| SideNav | `src/components/SideNav.tsx` | 데스크톱/모바일 내비게이션 |
| DashboardPage | `src/app/dashboard/page.tsx` | 대시보드 화면 |
| ItemsPage | `src/app/items/page.tsx` | 아이템 목록 화면 |
| ItemDetailPage | `src/app/items/[itemId]/page.tsx` | 상세 화면 |
| ItemCreatePage | `src/app/items/new/page.tsx` | 생성 화면 |
| ItemEditPage | `src/app/items/[itemId]/edit/page.tsx` | 수정 화면 |
| ItemList | `src/components/ItemList.tsx` | 테이블/모바일 카드 목록 |
| ItemDetail | `src/components/ItemDetail.tsx` | 상세 정보 컴포넌트 |
| ItemForm | `src/components/ItemForm.tsx` | 생성/수정 공통 폼 |
| EmptyState | `src/components/EmptyState.tsx` | 빈 상태 |
| LoadingState | `src/components/LoadingState.tsx` | 로딩 상태 |
| ErrorMessage | `src/components/ErrorMessage.tsx` | 오류 상태 |
| ConfirmDialog | `src/components/ConfirmDialog.tsx` | 삭제/위험 액션 확인 |
| Toast | `src/components/Toast.tsx` | 성공/오류 피드백 |

## 후속 작업

Figma Make 결과가 나오면 다음 순서로 문서를 갱신합니다.

1. `docs/ui_ux_flow.md`에 실제 화면 흐름을 반영합니다.
2. `docs/development_plan.md`의 컴포넌트 파일 목록을 Figma 결과와 맞춥니다.
3. `docs/functional_spec.md`에 화면 액션과 상태를 기능으로 정리합니다.
4. 실제 구현 시 `src/components`와 `src/app` 구조를 Figma 프레임명 기준으로 생성합니다.
