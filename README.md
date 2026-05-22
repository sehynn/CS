1. 의존성을 왜쓰는가? Nest.js 의존성 주입 방법
   
1) 결합도(Coupling) 감소객체 내부에서 new 키워드로 직접 다른 객체를 생성하면, 해당 객체와 강하게 결합됩니다.
   의존성을 외부에서 주입받으면 클래스 내부의 수정 없이 외부에서 교체할 수 있습니다.
   
2) 테스트의 용이성
   직접 객체를 생성하는 코드는 분리하기 어려워 단위 테스트가 힘듭니다.
   의존성을 주입받으면, 테스트 시에는 실제 객체 대신 가짜 객체(Mock)를 주입하여 독립적인 테스트가 가능합니다.
  
3) 유연성과 재사용성 증가인터페이스를 통해 의존성을 주입하도록 설계하면, 기능 변경이나 확장 시 기존 코드를 수정할 필요 없이 새로운 구현체만 갈아 끼우면 됩니다.
  
4) 단일 책임 원칙(SRP) 준수객체는 자신의 핵심 비즈니스 로직에만 집중하고, 객체 생성 및 관리의 책임은 외부로 위임하여 코드가 훨씬 깔끔해집니다.

Module
기능 단위로 묶는 기본 단위. 컨트롤러, 서비스 등을 포함
@Module({ controllers: [], providers: [] })


Controller
HTTP 요청을 받아 라우팅 처리
@Controller('user'), @Get()


Service
비즈니스 로직 담당. Controller와 분리
@Injectable() 사용


Decorator
클래스/메서드에 메타데이터를 부여
@Get(), @Param(), @Body() 등


Dependency Injection (DI)
의존 객체를 직접 생성하지 않고 자동 주입
생성자에 의존성 선언만으로 주입됨


Pipe
요청 데이터의 검증 및 변환
ValidationPipe, ParseIntPipe 등


Guard
요청 차단 또는 허용 (인증/권한 검사)
@UseGuards(AuthGuard('jwt'))


Interceptor
요청/응답을 가로채어 공통 처리 (로깅, 캐싱 등)
NestInterceptor 구현


Exception Filter
예외 발생 시 커스텀 응답 처리
@Catch(HttpException)


Middleware
요청 전 처리 로직 (로그, 인증 등)
Express 스타일 req, res, next 사용 
운영 체제와 응용 프로그램, 혹은 클라이언트와 서버 사이에서 서로 다른 시스템 및 애플리케이션을 연결하여 데이터를 교환하고 중개하는 역할을 하는 소프트웨어

https://dev.to/hasanelsherbiny/what-is-middleware-26mk

Why Do we need to use Middleware ?
Request Processing
Response Processing
Cross-Cutting Concerns
Flexibility and Extensibility
Separation of Concerns


Most Common Uses of Middleware
Authentication and Authorization
Error Handling
Logging and Monitoring
Request Parsing and Validation
Compression and Caching
Routing and URL Rewriting

웹소켓 grpc trpc rpc web protocol
3. 인덱스를 최적화 

4. 동시성 문제란? 동시성 문제 해결 방법 

5. AWS
   Iam MSK Cloudfare Cloudwatch

6. AI harness engineerign

7. kafka
   https://aws.amazon.com/ko/what-is/apache-kafka/
