## 1. 인덱스 최적화
### 인덱스란?
인덱스는 데이터베이스에서 데이터를 빠르게 검색하고 조회하기 위한 자료 구조입니다. 인덱스는 특정 칼럼 또는 칼럼의 조합에 대한 값과 해당 값이 존재하는 테이블 내의 물리적인 위치를 매핑합니다. 이를 통해 데이터베이스는 인덱스를 사용하여 효율적으로 데이터를 검색하고 필요한 정보를 빠르게 가져올 수 있습니다.

인덱스는 데이터베이스의 성능을 향상시키는 데 중요한 역할을 합니다. 인덱스를 사용하면 데이터베이스는 전체 테이블을 스캔하지 않고도 원하는 데이터를 찾을 수 있으므로, 쿼리의 실행 속도를 향상시킬 수 있습니다. 인덱스는 주로 SELECT 문에서 WHERE 절에 사용되는 칼럼이나 JOIN 연산에 사용되는 칼럼에 생성하는 것이 일반적입니다.

### 인덱스 최적화 방법
- 적절한 인덱스 설계: 인덱스를 설계할 때는 데이터의 특성과 접근 패턴을 고려해야 합니다. 자주 검색되는 열이나 조건으로 사용되는 열에 인덱스를 생성하는 것이 좋습니다. 또한, 인덱스의 크기를 최소화하고 중복된 인덱스를 제거하여 성능을 최적화할 수 있습니다.
- 인덱스 통계 유지: 데이터베이스는 쿼리 실행 계획을 수립할 때 인덱스의 통계 정보를 사용합니다. 따라서 인덱스의 통계 정보를 주기적으로 업데이트하여 최신 정보를 유지해야 합니다.
- 인덱스의 갱신 비용 고려: 인덱스를 생성하면 데이터의 갱신 작업(삽입, 수정, 삭제)에 대한 성능 저하가 발생할 수 있습니다. 따라서 인덱스의 갱신 비용을 고려하여 필요한 인덱스만 생성해야 합니다.
- 쿼리의 재작성과 최적화: 쿼리의 실행 계획을 분석하고 쿼리를 재작성하여 인덱스를 최대한 활용할 수 있도록 해야 합니다. 불필요한 조인이나 조건식을 제거하고, 인덱스 스캔을 인덱스 범위 스캔으로 변경하는 등의 최적화 작업을 수행할 수 있습니다.


## 2. 의존성이란? Nest.js에서 의존성을 왜 쓰는가? 자동으로 되어있는 이유? 어떤 구조?

의존성(Dependency)은: 어떤 객체가 동작하기 위해 다른 객체를 필요로 하는 관계
Service 혼자 DB 접근 못 하고 > Repository가 필요하니까 > Repository에 의존한다는 뜻.

의존성 없이 직접 생성하면 문제점 : 결합도 증가, 테스트 어려움, 교체 어려움...
DI(Dependency Injection) : 객체가 필요한 의존성을 직접 생성하지 않고, 외부에서 주입받는 방식

nest에서는... 
@Injectable()
export class UserService {
  constructor(
    private readonly repo: UserRepository
  ) {}
}

-> 처럼 자동으로 만들어줌. Nest에 내부적으로 IoC Container(Inversion of Control Container)가 있기 때문. (제어권이 뒤집혔다~)

Nest 내부 구조
데코레이터 수집 -> 메타데이터 분석 -> Provider Container 등록 -> 생성 시 의존성 해결
Container가 그래프처럼 의존성 연결

기본적으로 의존성 그래프 구조로 동작함 
예시. 
Controller
  ↓
Service
  ↓
Repository

Nest는 이 관계를 분석해서:

순서대로 생성
singleton 관리
순환 참조 검사

등 수행함.
 


## 3. 동시성이란? 동시성 해결 방법
블로그에 정리함 
 

## 4. RESTFUL API란?
- 웹의 설계 원칙인 REST를 따르는 API 설계 방식. 
- 서버에 있는 데이터를 자원으로 보고, HTTP가 가지고 있는 기능(GET, POST..)을 그대로 활용해서 일관성 있게 데이터를 주고받자는 철학  
- Representational State Transfer
REST is an architectural style for designing web APIs.
URL represents a Resource, while HTTP Methods represent Actions.

Resource -> HTTP Method -> Representation 
Because a URL represents a resource, it should generally be a noun, not a verb.
Actions should be expressed through HTTP methods rather than the URL itself.

