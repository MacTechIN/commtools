# 데이터 모델

## 1. 엔티티 목록

| 엔티티 | 설명 | 주요 필드 |
| --- | --- | --- |
| User | 사용자 | id, email, name, role |
| Item | 관리 대상 핵심 데이터 | id, name, status, ownerId, tags |
| Activity | 아이템 변경/활동 기록 | id, itemId, actorId, action, createdAt |
| UserSettings | 사용자별 설정 | userId, defaultView, theme, notificationsEnabled |

## 2. 엔티티 상세

### User

| 필드 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| id | string | Y | 사용자 고유 ID |
| email | string | Y | 로그인 이메일 |
| name | string | Y | 표시 이름 |
| role | string | Y | 사용자 역할: admin, operator, member |
| createdAt | datetime | Y | 생성 시각 |
| updatedAt | datetime | Y | 수정 시각 |

### Item

| 필드 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| id | string | Y | 아이템 고유 ID |
| name | string | Y | 아이템 이름 |
| description | string | N | 아이템 설명 |
| status | string | Y | active, pending, archived, draft |
| ownerId | string | N | 담당자 User ID |
| ownerName | string | N | 표시용 담당자 이름 |
| tags | string[] | N | 분류 태그 |
| notes | string | N | 추가 메모 |
| createdAt | datetime | Y | 생성 시각 |
| updatedAt | datetime | Y | 수정 시각 |

### Activity

| 필드 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| id | string | Y | 활동 고유 ID |
| itemId | string | Y | 대상 아이템 ID |
| actorId | string | Y | 수행 사용자 ID |
| action | string | Y | created, updated, deleted, statusChanged |
| message | string | N | 활동 표시 메시지 |
| createdAt | datetime | Y | 활동 시각 |

### UserSettings

| 필드 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| userId | string | Y | 사용자 ID |
| defaultView | string | Y | dashboard 또는 items |
| theme | string | Y | system, light, dark |
| notificationsEnabled | boolean | Y | 알림 사용 여부 |
| updatedAt | datetime | Y | 수정 시각 |

## 3. 관계

```text
User 1 -> N Item
User 1 -> N Activity
Item 1 -> N Activity
User 1 -> 1 UserSettings
```

관계 설명:

- `Item.ownerId`는 `User.id`를 참조합니다.
- `Activity.itemId`는 `Item.id`를 참조합니다.
- `Activity.actorId`는 `User.id`를 참조합니다.
- `UserSettings.userId`는 `User.id`를 참조합니다.

## 4. 데이터 보존 정책

- 사용자 데이터 보존 기간: 운영 정책 확정 전까지 미정
- 아이템 삭제 정책: MVP에서는 hard delete를 기본으로 하되, 향후 soft delete 검토
- 활동 기록 보존: 삭제된 아이템과 연결된 활동 기록 처리 정책은 추후 확정
- 백업 정책: 운영 데이터베이스 선정 후 확정

## 5. 마이그레이션 고려사항

- 초기 스키마는 `User`, `Item`, `Activity`, `UserSettings`를 기준으로 생성합니다.
- `status`, `role`, `theme`는 enum 또는 제한된 문자열 값으로 관리합니다.
- 향후 실제 도메인명이 확정되면 `Item` 테이블/모델명을 변경하거나 별도 도메인 모델로 교체합니다.
