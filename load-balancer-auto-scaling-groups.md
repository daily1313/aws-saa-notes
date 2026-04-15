## Scalability (확장성)

- 애플리케이션 / 시스템이 조정을 통해 더 많은 양을 처리할 수 있는 능력
- vertical Scalability(수직 확장): 인스턴스의 크기 확장
    - ex) t2.micro → t2.large
    - rds, elasticache
- horizontal Scalability(수평 확장): 인스턴스 / 시스템 수 증가
    - 분산 시스템
    - ec2

## High Availability

- 수평 확장과 함께 사용되는 개념
- 애플리케이션 또는 시스템을 적어도 둘 이상의 AWS의 AZ나 데이터 센터에서 가동 중인 것을 의미합니다.
- 고가용성의 목표: 데이터 손실에서 살아남는 것. 따라서 센터 하나가 멈춰도 계속 작동이 가능하도록 합니다.
    - 고가용성은 수동적일 수 있습니다. (ex. RDS Multi AZ)
    - 고가용성은 능동적일 수 있습니다. (ex. 수평 확장)

## Load balancing

<img width="541" height="224" alt="Image" src="https://github.com/user-attachments/assets/fb8a5657-1d24-4dfb-b65c-8cb87bff1321" />

- 부하를 다운스트림 인스턴스로 분산하기 위해 사용합니다.
- 애플리케이션의 단일 액세스 지점 노출
- Health Check, HTTPS
- 트래픽 분리, 영역간 고가용성, 쿠키로 정적 분배를 적용합니다.

### ELB (Elastic Load Balancer)

- **관리형 로드 밸런서**
    - AWS가 관리하며, 어떠한 경우에도 로드 밸런서가 작동할 것을 보장합니다.
    - AWS는 업그레이드, 유지 관리 및 고가용성을 책임집니다.
    - AWS는 몇 가지 구성 놉(configuration knobs)도 제공합니다.
- 다수의 AWS 서비스들과 통합되어 있습니다.
    - EC2, EC2 Auto Scaling Groups, Amazon ECS
    - AWS Certificate Manager (ACM), CloudWatch
    - Route53, AWS WAF, AWS Global Accelerato
- 애플리케이션에 사용 가능한 정적 DNS 이름을 제공합니다.

### Health Checks

- Load Balancer에서 Health Checks는 매우 중요합니다.
- LB가 전달하는 트래픽을 처리할 수 있는 인스턴스의 가용성을 파악할 수 있습니다.
- url: /health

### Load Balancer Types

**1. Classic Load Balancer** (v1 - old generation)– 2009 – CLB (AWS 지원 X)

- HTTP, HTTPS, TCP, SSL (secureTCP)

**2. Application Load Balancer** (v2 - new generation) – 2016 – ALB

- 7 layer (default: 활성화)
- redirection을 지원합니다.
- 동일 ec2 상의 여러 애플리케이션에 대한 로드 밸런싱을 지원합니다.
- url 경로, 호스트 이름, 쿼리스트링, 헤더 기반으로 라우팅합니다.
- MSA, 컨테이너 기반 애플리케이션에 적합합니다.
- HTTP(/2), HTTPS, WebSocket

**3. Network Load Balancer** (v2 - new generation) – 2017 – NLB

- 4  layer (default: 비활성화)
    - TCP, UDP Traffic을 instance로 전달합니다.
    - 초당 수백만건의 요청 처리가 가능하며 지연 시간이 짧습니다.
- 가용 영역 당 하나의 고정 IP를 가집니다.
- 탄력적 IP를 적용할 수 있습니다.
- TCP, TLS (secureTCP), UDP

**4.Gateway Load Balancer** – 2020 – GWLB

- 배포 및 확장, AWS 3rd party 네트워크 가상 어플라이언스 플릿 관리에 사용됩니다. (default: 비활성화)
    - firewall..
- Layer 3 (IP Packet)
- 6081 Port, GENEVE Protocol을 사용합니다.
- Operates at layer 3 (Network layer) – IP Protocol

### Sticky Sessions (고정 세션)