An operation is idempotent if making the same request multiple times produces the same final state on the server.
| Method | Meaning                     | Idempotent |
| ------ | --------------------------- | ---------- |
| GET    | Retrieve                    | O          |
| POST   | Create                      | X          |
| PUT    | Replace an entire resource  | O          |
| PATCH  | Partially update a resource | Usually O  |
| DELETE | Remove a resource           | O          |

### GET : Retrieve a resource.
- Does not modify server data.
- Produces the same result even when called multiple times.
- Can be cached.
- Is idempotent. : returns the same resource without changing the server state.


### Post : Create a new resource.
- Creates a new resource.
- May produce a different result with each request.
- Is not idempotent.


### PUT : Replace an entire resource.
- Updates the entire resource.
- Repeating the same request produces the same final state.
- Is idempotent.

### PATCH : Partially update a resource. 
- Updates only specified fields. 
- Usually considered idempotent, depending on implementation

### DELETE : Remove a resource. 
- Deletes the specified resource.
- Repeating the same request results in the same final state.
- Is idempotent. 

## 5. POST와 GET의 차이 
GET

주로 조회 할때 사용된다
요청데이터를 URL에 파라메터로 담아서 전송한다
URL에 데이터를 포함해서 전달하므로 전송하는 길이에 제한이있다
정적 컨텐츠를 요청하면, 브라우저가 요청을 캐싱해두고 동일 요청이 들어오면 캐싱된 데이터를 전송


POST

서버의 상태나 데이터를 생성할때 상용된다
요청 정보를 BODY에 담는다
요청 BODY는 길이 제한이 없기 때문에 GET과 달리 대용량 데이터 전송 가능
요청 헤더의 Content-Type에 요청 데이터의 타입을 표시해야한다

참고 : https://dev-coco.tistory.com/60

## 6. HTTP와 HTTPS의 차이 
암호화 : encryption

HTTP HTTP는 인터넷에서 웹 서버와 사용자 측의 웹 브라우저 사이에 문서를 전송하기 위한 프로토콜이다 인터넷에서 HyperText를 전송하기 위해 사용하는 프로토콜이다

HTTP의 문제점 HTTP는 정보를 단순 텍스트로 주고 받기 때문에 (암호화X) N/W상에서 전송 신호를 인터셉트하는 경우 데이터 유출이 발생할수있다.

HTTPS

SSL이나 TLS 프로토콜을 사용하여 데이터를 암호화한다
HTTPS는 초기에 C,S가 통신을 하며 암호화 키를 서로 안전하게 주고 받는다.(SSL Handshake)
이때 암호화 키값이 노출되지 않도록 안전하게 해주는게 https 서버 인증서이다.

- SSL(보안 소켓 계층)을 사용함으로써 서버와 브라우저 사이에 안전하게 암호화된 연결을 만든다 - TLS(전송 계층 보안) 프로토콜을 통해서도 보안을 유지한다. - TLS는 데이터 무결성을 제공하기 때문에 데이터가 전송 중에 수정,손상되는것을 방지한다


## 7. JOIN이란?

조인이란

한 데이터베이스 내의 여러 테이블의 레코드를 조합하여 하나의 열로 표현한 것이다.
따라서 조인은 테이블로서 저장되거나, 그 자체로 이용할 수 있는 결과 셋을 만들어 낸다.


조인의 필요성

관계형 데이터베이스의 구조적 특징으로 정규화를 수행하면 의미 있는 데이터의 집합으로 테이블이 구성되고 각 테이블끼리는 관계(Relationship)을 갖게 된다.
이와 같은 특징으로 관계형 데이터베이스는 저장 공간의 효율성과 확장성이 향상되게 된다.
다른 한편으로는 서로 관계있는 데이터가 여러 테이블로 나뉘어 저장되므로 각 테이블에 저장된 데이터를 효과적으로 검색하기 위해 조인이 필요하다.

 
## 8. JWT

- JSON Web Token : `xxxxx.yyyyy.zzzzz` 형태. → `Header.Payload.Signature`<br>
- JSON 형태의 정보를 Base64로 인코딩해서 서명(Signature)한 문자열이다.

1. Header : 어떤 알고리즘으로 서명했는지?<br>

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
````

2. Payload : 사용자 정보<br>

