# API 명세

## 1. 기본 정보

- Base URL: `/api`
- 인증 방식: 미정. MVP 문서에서는 보호 API가 `requireUser(request)`를 사용한다고 가정합니다.
- 요청 형식: JSON
- 응답 형식: JSON

## 2. 공통 응답 형식

### 성공

```json
{
  "data": {},
  "message": "ok"
}
```

### 실패

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "사용자에게 표시할 메시지"
  }
}
```

## 3. 엔드포인트

### GET /api/health

- 목적: 서버 상태 확인
- 인증: 불필요

#### Response

```json
{
  "data": {
    "status": "ok"
  },
  "message": "ok"
}
```

### GET /api/items

- 목적: 아이템 목록 조회
- 인증: 필요
- Query:
  - `search`: string, optional
  - `status`: string, optional
  - `ownerId`: string, optional
  - `tag`: string, optional
  - `sortKey`: string, optional
  - `sortDirection`: asc 또는 desc, optional
  - `page`: number, optional

#### Response

```json
{
  "data": {
    "items": [
      {
        "id": "item_001",
        "name": "Sample item",
        "description": "Example description",
        "status": "active",
        "ownerId": "user_001",
        "ownerName": "Admin User",
        "tags": ["sample"],
        "createdAt": "2026-06-18T00:00:00.000Z",
        "updatedAt": "2026-06-18T00:00:00.000Z"
      }
    ],
    "page": 1,
    "total": 1
  },
  "message": "ok"
}
```

### POST /api/items

- 목적: 새 아이템 생성
- 인증: 필요

#### Request

```json
{
  "name": "New item",
  "description": "Description",
  "status": "draft",
  "ownerId": "user_001",
  "tags": ["new"],
  "notes": "Optional notes"
}
```

#### Response

```json
{
  "data": {
    "id": "item_002",
    "name": "New item",
    "status": "draft"
  },
  "message": "created"
}
```

### GET /api/items/[itemId]

- 목적: 단일 아이템 상세 조회
- 인증: 필요

#### Response

```json
{
  "data": {
    "item": {
      "id": "item_001",
      "name": "Sample item",
      "description": "Example description",
      "status": "active",
      "ownerId": "user_001",
      "ownerName": "Admin User",
      "tags": ["sample"],
      "notes": "Optional notes",
      "createdAt": "2026-06-18T00:00:00.000Z",
      "updatedAt": "2026-06-18T00:00:00.000Z"
    },
    "activities": []
  },
  "message": "ok"
}
```

### PATCH /api/items/[itemId]

- 목적: 아이템 수정
- 인증: 필요

#### Request

```json
{
  "name": "Updated item",
  "description": "Updated description",
  "status": "active",
  "ownerId": "user_001",
  "tags": ["updated"],
  "notes": "Updated notes"
}
```

#### Response

```json
{
  "data": {
    "id": "item_001",
    "name": "Updated item",
    "status": "active"
  },
  "message": "updated"
}
```

### DELETE /api/items/[itemId]

- 목적: 아이템 삭제
- 인증: 필요
- 권한: 관리자 삭제 권한 필요

#### Response

```json
{
  "data": {
    "itemId": "item_001"
  },
  "message": "deleted"
}
```

## 4. 에러 코드

| 코드 | HTTP 상태 | 설명 |
| --- | --- | --- |
| VALIDATION_ERROR | 400 | 요청값이 올바르지 않음 |
| UNAUTHORIZED | 401 | 인증 필요 |
| FORBIDDEN | 403 | 권한 없음 |
| NOT_FOUND | 404 | 리소스 없음 |
| CONFLICT | 409 | 데이터 상태 충돌 |
| INTERNAL_ERROR | 500 | 서버 오류 |

## 5. 검증 규칙

| 필드 | 규칙 |
| --- | --- |
| name | 필수, 1자 이상 |
| description | 선택, 최대 길이 제한 필요 |
| status | active, pending, archived, draft 중 하나 |
| ownerId | 선택, 존재하는 사용자 ID |
| tags | 선택, 문자열 배열 |
| notes | 선택 |
