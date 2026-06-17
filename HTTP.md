# HTTP vs HTTPS

HTTP와 HTTPS는 웹 브라우저(클라이언트)와 웹 서버가 데이터를 주고받기 위한 통신 규약입니다. 가장 큰 차이는 데이터 암호화 유무이며, HTTPS는 HTTP에 보안(Secure)이 추가된 버전입니다.

- HTTP: 데이터를 암호화 없이 그대로 전송하기 때문에, 공용 와이파이나 네트워크 환경에서 해커가 통신을 엿듣거나 개인정보를 탈취할 위험이 큽니다.
- HTTPS: SSL/TLS 보안 인증서를 통해 데이터를 암호화하여 전송합니다. 따라서 제3자가 데이터를 가로채더라도 해독이 불가능합니다.

참고 : https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/

HTTP and HTTPS are communication protocols used by web browsers (clients) and web servers to exchange data. The main difference between them is whether the data is encrypted. HTTPS is the secure version of HTTP.

- HTTP: Data is transmitted in plain text without encryption. Therefore, on public Wi-Fi or unsecured networks, attackers may eavesdrop on the communication or steal sensitive information.
- HTTPS: Data is encrypted using SSL/TLS certificates before transmission. As a result, even if a third party intercepts the data, it cannot be easily decrypted or read.
 
## HTTP 프로토콜은 어떻게 작동하나요?
HTTP는 OSI(Open Systems Interconnection) 네트워크 통신 모델의 애플리케이션 계층 프로토콜입니다. HTTP는 여러 유형의 요청과 응답을 정의합니다. 예를 들어, 웹 사이트의 일부 데이터를 보려는 경우 HTTP GET 요청을 전송합니다. 연락처 양식 작성과 같은 일부 정보를 전송하려는 경우 HTTP PUT 요청을 전송합니다.

마찬가지로, 서버는 숫자 코드 및 데이터 양식으로 다양한 유형의 HTTP 응답을 전송합니다. 다음은 몇 가지 예입니다.
'''
200 - OK(정상)
400 - Bad request(잘못된 요청)
404 - Resource not found(리소스를 찾을 수 없음)
이러한 요청 및 응답 통신은 일반적으로 사용자에게 보이지 않습니다. 브라우저와 웹 서버가 사용하는 통신 방식이므로 World Wide Web은 모든 사용자에게 일관되게 작동합니다.
'''

## HTTPS 프로토콜은 어떻게 작동하나요?
HTTP는 암호화되지 않은 데이터를 전송합니다. 즉, 브라우저에서 전송된 정보를 제3자가 가로채고 읽을 수 있습니다. 이는 이상적인 프로세스가 아니었기 때문에, 통신에 또 다른 보안 계층을 추가하기 위해 HTTPS로 확장되었습니다. HTTPS는 HTTP 요청 및 응답을 SSL 및 TLS 기술에 결합합니다.

HTTPS 웹 사이트는 독립된 인증 기관(CA)에서 SSL/TLS 인증서를 획득해야 합니다. 이러한 웹 사이트는 신뢰를 구축하기 위해 데이터를 교환하기 전에 브라우저와 인증서를 공유합니다. SSL 인증서는 암호화 정보도 포함하므로 서버와 웹 브라우저는 암호화된 데이터나 스크램블된 데이터를 교환할 수 있습니다. 프로세스는 다음과 같이 작동합니다.

사용자 브라우저의 주소 표시줄에 https:// URL 형식을 입력하여 HTTPS 웹 사이트를 방문합니다.
브라우저는 서버의 SSL 인증서를 요청하여 사이트의 신뢰성을 검증하려고 시도합니다.
서버는 퍼블릭 키가 포함된 SSL 인증서를 회신으로 전송합니다.
웹 사이트의 SSL 인증서는 서버 아이덴티티를 증명합니다. 브라우저에서 인증되면, 브라우저가 퍼블릭 키를 사용하여 비밀 세션 키가 포함된 메시지를 암호화하고 전송합니다.
웹 서버는 프라이빗 키를 사용하여 메시지를 해독하고 세션 키를 검색합니다. 그런 다음, 세션 키를 암호화하고 브라우저에 승인 메시지를 전송합니다.
이제 브라우저와 웹 서버 모두 동일한 세션 키를 사용하여 메시지를 안전하게 교환하도록 전환합니다.

## HTTPS 설정 비용이 HTTP보다 더 많이 드나요?
HTTPS를 사용하려면 서버에서 SSL/TLS 인증서를 획득하고 유지 관리해야 합니다. 과거에는 대부분의 인증 기관이 인증서 등록 및 유지 관리에 대해 연간 요금을 청구했습니다. 하지만 이제는 더 이상 그렇지 않습니다.

무료 SSL 인증서를 획득할 수 있는 많은 출처가 있습니다. 예를 들어, Amazon Web Services(AWS)에서는 AWS Certificate Manager(ACM)를 제공합니다. ACM은 AWS 서비스 및 내부 연결 리소스와 함께 사용할 수 있는 퍼블릭 및 프라이빗 SSL/TLS 인증서를 프로비저닝, 관리 및 배포합니다. ACM은 SSL/TLS 인증서를 구매, 업로드 및 갱신하는 시간 소모적인 수동 프로세스를 대신 처리해줍니다.



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

# 비교 

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