- 로드 밸런서에 2가지 요청을 하는 클라이언트가 요청에 응답하기 위해 백엔드에 동일한 인스턴스를 갖는 것.
    - 고정 세션에 사용되는 쿠키는 클라이언트에서 로드 밸런서로 요청의 일부로서 전송되는 것이며 만료 기간을 포함하고 있습니다.
    - 사용 사례: 사용자의 로그인과 같은 중요한 정보를 취하는 세션 데이터를 잃지 않기 위해 사용합니다.
- Cookie Names
    - **Application-based Cookies** 애플리케이션 기반 쿠키 (AWSALBAPP)
    - **Duration-based Cookies** 기간 기반 쿠키

  (AWSALB, AWSELB)

### Gateway Load Balancer

- 배포 및 확장, AWS의 3rd party 네트워크 가상 어플라이언스의 플릿 관리에 사용됩니다.
- ex) 방화벽, 침입 탐지 및 방지 시스템
- layer 3 계층에서 실행됩니다. (protocol: geneve, port: 6081)

### **SSL/TLS - Basics**

- SSL 인증서: 클라이언트와 로드 밸런서 간의 트래픽을 암호화하여 전송 중에 보호합니다. (in-flight encryption)
- **SSL**: 웹 브라우저와 서버 간의 데이터를 암호화하여 통신을 보호하는 표준 보안 기술 (Secure Sockets Layer)
- **TLS**: 새로운 버전의 SSL. 전송 계층 보안 (Transport Layer Security)
- 인증 기관(CA)에 발급하며, SSL 읹으서는 만료 일자가 있기에 주기적으로 갱신해줘야 합니다.

### Load Balancer - SSL Certificates

- 로드 밸런서는 X.509 인증서 (SSL/TLS 서버 인증서)를 사용합니다.
- AWS에는 이 인증서들을 관리할 수 있는 ACM (AWS Certificate Manager)가 있습니다.
- ACM에는 사용자 지정 인증서를 업로드할 수 있습니다.
- HTTPS listener
    - 기본 인증서를 지정해야 합니다.
    - 다중 도메인을 지원하기 위해 선택적으로 인증서 목록을 추가할 수 있습니다
    - **클라이언트는 SNI (Server Name Indication)를 사용하여 도달하는 호스트 이름을 지정할 수 있습니다.**
    - SNI는 여러 개의 SSL 인증서를 웹 서버에 로드해 하나의 웹서버가 여러개의 웹사이트를 지원할 수 있도록 합니다.

### Load Balancer - Connection Draining

- 인스턴스가 등록 취소, 비정상적인 상태에 있을 때 인스턴스에 어느정도 시간을 둬서 in-flight 요청, 즉 활성 요청을 할 수 있도록 하는 기능
- 등록 취소 중인 EC2 인스턴스에 새 요청을 보내지 않도록 해야합니다.

### Auto Scaling Group

- 오토 스케일링(Auto Scaling): 클라우드의 유연성을 돋보이게 하는 핵심기술로 CPU, 메모리, 디스크, 네트워크 트래픽과 같은 시스템 자원들의 메트릭(Metric) 값을 모니터링하여 서버 사이즈를 자동으로 조절 하는 서비스
- 웹사이트나 애플리케이션의 부하는 별할 수 있기에 AWS에서 서버를 빠르게 생성하고 종료할 수 있습니다.
- ASG
    - 부하 증가에 따른 scale-out (인스턴스 추가)
    - 부하 감소에 따른 scale-in  (인스턴스 감소)
    - 취소 및 최대 실행 중인 EC2 인스턴스 수를 보장합니다.
    - 새로운 인스턴스를 로드 밸런서에 자동 등록합니다.
    - 한 인스턴스가 비정상이면 종료하고 이를 대체할 새 ec2 인스턴스를 생성합니다
    - ASG를 CloudWatch 경보에 기반하여 스케일 인 및 스케일 아웃할 수 있습니다. (cpu, metric 모니터링)
        - 경보(지표)를 기반으로 scale-in/out 정책을 만들 수 있습니다.