```json
{
  "userId": 1,
  "role": "USER",
  "exp": 1780000000
}
```

3. Signature : 위조 여부를 확인하는 서명<br>

```text
Header + Payload
↓
비밀키로 서명
↓
Signature
```

그래서 누군가 Payload를 마음대로 바꾸면, 서명이 달라져서 검증에 실패한다.
 
### 장점 
- Stateless하여 서버가 로그인 상태를 저장하지 않아도 된다.
- 서버 확장(Scale-out)과 로드 밸런싱에 유리하다.
- 별도의 세션 저장소(Redis 등)가 없어도 인증이 가능하다.
- 모바일 앱, SPA 등 다양한 클라이언트 환경에서 사용하기 쉽다.
- 여러 서버 간 인증 정보를 공유하기 편리하다.


### 단점 
- 발급된 토큰을 즉시 무효화하기 어렵다.
- 강제 로그아웃 구현 시 Redis 등의 블랙리스트 관리가 필요할 수 있다.
- Payload가 노출되므로 민감한 정보를 저장하면 안 된다.
- 토큰 크기가 커질수록 네트워크 오버헤드가 증가한다.
- 만료 시간(Expiration) 관리 전략이 필요하다.

### 언제 사용하면 좋을까? 
- MSA(Microservice Architecture) 환경
- 여러 서버로 수평 확장이 필요한 서비스
- 모바일 앱과 웹이 동일한 인증 체계를 사용해야 하는 경우
- SPA(React, Vue 등) 기반 서비스
- 서버가 사용자 세션 상태를 최소화하고 싶은 경우

관련 개념:
Access Token
Refresh Token
OAuth 2.0
Bearer Token
JWS / JWE

### localStorage vs Cookie 차이?
-> 저장 위치, 자동 전송 여부, 보안 특성
localStorage는 구현은 쉽지만 XSS에 약하다.
Cookie는 자동 전송되고 보안 옵션을 줄 수 있지만 CSRF에 주의해야 한다.
*CSRF : Cross-Site Request Forgery (사이트 간 요청 위조)
사용자가 의도하지 않았는데, 이미 로그인된 상태를 이용해서 악성 요청을 보내게 만드는 공격이야. -> “브라우저가 자동으로 쿠키를 보내는 특성”을 악용하는 것

실무에서는 보통
Access Token은 메모리
Refresh Token은 HttpOnly Cookie
조합을 많이 고려한다.

### 참고 : 쿠키-캐시 : https://dev-coco.tistory.com/61
쿠키와 세션의 차이

쿠키와 세션은 비슷한 역할을 하며, 동작 원리도 비슷하다. 그 이유는 세션도 결국 쿠키를 사용하기 때문이다.
큰 차이점은 사용자의 정보가 저장되는 위치이다. 쿠키는 서버의 자원을 전혀 사용하지 않으며, 세션은 서버의 자원을 사용한다.
보안 면에서 세션이 더 우수하며,
쿠키는 클라이언트 로컬에 저장되기 때문에 변질되거나 request에서 스니핑 당할 우려가 있어서 보안에 취약하지만
세션은 쿠키를 이용해서 session-id만 저장하고 그것으로 구분하여 서버에서 처리하기 때문에 비교적 보안성이 높다.
라이프 사이클은 쿠키도 만료기간이 있지만 파일로 저장되기 때문에 브라우저를 종료해도 정보가 유지될 수 있다. 또한 만료기간을 따로 지정해 쿠키를 삭제할 때까지 유지할 수도 있다.
반면에 세션도 만료기간을 정할 수 있지만, 브라우저가 종료되면 만료기간에 상관없이 삭제된다.
속도 면에서 쿠키가 더 우수하며,
쿠키는 쿠키에 정보가 있기 때문에 서버에 요청 시 속도가 빠르고 
세션은 정보가 서버에 있기 때문에 처리가 요구되어 비교적 느린 속도를 낸다.

보통 쿠키와 세션의 차이에 대해서 저장 위치와 보안에 대해서는 잘 알고 있지만, 사실 가장 중요한 것은 라이프사이클이다.

Q. 세션을 사용하면 좋은데 왜 쿠키를 사용할까?
A. 세션이 쿠키에 비해 보안이 높은 편이나 쿠키를 사용하는 이유는 세션은 서버에 저장되고, 서버의 자원을 사용하기에 서버 자원에 한계가 있고, 속도가 느려질 수 있기 때문에 자원관리 차원에서 쿠키와 세션을 적절한 요소 및 기능에 병행 사용하여 서버 자원의 낭비를 방지하며 웹사이트의 속도를 높일 수 있다.

