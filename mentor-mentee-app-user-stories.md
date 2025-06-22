# 멘토-멘티 매칭 앱 사용자 스토리

## 앱 개요

이 앱은 **멘토와 멘티를 서로 매칭하는 시스템**입니다. 멘토는 자신의 기술 스택과 소개를 등록하고, 멘티는 원하는 멘토에게 매칭 요청을 보낼 수 있습니다. 멘토는 매칭 요청을 수락하거나 거절할 수 있으며, 한 명의 멘토는 동시에 한 명의 멘티와만 매칭할 수 있습니다.

## 기능

### 1. 회원가입

```text
AS a user,
I WANT to navigate to the `/signup` page
SO THAT I can sign up the service.
```

```text
AS a user,
I WANT to create an account using my email address, password and role (either mentor or mentee)
SO THAT I can sign up the service.
```

```text
AS a user,
I WANT to be redirected to the `/` page after completing the sign-up process
SO THAT I can log-in to the service.
```

### 2. 로그인

```text
AS a user,
I WANT to navigate to the `/` page
SO THAT I can be automatically redirected to the `/login` page, if I'm not authenticated.
```

```text
AS a user,
I WANT to navigate to the `/` page
SO THAT I can be automatically redirected to the `/profile` page, if I'm authenticated.
```

```text
AS a user,
I WANT to navigate to the `/login` page
SO THAT I can log-in to the service.
```

```text
AS a user
I WANT to log-in to the service using my email address and password
SO THAT I can be redirected to the `/profile` page.
```

```text
AS a user
I WANT to log-in to the service using my email address and password
SO THAT I can receive a JWT token as an authenticated user.
```

```text
AS a user
I WANT to log-in to the service using my email address and password
SO THAT I can receive a JWT token that contains my details including the email address and role.
```

```text
AS a user having the mentor role
I WANT to log-in to the service using my email address and password
SO THAT I can see the navigation bar containing `/profile` and `/requests`.
```

```text
AS a user having the mentee role
I WANT to log-in to the service using my email address and password
SO THAT I can see the navigation bar containing `/profile`, `/mentors` and `/requests`.
```

### 3. 사용자 프로필

```text
AS a user
I WANT to be able to navigate to the `/profile` page
SO THAT I can see my details.
```

```text
AS a user having the mentor role
I WANT to be able to register my profile data including name, bio, image and tech skillsets
SO THAT mentees can see my details.
```

```text
AS a user having the mentee role
I WANT to be able to register my profile data including name, bio and image
SO THAT mentors can see my details.
```

```text
AS a user having the mentor role
I WANT to be able to show my default image with this URL, https://placehold.co/500x500.jpg?text=MENTOR
SO THAT mentees and I can see my profile image.
```

```text
AS a user having the mentee role
I WANT to be able to show my default image with this URL, https://placehold.co/500x500.jpg?text=MENTEE
SO THAT mentors and I can see my profile image.
```

```text
AS a user
I WANT to be able to upload an image from my local computer
SO THAT everyone can see my profile image.
```

```text
AS a user
I WANT to upload an image with the format of .png or .jpg.
```

```text
AS a user
I WANT to upload an image less than 1MB size
SO THAT I cannot upload the image larger than 1MB size.
```

### 4. 멘토 목록 조회

```text
AS a user having the mentee role
I WANT to navigate to the `/mentors` page
SO THAT I can see the list of the mentors.
```

```text
AS a user having the mentee role
I WANT to enter a search keyword around the tech skillsets on the `/mentors` page
SO THAT I can see the list of the mentors filtered by the keyword.
```

```text
AS a user having the mentee role
I WANT to be able to sort the list of mentors
SO THAT I can see the list of the mentors ordered by mentor name or tech skillsets.
```

### 5. 매칭 요청

```text
AS a user having the mentee role
I WANT to send a request to a mentor
SO THAT I can see the request status at the `/requests` page.
```

```text
AS a user having the mentee role
I WANT to send only one request to one mentor at a time
SO THAT I cannot send requests multiple times until the request is accepted or rejected.
```

```text
AS a user having the mentee role
I WANT to send a request to a mentor with a message
SO THAT the mentor can see my message.
```

### 6. 매칭 요청 수락/거절

```text
AS a user having the mentor role
I WANT to navigate to the `/requests` page
SO THAT I can see the list of requests from mentees.
```

```text
AS a user having the mentor role
I WANT to be able to accept or reject a request
SO THAT the mentee can see the request status updated.
```

```text
AS a user having the mentor role
I WANT to only accept one request from one mentee
SO THAT the other requests from the other mentees are automatically rejected.
```

### 7. 매칭 요청 목록

```text
AS a user having the mentee role
I WANT to see the request status
SO THAT I can check the request status.
```

```text
AS a user having the mentee role
I WANT to cancel a request to a mentor before being accepted or rejected
SO THAT the mentor cannot see my message.
```

## 테스트 가용성 고려사항

다음 HTML 엘리먼트는 반드시 UI 기능 테스트를 위해 ID 값을 갖춰야 합니다.

### 1. 회원가입

- email 인풋 필드: `id`=`email`
- password 인풋 필드: `id`=`password`
- role 인풋 필드: `id`=`role`
- signup 버튼: `id`=`signup`

### 2. 로그인

- email 인풋 필드: `id`=`email`
- password 인풋 필드: `id`=`password`
- login 버튼: `id`=`login`

### 3. 사용자 프로필

- name 인풋 필드: `id`=`name`
- bio 인풋 필드: `id`=`bio`
- skillsets 인풋 필드: `id`=`skillsets`
- profile 사진: `id`=`profile-photo`
- profile 사진 인풋 필드: `id`=`profile`
- save 버튼: `id`=`save`

### 4. 멘토 목록 조회

- 개별 멘토 엘리먼트: `class`=`mentor`
- 멘토 스킬셋 검색 인풋 필드: `id`=`search`
- 멘토 정렬 인풋 (이름): `id`=`name`
- 멘토 정렬 인풋 (스킬셋): `id`=`skill`

### 5. 매칭 요청

- 요청 메시지 인풋 필드: `id`=`message`, `data-mentor-id`=`{{mentor-id}}`, `data-testid`=`message-{{mentor-id}}`
- 요청 상태: `id`=`request-status`
- 요청 버튼: `id`=`request`

### 6. 매칭 요청 수락/거절

- 요청 메시지: `class`=`request-message`, `mentee`=`{{mentee-id}}`
- 수락 버튼: `id`=`accept`
- 거절 버튼: `id`=`reject`

### 7. 매칭 요청 목록
