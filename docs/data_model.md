# 데이터 모델

## 1. 엔티티 목록

| 엔티티 | 설명 | 주요 필드 |
| --- | --- | --- |
| User | 사용자 | id, email, name |
| TBD | TBD | TBD |

## 2. 엔티티 상세

### User

| 필드 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| id | string | Y | 사용자 고유 ID |
| email | string | Y | 로그인 이메일 |
| name | string | N | 표시 이름 |
| createdAt | datetime | Y | 생성 시각 |
| updatedAt | datetime | Y | 수정 시각 |

## 3. 관계

```text
User
  -> TBD
```

## 4. 데이터 보존 정책

- 사용자 데이터 보존 기간: TBD
- 삭제 정책: TBD
- 백업 정책: TBD

## 5. 마이그레이션 고려사항

- 초기 스키마 생성: TBD
- 변경 이력 관리 방식: TBD