쿠키와 세션 그리고 캐시(Cache)?
캐시(Cache)는 웹 페이지 요소를 저장하기 위한 임시 저장소이고, 
쿠키/세션은 정보를 저장하기 위해 사용된다.
캐시는 웹 페이지를 빠르게 렌더링 할 수 있도록 도와주고, 
쿠키/세션은 사용자의 인증을 도와준다.

캐시는 이미지, 비디오, 오디오, css, js파일 등 데이터나 값을 미리 복사해 놓는 리소스 파일들의 임시 저장소이다.
저장 공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다.
같은 웹 페이지에 접속할 때 사용자의 PC에서 로드하므로 서버를 거치지 않아도 된다.
이전에 사용된 데이터가 다시 사용될 가능성이 많으면 캐시 서버에 있는 데이터를 사용한다.
그래서 다시 사용될 확률이 있는 데이터들이 빠르게 접근할 수 있어진다. (페이지의 로딩 속도 ↑)
캐시 히트(hit) : 캐시를 사용할 수 있는 경우 (ex. 이전에 왔던 요청이랑 같은 게 왔을 때)
캐시 미스(miss) : 캐시를 사용할 수 없는 경우 (ex. 웹서버로 처음 요청했을 때)


출처: https://dev-coco.tistory.com/61 [슬기로운 개발생활:티스토리]

### HttpOnly Cookie를 사용하는 이유는?
JS로 토큰을 못 읽게 해서 XSS로 인한 탈취 위험을 줄이기 위해서
HttpOnly가 붙은 쿠키는:

브라우저가 HTTP 요청에는 자동으로 넣어주지만
프론트엔드 JS에서 document.cookie로 읽을 수 없음

JWT 자체는 암호화가 아니라 서명된 토큰이라서, 탈취되면 만료 전까지 악용 가능
그래서 토큰을 브라우저 JS가 직접 만질 수 있게 두는 건 위험도가 올라간다.

HttpOnly Cookie가 만능은 아니다.

XSS는 줄일 수 있지만
쿠키는 자동 전송되니까 CSRF 위험은 남을 수 있음

HttpOnly Cookie는 JavaScript에서 접근할 수 없기 때문에 XSS로 인한 토큰 탈취를 방지하는 데 유리합니다. 다만 쿠키는 자동 전송되므로 CSRF 대응도 함께 고려해야 합니다.

- JWT가 탈취되었을 때 대응 방법은?
1. Access Token 만료 시간을 짧게 한다 : 토큰이 탈취되더라도 사용 가능한 시간을 줄인다 -> 너무 짧으면 재인증/재발급이 자주 발생함
2. Refresh Token을 별도로 관리한다 : Access Token은 짧게, Refresh Token으로 다시 발급받게 한다. -> Refresh Token이 탈취되면 더 위험함
3. Refresh Token Rotation 적용 : 리프레시 토큰을 재발급할 때마다 기존 토큰을 폐기하고 새 토큰을 준다 -> 탈취된 예전 Refresh Token 재사용 감지 가능, 재사용이 탐지되면 해당 세션 전체를 차단할 수 있음
4. 서버에서 토큰 블랙리스트 관리 : 특정 토큰의 jti 같은 식별자를 Redis 등에 저장해서 무효화할 수 있음.
5. 토큰 서명키 회전(Key Rotation) : 서버의 JWT 서명 키가 유출된 경우 전체 체계가 위험해질 수 있으니 키를 교체해야 함.

   
- 강제 로그아웃 구현하는 방법? : “서버가 이 사용자의 토큰을 더 이상 인정하지 않게 만드는 것”
  JWT는 원래 stateless라서, 그냥 발급된 토큰 자체만 보면 서버가 즉시 무효화하기 어렵다
  1. Refresh Token 삭제
  2. Access Token 블랙리스트
  3. 사용자별 토큰 버전 관리
  4. 세션 ID 기반으로 서버 추적

  

