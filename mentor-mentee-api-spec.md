# 멘토-멘티 매칭 앱 API 명세서

- 모든 API 엔드포인트는 `http://localhost:8080/api/` 하위 경로를 기준으로 정의했습니다.
- 모든 인증이 필요한 요청에는 반드시 `Authorization: Bearer <token>` 헤더를 포함해야 합니다.
- 모든 요청과 응답은 JSON 형식의 개체를 주고 받아야 합니다.

## 1. 인증 (Authentication)

### POST `/signup`: 회원가입

#### **Request Body**

```jsonc
{
  "email": "user@example.com",
  "password": "password123",
  "name": "김멘토",
  "role": "mentor" // or "mentee"
}
```

#### **Response:**

- `201 Created`
- `400 Bad request`: 요청 payload 형식이 틀렸을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### POST `/login`: 로그인

#### **Request Body:**

```jsonc
{
  "email": "user@example.com",
  "password": "password123"
}
```

#### **Response:**

- `200 OK`

    ```jsonc
    {
      "token": "JWT_TOKEN"
    }
    ```

- `400 Bad request`: 요청 payload 형식이 틀렸을 경우
- `401 Unauthorized`: 로그인에 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

## 2. 사용자 정보

### GET `/me`: 내 정보 조회

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`

    ```jsonc
    // 멘토
    {
      "id": 1,
      "email": "user@example.com",
      "role": "mentor",
      "profile": {
        "name": "Alice",
        "bio": "Frontend mentor",
        "imageUrl": "/images/mentor/1",
        "skills": ["React", "Vue"]
      }
    }
    ```

    ```jsonc
    // 멘티
    {
      "id": 10,
      "email": "user@example.com",
      "role": "mentee",
      "profile": {
        "name": "Alice",
        "bio": "Frontend mentor",
        "imageUrl": "/images/mentee/10"
      }
    }
    ```

- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### GET `/images/:role/:id`: 프로필 이미지

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`: 프로필 이미지 렌더링
- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### PUT `/profile`: 프로필 수정

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Request Body:**

```jsonc
// 멘토
{
  "id": 1,
  "name": "Alice",
  "role": "mentor",
  "bio": "Frontend mentor",
  "image": "BASE64_ENCODED_STRING",
  "skills": ["React", "Vue"]
}
```

```jsonc
// 멘티
{
  "id": 21,
  "name": "Alice",
  "role": "mentee",
  "bio": "Frontend mentee",
  "image": "BASE64_ENCODED_STRING"
}
```

#### **Response:**

- `200 OK`

    ```jsonc
    // 멘토
    {
      "id": 1,
      "email": "user@example.com",
      "role": "mentor",
      "profile": {
        "name": "김앞단",
        "bio": "Frontend mentor",
        "imageUrl": "/images/mentor/1",
        "skills": ["React", "Vue"]
      }
    }
    ```

    ```jsonc
    // 멘티
    {
      "id": 21,
      "email": "user@example.com",
      "role": "mentee",
      "profile": {
        "name": "이뒷단",
        "bio": "Passionate backend developer",
        "imageUrl": "/images/mentee/21"
      }
    }
    ```

- `400 Bad request`: 요청 payload 형식이 틀렸을 경우
- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

## 3. 멘토 리스트 조회

### GET `/mentors`: 멘토 전체 리스트 조회 (멘티 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Query Parameters:**

- `skill=<sill_set>`

  > **NOTE**:
  > 
  > - 한 번에 한 가지 Skill Set만 검색할 수 있습니다. 예를 들어, `react` 또는 `spring` 처럼 키워드 하나만 검색할 수 있지, `react, spring` 처럼 두 개 이상의 키워드를 동시에 검색할 수 없습니다.
  > - 쿼리 파라미터를 제공하지 않으면 전체 멘토 리스트를 반환합니다.

- `order_by=<skill_or_name>`

  > **NOTE**:
  > 
  > - 다수의 멘토를 검색할 경우 skill 또는 name 을 기준으로 멘토 리스트를 오름차순으로 정렬합니다.
  > - 쿼리 파라미터를 제공하지 않으면 mentor ID 기준 오름차순으로 정렬합니다.

