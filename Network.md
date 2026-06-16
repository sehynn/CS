## OSI 7계층
- 네트워크 통신 과정을 7단계로 추상화한 참조 모델이다.
- 각 계층이 역할을 분리하여 통신 구조를 이해하고, 문제를 분석하기 쉽도록 만든다.

계층	역할	대표 프로토콜/장비
7. 응용 계층 (Application)	사용자와 직접 상호작용하는 서비스 제공	HTTP, FTP, SMTP, DNS
6. 표현 계층 (Presentation)	데이터 형식 변환, 암호화, 압축	SSL/TLS, JPEG
5. 세션 계층 (Session)	세션 생성·유지·종료	NetBIOS
4. 전송 계층 (Transport)	종단 간 통신, 신뢰성 보장	TCP, UDP
3. 네트워크 계층 (Network)	경로 선택 및 IP 기반 라우팅	IP, ICMP, 라우터
2. 데이터 링크 계층 (Data Link)	물리 주소(MAC) 기반 통신	Ethernet, 스위치
1. 물리 계층 (Physical)	실제 전기 신호 전송	케이블, 허브


## TCP/IP 4계층 
- 실제 인터넷에서 사용되는 네트워크 모델이다.
계층	역할	대표 프로토콜
응용 계층 (Application)	사용자 서비스 제공	HTTP, HTTPS, DNS
전송 계층 (Transport)	프로세스 간 통신	TCP, UDP
인터넷 계층 (Internet)	IP 기반 패킷 전달	IP, ICMP
네트워크 접근 계층 (Network Access)	물리 네트워크 통신	Ethernet, Wi-Fi


## DNS, IP, 포트, NAT
### DNS (Domain Name System)
- 도메인 이름을 IP 주소로 변환해주는 시스템
- ex. google.com -> 142.523.xxx.xxx : 사람이 이해하기 쉬운 도메인을, 컴퓨터가 이해할 수 있는 IP 구조로 변환한다. 

### IP (Internet Protocol)
- 인터넷에서 장치를 식별하기 위한 주소 체계
- ex. 192.153.0.1 , 6.6.6.6
- 패킷 전달 담당, 신뢰성 보장 없음, IPv4, IPv6 존재

### IPv4 vs IPv6


### 포트
- 하나의 컴퓨터 내부에서 어떤 프로세스가 통신할지 구분하는 번호
포트	서비스
80	HTTP
443	HTTPS
22	SSH
3306	MySQL
5432	PostgreSQL
6379	Redis

### NAT (Network Address Translation)
- 사설 IP와 공인 IP를 변환하는 기술
- 사용 이유 : IPv4 주소 부족 문제 해결, 여러 장치가 하나의 공인 IP 공유 가능, 내부 네트워크 보호
- ex. 집 공유기 -> 192.168.0.2, 192.168.0.3, 192.168.0.4 -> (NAT 변환) -> 공인 IP 1개로 인터넷 통신 


## TCP, UDP 
### TCP (Transmission Control Protocol) 
- 신뢰성을 보장하는 연결 지향 프로토콜이다.

### UDP (User Datagram Protocol)
- 빠른 전송에 초점을 둔 비연결형 프로토콜이다. 