- JWT의 크기가 커지면 어떤 문제가 있나요?
JWT는 보통 요청마다 헤더에 실리기 때문에 크기가 커질수록 매 요청 오버헤드가 누적
1. 네트워크 트래픽 증가
2. HTTP 헤더 크기 제한에 걸릴 수 있음.
3. 캐시 효율 저하 / 성능 저하
4. 민감정보 노출 위험 증가


- 대규모 서비스에서 Refresh Token 관리 방법은?
누구의 어떤 디바이스 토큰인지 추적 가능해야 한다
1. Redis에 저장
2. DB에 저장
3. 토큰 원문 저장 대신 해시 저장
4. 디바이스 단위 관리
5. Refresh Token Rotation + 재사용 탐지


## 9. 미들웨어
미들웨어는 양 쪽을 연결하여 데이터를 주고 받을 수 있도록 중간에서 매개 역할을 하는 소프트웨어, 네트워크를 통해서 연결된 여러 개의 컴퓨터에 있는 많은 프로세스들에게 어떤 서비스를 사용할 수 있도록 연결해 주는 소프트웨어를 말한다. 3계층 클라이언트/서버 구조에서 미들웨어가 존재한다. 웹 브라우저에서 데이터베이스로부터 데이터를 저장하거나 읽어올 수 있게 중간에 미들웨어가 존재하게 된다.


## 10. Some Terminologies
- MSA : Microservice Architecture 하나의 큰 서비스를 여러 개의 독립적인 작은 서비스로 분리해 개발·배포·운영하는 아키텍처 스타일
- MSK : Managed Streaming for Apache Kafka AWS에서 제공하는 Apache Kafka 관리형 서비스. Kafka 클러스터 구축/운영을 AWS가 대신 관리해줌.
- CCB : Change Control Board (변경 심의 위원회)
- SCM : Source Code Management Git 같은 형상관리/소스코드 관리 시스템 의미.

## 11. RPC (Remote Procedure Call) : “다른 서버의 함수를 마치 내 함수처럼 호출하는 방식”
로컬 함수 호출처럼 보이지만, 실제로는 네트워크 요청

| REST     | RPC         |
| -------- | ----------- |
| 리소스 중심   | 함수 중심       |
| JSON 많음  | protobuf 많음 |
| 사람 읽기 쉬움 | 성능 좋음       |
| 브라우저 친화적 | 서버간 통신 최적   |
| 범용 API   | 내부 시스템      |

protobuf : 구글이 만든 데이터 직렬화 포멧 -> 객체 데이터를 매우 작고 빠른 binary 형태로 변환하는 기술
json은 문자열 파싱 필요하지만, protobuf는 binary로 보내기 때문에 매우 빠름 
Rest + JSON은 사람 친화적이고 protobuf는 컴퓨터 친화적 
MSA + AI + 실시간 시스템 때문에 네트워크 비용, latency(요청 보내고 응답 받을 때까지 걸리는 시간), serialization cost(객체를 네트워크로 보낼 수 있는 형태로 변환하는 비용)가 중요해져서 protobuf + gRPC 조합이 많이 쓰인다. 

아 직렬화가 serialization 역직렬화는 deserialization
대규모 시스템 최적화는 결국 얼마나 빨리/작게/적은 cpu로 데이터를 주고받느냐 싸움~!! 



|       | gRPC              | tRPC             |
| ----- | ----------------- | ---------------- |
| 목적    | 고성능 서버 통신         | TS 풀스택 개발        |
| 타입 정의 | protobuf          | TypeScript 타입 추론 |
| 언어    | 다국어               | TypeScript only  |
| 전송    | HTTP/2 + protobuf | HTTP + JSON      |
| 성능    | 매우 빠름             | 일반 HTTP 수준       |
| 브라우저  | 불편                | 매우 편함            |
| 사용처   | MSA 내부            | Next.js/BFF      |
| 특징    | infra-oriented    | DX-oriented      |


## 12. Redis 
Redis(Remote Dictionary Server)는 메모리 기반의 NoSQL 데이터 저장소입니다.
데이터를 RAM에 저장하기 때문에 디스크 기반 DB보다 매우 빠른 읽기/쓰기 성능을 제공합니다.
이를 통해: DB 부하 감소, 응답 속도 향상, 대용량 트래픽 처리가 가능합니다.

Redis vs RDBMS
| Redis        | RDBMS    |
| ------------ | -------- |
| In-memory    | Disk 기반  |
| 매우 빠름        | 상대적으로 느림 |
| Key-Value    | Table 기반 |
| NoSQL        | SQL      |
| 복잡한 Join 어려움 | Join 가능  |