#### **Response:**

- `200 OK`

    ```jsonc
    // 조회 결과 없을 경우
    []
    ```

    ```jsonc
    // 조회 결과 있을 경우
    [
      {
        "id": 3,
        "email": "user@example.com",
        "role": "mentor",
        "profile": {
          "name": "김앞단",
          "bio": "Frontend mentor",
          "imageUrl": "/images/mentor/3",
          "skills": ["React", "Vue"]
        }
      },
      {
        "id": 4,
        "name": "이뒷단",
        "role": "mentor",
        "bio": "Backend mentor",
        "imageUrl": "/images/mentor/4",
        "skills": ["Spring Boot", "FastAPI"]
      }
    ]
    ```

- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

## 4. 멘토 매칭 요청

### POST `/match-requests`: 매칭 요청 보내기 (멘티 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Request Body:**

```jsonc
{
  "mentorId": 3,
  "menteeId": 4,
  "message": "멘토링 받고 싶어요!"
}
```

#### **Response:**

- `200 OK`

    ```jsonc
    {
      "id": 1,
      "mentorId": 3,
      "menteeId": 4,
      "message": "멘토링 받고 싶어요!",
      "status": "pending"
    }
    ```

- `400 Bad request`: 요청 payload 형식이 틀렸을 경우 또는 멘토가 존재하지 않을 경우
- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### GET `/match-requests/incoming`: 나에게 들어온 요청 목록 (멘토 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`

    ```jsonc
    [
      {
        "id": 11,
        "mentorId": 5,
        "menteeId": 1,
        "message": "멘토링 받고 싶어요!",
        "status": "pending"
      },
      {
        "id": 12,
        "mentorId": 5,
        "menteeId": 2,
        "message": "멘토링 받고 싶어요!",
        "status": "accepted"
      },
      {
        "id": 13,
        "mentorId": 5,
        "menteeId": 3,
        "message": "멘토링 받고 싶어요!",
        "status": "rejected"
      },
      {
        "id": 14,
        "mentorId": 5,
        "menteeId": 4,
        "message": "멘토링 받고 싶어요!",
        "status": "cancelled"
      }
    ]
    ```

- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### GET `/match-requests/outgoing`: 내가 보낸 요청 목록 (멘티 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`

    ```jsonc
    [
      {
        "id": 11,
        "mentorId": 1,
        "menteeId": 10,
        "status": "pending"
      },
      {
        "id": 12,
        "mentorId": 2,
        "menteeId": 10,
        "status": "accepted"
      },
      {
        "id": 13,
        "mentorId": 3,
        "menteeId": 10,
        "status": "rejected"
      },
      {
        "id": 14,
        "mentorId": 4,
        "menteeId": 10,
        "status": "cancelled"
      }
    ]
    ```

- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### PUT `/match-requests/:id/accept`: 요청 수락 (멘토 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`

    ```jsonc
    {
      "id": 11,
      "mentorId": 2,
      "menteeId": 1,
      "message": "멘토링 받고 싶어요!",
      "status": "accepted"
    }
    ```

- `404 Not found`: 매칭 요청 ID 값이 존재하지 않을 경우
- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### PUT `/match-requests/:id/reject`: 요청 거절 (멘토 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`

    ```jsonc
    {
      "id": 11,
      "mentorId": 2,
      "menteeId": 1,
      "message": "멘토링 받고 싶어요!",
      "status": "rejected"
    }
    ```

- `404 Not found`: 매칭 요청 ID 값이 존재하지 않을 경우
- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우

### DELETE `/match-requests/:id`: 요청 삭제/취소 (멘티 전용)

#### **Request Headers:**

- `Authorization: Bearer <token>`

#### **Response:**

- `200 OK`

    ```jsonc
    {
      "id": 11,
      "mentorId": 2,
      "menteeId": 12,
      "message": "멘토링 받고 싶어요!",
      "status": "cancelled"
    }
    ```

- `404 Not found`: 매칭 요청 ID 값이 존재하지 않을 경우
- `401 Unauthorized`: 인증 실패했을 경우
- `500 Internal server error`: 처리중 에러가 생겼을 경우
