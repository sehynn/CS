출처 : https://velog.io/@480/SIGKILL-vs-SIGTERM-%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%A2%85%EB%A3%8C-%EC%8B%A0%ED%98%B8

우리의 개발 결과물은 리눅스 프로세스로 구동됩니다.
Node.js는 node 프로세스이며, PHP는 nginx 또는 php-fpm 프로세스입니다.

우리가 예기치 않은 예외에 잘 대응했다면 OOM이나 Stack Overflow같은 강제 종료 사유 외에는 스스로 종료되지 않죠.
그러나, 외부에서 프로세스를 종료할 방법이 있습니다.

커널을 통해 프로세스에 신호를 보낼 수 있는데요.


## SIGKILL vs SIGTERM 리눅스 종료 신호
- SIGKILL : 당장 죽어
- SIGTERM : 죽어달라는 요청 -> 거절 / 이것만 끝내고 죽을게요

상황1) 리눅스 서버 종료
리눅스 서버는 서버 종료 명령을 받으면 실행중인 모든 프로세스에 일괄적으로 종료 신호를 보냅니다.
그런데 SIGKILL 을 보낼까요 SIGTERM을 보낼까요?

대략 다음 절차를 거치게 됩니다.

시스템이 shutdown 명령을 받으면,
모든 프로세스에 SIGTERM 신호를 보낸 후
약 5초간 (시스템 설정에 따라 시간은 다를 수 있습니다.) 1초마다 프로세스 종료 여부를 체크하여
시간이 지난 후에도 남아있는 프로세스는 SIGKILL 을 보냅니다.

https://www.computerhope.com/unix/ushutdow.htm

상황2) AWS의 EC2 Spot 인스턴스 종료
하지만 AWS의 저렴한 Spot Instance는 약간 다릅니다.

SIGTERM 신호를 보내기에 앞서 2분 전에 AWS에서 먼저 EC2의 상태를 바꾸게 됩니다.
이것은 스팟인스턴스의 특성으로 리눅스 표준은 아닙니다.

이 신호는 시스템 커널이 보내는 신호가 아니며, 실제로 서버에는 어떤 이벤트도 받지 못합니다.
다만 AWS의 다른 서비스를 통해 알아낼 수 있습니다.

https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/spot-interruptions.html
https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html