Redis는 빠른 데이터 접근에 강점이 있고,
RDBMS는 영속성과 복잡한 관계 데이터 처리에 강점이 있습니다.

Redis가 빠른 이유
- 데이터를 RAM에 저장
- Single Thread Event Loop 구조
  멀티 스레드 환경에서는 락 관리 비용이 발생, 싱글 스레드 기반으로 설계하여 컨텍스트 스위칭 제거, 락 오버헤드 제거(일부 네트워크 부분은 멀티스레드 활용)
- 단순한 Key-Value 구조
디스크 I/O가 발생하지 않아 매우 빠르게 데이터를 처리할 수 있습니다.

Redis 자료구조
string, list, hash, set, sorted set(실시간 랭킹 시스템)

- Cache Aside Pattern : 가장 많이 사용하는 캐싱 패턴으로 캐시 미스 시에만 db 조회하고 redis 에 저장 
- Write Through : 데이터 저장 시 redis, db 동시에 반영하는 방식, 일관성이 높다.
- Cache Stampede : 캐시가 동시에 만료되어 수많은 요청이 한 번에 db로 몰리는 현상. TTL 랜덤 적용, 뮤텍스 락, 프리워밍으로 해결 
- Cache Penetration : 존재하지 않는 데이터에 대한 요청이 반복되어 매번 DB 조회가 발생하는 현상 : null 캐시, 블룸 필터로 해결
- Cache Avalanche : 대량의 캐시가 동시에 만료되는 현상 TTL 랜덤화, 다중 캐시로 해결
- Redis Persistence : 메모리에 저장된 데이터를 디스크에 백업하는 기능 -> RDB, AOF 방식이 있다.
  
| RDB      | AOF       |
| -------- | --------- |
| Snapshot | 명령어 로그    |
| 복구 빠름    | 데이터 유실 적음 |
| 파일 작음    | 파일 큼      |
| 성능 좋음    | 성능 낮음     |

- Redis Pub/Sub : Publisher가 메시지를 발행하면 Subscriber가 실시간으로 수신하는 구조
  publisher -> redis -> subscriber 구조로 채팅, 알림에 활용됨. but 메시지 영속성이 없어서 subscriber 연결이 끊겨 있으면 메시지 수신 불가
- Redis 분산 락 : 멀티 서버 환경에서 동시성 문제 해결하기 위해 사용 SETNX 명령어 활용하여 여러 서버가 동시에 같은 자원을 수정하지 못하게.
- Redis TTL : time to live (데이터 만료 시간)

주로 다음 용도로 사용됩니다.
- 캐싱(Cache)
- 세션 저장소
- 실시간 랭킹 시스템
- 분산 락
- Pub/Sub
- 메시지 브로커

## 13. PostgreSQL
오픈소스 관계형 데이터베이스(RDBMS)로, SQL 표준을 잘 준수하며 안정성과 확장성이 뛰어난 데이터베이스
MVCC : Multi Version Concurrency Control -> 락을 최소화하면서 동시성 처리 가능
트랜잭션 강하고 데이터 무결성 좋고 확장성 높고 복잡한 쿼리 강함


## 14. MongoDB
문서(Document) 기반 NoSQL 데이터베이스
BSON 문서로 데이터를 저장함
복잡한 join 어렵고 정합성 관리 어려움. 스키마는 유연


## REST API 멱등성 (Idempotency)
같은 요청을 여러 번 보내도 결과가 한 번 보낸 것과 동일한 성질
An operation is idempotent if making the same request multiple times produces the same result as making it once.
| Method | 멱등성       |
| ------ | --------- |
| GET    | O         |
| PUT    | O         |
| DELETE | O         |
| POST   | X         |
| PATCH  | 상황에 따라 다름 |



## Threading 
프로세스 내부에서 실제 작업을 수행하는 실행 단위 
* Process vs Thread
Process : 운영체제가 실행 중인 프로그램, 각 프로세스는 독립적인 메모리 공간을 가짐
Thread : 프로세스 안에서 일하는 작업자, 스레드끼리는 메모리 공유
멀티 스레드/싱글 스레드
결국 메모리 공유로 인한 race condition(동시에 가은 자원에 접근해서 예상치 못한 결과 발생)하는게 문제점
해결은 mutex/lock/deadlock...인데 동시성 해결 방법 블로그에 써놨으니 까먹지 않게 항상공부

