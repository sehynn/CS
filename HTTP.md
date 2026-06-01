# HTTP vs HTTPS
## 1. Connectionless 프로토콜 (비연결 지향)
  클라이언트가 서버에 요청(Request)을 했을 때, 그 요청에 맞는 응답(Response)을 보낸 후 연결을 끊는 처리방식이다.

HTTP 1.1 버전에서 커넥션을 계속 유지하고, 요청(Request)에 재활용하는 기능이 추가되었다. (HTTP Header)에 keep-alive 옵션을 주어 커넥션을 재활용하게 한다. HTTP 1.1 버전에선 디폴트(default) 옵션이다. HTTP가 TCP위에서 구현되었기 때문에(TCP : 연결 지향, UDP : 비연결 지향) 연결 지향적이라고 할 수 있다는 얘기가 있어 논란이 있지만, 아직까진 네트워크 관점에서 keep-alive는 옵션으로 두고, 서버 측에서 비연결 지향적인 특성으로 커넥션 관리에 대한 비용을 줄이는 것이 명확한 장점으로 보기 때문에 비연결 지향으로 알아두었다.

## 2. Stateless 프로토콜
커넥션을 끊는 순간 클라이언트와 서버의 통신이 끝나며 상태 정보는 유지하지 않는 특성이 있다.

클라이언트와 첫 번째 통신에서 데이터를 주고받았다 해도, 두 번째 통신에서 이전 데이터를 유지하지 않는다. 하지만, 실제로는 데이터 유지가 필요한 경우가 많다.

정보가 유지되지 않으면, 매번 페이지를 이동할 때마다 로그인을 다시 하거나,상품을 선택했는데 구매 페이지에서 선택한 상품의 정보가 없거나 하는 등의 일이 발생할 수 있다. → 따라서, Stateful 경우를 대처하기 위해 쿠키와 세션을 사용한다.      쿠키와 세션의 차이점은 크게 상태 정보의 저장 위치이다.      쿠키는 '클라이언트(=로컬 PC)'에 저장하고, 세션은 '서버'에 저장한다.

서버와 클라이언트가 통신을 할 때 통신이 연속적으로 이어지지 않고 한 번 통신이 되면 끊어진다. 따라서 서버는 클라이언트가 누구인지 계속 인증을 해줘야 한다. 하지만 그것은 매우 귀찮고 번거로운 일이다. 그런 번거로움을 해결하는 방법이 바로 쿠키와 세션이다. 출처: https://dev-coco.tistory.com/61 [슬기로운 개발생활:티스토리]


# HTTP Status Code

## 상태 코드란?

HTTP 상태 코드는 클라이언트가 보낸 요청(Request)에 대해 서버가 처리 결과를 알려주는 3자리 숫자 코드이다.

클라이언트는 상태 코드를 통해 요청이 성공했는지, 인증에 실패했는지, 잘못된 요청인지, 서버에 문제가 발생했는지 등을 확인할 수 있다.

예시:

```http
GET /users/1
```

```http
HTTP/1.1 200 OK
```

위 응답은 서버가 요청을 정상적으로 처리했음을 의미한다.

---

## 상태 코드 분류

| 범위 | 의미 |
|--------|--------|
| 1xx | 정보 응답 |
| 2xx | 성공 |
| 3xx | 리다이렉트 |
| 4xx | 클라이언트 오류 |
| 5xx | 서버 오류 |

---

# 2xx Success

## 200 OK

조회 성공

```http
GET /users/1
→ 200 OK
```

## 201 Created

생성 성공

```http
POST /users
→ 201 Created
```

## 204 No Content

성공했지만 응답 Body 없음

```http
DELETE /users/1
→ 204 No Content
```

---

# 3xx Redirection

## 301 Moved Permanently

영구 이동

- URL이 영구적으로 변경됨

## 302 Found

임시 이동

- URL이 일시적으로 변경됨

---

# 4xx Client Error

## 400 Bad Request

잘못된 요청

```text
필수 파라미터 누락
잘못된 요청 형식
```

---

## 401 Unauthorized

인증 실패

```text
JWT 없음
JWT 만료
```

### 핵심

인증(Authentication) 문제

---

## 403 Forbidden

권한 없음

```text
일반 사용자가 관리자 API 호출
```

### 핵심

인증은 되었지만 권한(Authorization)이 없음

---

## 404 Not Found

리소스를 찾을 수 없음

```text
존재하지 않는 회원
존재하지 않는 API
```

---

## 409 Conflict

현재 상태와 충돌

```text
중복 이메일 가입
이미 처리된 주문
```

---

## 422 Unprocessable Entity

요청 형식은 맞지만 비즈니스 검증 실패

```text
비밀번호 길이 부족
이메일 형식 오류
```

### 400 vs 422

```text
400
요청 자체가 잘못됨

422
요청 형식은 맞음
비즈니스 규칙 위반
```

---

## 429 Too Many Requests

요청 과다

```text
Rate Limit 초과
```

---

# 5xx Server Error

## 500 Internal Server Error

서버 내부 오류

```text
예상하지 못한 예외
DB 오류
```

---

## 502 Bad Gateway

서버 간 통신 실패

```text
Nginx → API 서버 오류
```

---

## 503 Service Unavailable

일시적으로 서비스 불가

```text
서버 점검
서버 과부하
```

---

## 504 Gateway Timeout

서버 간 통신 시간 초과

```text
외부 API 응답 지연
```

---

# 면접 단골 질문

## 401 vs 403

401

- 인증 실패
- 로그인 필요

403

- 인증 성공
- 권한 없음

---

## 400 vs 422

400

- 요청 자체가 잘못됨

422

- 요청 형식은 맞음
- 비즈니스 검증 실패

---

## 500 vs 503

500

- 서버 코드 문제

503

- 서버는 정상
- 점검 또는 과부하

---

# 이것만 외우면 됨

- 200 OK
- 201 Created
- 204 No Content

- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 409 Conflict
- 422 Unprocessable Entity
- 429 Too Many Requests

- 500 Internal Server Error
- 502 Bad Gateway
- 503 Service Unavailable
- 504 Gateway Timeout
