# API 명세

## 1. 기본 정보

- Base URL: TBD
- 인증 방식: TBD
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

### GET /health

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

### TBD

- Method: TBD
- Path: TBD
- 목적: TBD
- 인증: TBD
- Request: TBD
- Response: TBD
- Error: TBD

## 4. 에러 코드

| 코드 | HTTP 상태 | 설명 |
| --- | --- | --- |
| VALIDATION_ERROR | 400 | 요청값이 올바르지 않음 |
| UNAUTHORIZED | 401 | 인증 필요 |
| FORBIDDEN | 403 | 권한 없음 |
| NOT_FOUND | 404 | 리소스 없음 |
| INTERNAL_ERROR | 500 | 서버 오류 |