- Thread
A thread is the smallest unit of execution within a process.

- Race Condition
A race condition occurs when multiple threads access and modify shared data concurrently, leading to unpredictable results.

- Deadlock
A deadlock occurs when two or more threads wait indefinitely for resources held by each other.


## 캐싱
자주 사용하는 데이터를 미리 저장해두고, 다음 요청 떄 DB 대신 빠르게 꺼내 쓰는 기술
캐시에 있으면 Cache Hit, 캐시에 없으면 Cache Miss 
실무에서 캐싱은 거의 Redis
특징으로는 메모리에 저장, 엄청 빠름, Key-Value 구조 


## 객체지향
객체 간 상호작용으로 문제를 해결하는 패러다임
1. 캡슐화 Encapsulation : 외부에서 수정하지 못하도록 데이터를 보호 
2. 상속 Inheritance : 기존 클래스 기능을 재사용
3. 다형성 Polymorphism : 같은 인터페이스로 다른 동작을 하도록
4. 추상화 Abstraction : 복잡한 구현 숨기기

장점: 재사용성 확장성 유지보수성 캡슐화 
단점 : 설계 복잡, 과도한 추상화

상속보다 조합을 선호하는 이유 : 상속은 클래스 간 결합도를 높이고, 변경에 취약하기 때문에, 더 유연하고 확장성이 좋은 조합을 선호 


## 싱글톤 (Singleton Pattern)

- 클래스의 인스턴스를 하나만 생성하고, 애플리케이션 전체에서 해당 인스턴스를 공유하여 사용하는 디자인 패턴이다.
- 객체를 한 번만 생성하기 때문에 메모리 사용량을 줄일 수 있고, 공통 상태나 설정을 관리하는 객체(DB 커넥션 풀, 캐시, 설정 객체, 로거 등)에 자주 사용된다.
- 일반적으로 생성자를 외부에서 호출하지 못하도록 `private`으로 제한하고, `getInstance()` 또는 DI 컨테이너(Spring Bean, NestJS Provider 등)를 통해 유일한 인스턴스에 접근한다.

### 장점

- 객체를 한 번만 생성하므로 메모리 사용량 감소
- 전역적으로 동일한 객체를 공유 가능
- 공통 설정, 캐시, 로거 등을 중앙에서 관리 가능
- 객체 생성 비용이 큰 경우 성능상 이점

### 단점

- 여러 스레드가 동일한 객체를 공유하므로 동시성 문제 발생 가능
- 전역 상태(Global State)를 가지기 쉬워 객체 간 결합도가 높아질 수 있음
- 의존성이 숨겨져 테스트 및 유지보수가 어려워질 수 있음

### 멀티스레드 환경에서 동시성 문제가 발생하는 이유

- 싱글톤 자체가 문제인 것이 아니라 여러 스레드가 같은 객체의 상태(State)를 동시에 수정할 때 문제가 발생한다.
- 싱글톤은 객체가 하나뿐이므로 Race Condition(경쟁 상태)이 발생할 수 있다.
- 또한 싱글톤 생성 과정에서도 여러 스레드가 동시에 접근하면 인스턴스가 여러 개 생성될 수 있다.

### 해결 방법

#### synchronized

- 한 번에 하나의 스레드만 접근하도록 보장
- 구현이 단순함
- 매번 Lock을 획득해야 하므로 성능 저하 가능

#### Double Checked Locking (DCL)

- 인스턴스 생성 시에만 Lock 사용
- synchronized보다 성능이 좋음
- Java에서는 `volatile`과 함께 사용

#### Eager Initialization

- 애플리케이션 시작 시 미리 인스턴스 생성
- 구현이 가장 단순하고 Thread-Safe
- 사용하지 않아도 메모리 점유

### NestJS에서는?

- NestJS Provider는 기본적으로 Singleton Scope이다.
- Node.js는 기본적으로 Single Thread Event Loop 기반이므로 Java와 같은 전통적인 멀티스레드 Race Condition은 상대적으로 적다.
- 하지만 Provider 인스턴스는 모든 요청이 공유하므로 요청별 상태를 필드에 저장하면 문제가 발생할 수 있다.
- 따라서 NestJS의 Singleton Provider는 Stateless하게 설계하는 것이 일반적이다.
