### AWS KMS (Key Management Service)

- AWS 암호화 키를 관리
- 다중 리전 키를 제공합니다.
    - 선택된 한 리전의 기본키를 가집니다.
    - 기본키 1 (Primary) : 복제키 N (Replica)

### 다른 Region의 키를 복사하려면

<img width="437" height="184" alt="Image" src="https://github.com/user-attachments/assets/55b455af-e89f-4b2d-b2f3-5bfac0a522ca" />

1. EBS 볼륨 스냅샷 생성
2. 암호화된 스냅샷에서 스냅샷을 생성하면 생성된 스냅샷 또한 동일한 KMS 키로 암호화됩니다.
3. 다른 리전으로 스냅샷을 복사하기 위해 다른 KMS 키를 사용하여 스냅샷을 다시 암호화해야 하는데 이는 AWS에서 자동으로 처리합니다. - 동일한 KMS 키가 서로 다른 리전에 있을 수 없습니다.
4. KMS로 스냅샷을 자체 EBS 볼륨으로 복원하고, KMS 키를 다른 리전으로 복원합니다.

### **SSM Parameter Store**

<img width="434" height="583" alt="Image" src="https://github.com/user-attachments/assets/d5cd151b-125b-40f1-a0d5-556e9f1c7c97" />

- 구성(configuration) 및 암호를 위한 보안 스토리지
- 선택적으로 KMS 서비스를 사용하여 원할한 암호화가 가능합니다.
- 서버리스, 확장성, 내구성이 뛰어나며 쉬운 SDK를 제공합니다.

### **AWS Secrets Manager**

- 암호를 저장하는 최신 서비스
- N일마다 강제로 **암호를 교체**하는 기능이 있습니다.
- 교체할 암호 강제 생성 및 자동화 기능 (Lambda 함수 사용)
- **Amazon RDS**와 통합이 가능합니다. (MySQL, PostgreSQL, Aurora)
- KMS를 사용한 암호화 기능을 제공합니다.

### **AWS Certificate Manager (ACM)**

<img width="454" height="467" alt="Image" src="https://github.com/user-attachments/assets/a6a99c0f-e125-4d9a-b4bb-29bcc3fdf12b" />

- **TLS 인증서**를 AWS에서 프로비저닝, 관리, 및 배포할 수 있는 서비스
- 웹사이트의 전송 중 암호화를 위한 인증서를 제공합니다. (HTTPS)
- 퍼블릭 및 프라이빗 TSL 인증서 모두 지원합니다.
- 퍼블릭 TLS 인증서는 무료로 제공됩니다.
- TLS 인증서 자동 갱신 기능을 제공합니다.
- 여러 AWS 서비스와 통합이 가능합니다.
    - Elastic Load Balancer (CLB, ALB, NLB)
    - CloudFront 배포
    - API Gateway의 모든 API
- EC2에서는 ACM 사용이 불가합니다. (인증서 추출 불가능)

### **AWS WAF – Web Application Firewall**

<img width="449" height="172" alt="Image" src="https://github.com/user-attachments/assets/c74cab81-d879-4c97-ade1-fc4b12931711" />

- 주로 7계층 (Layer 7)에서 일어나는 웹 취약점 공격으로부터 웹 애플리케이션을 보호합니다.
- NLB에서 는 지원하지 않으며, ALB에서만 지원합니다.
- ALB에 고정 IP가 없기에, AWS Global Accelerator로 고정 IP를 할당받은 후 다음 ALB에서 AWS를 활성화해야합니다.

### **AWS Shield**

- **DDos**: Distributed Denial of Service, 분산 서비스 거부 공격 - 인프라에 동시에 엄청난 양의 요청이 세계 곳곳의 여러 컴퓨터에서 유입되는 공격, 인프라에 과부하를 일으켜 사용자들에게 서비스를 제공할 수 없게 만드는 것이 목적입니다.
- **AWS Shield는** DDos 공격으로부터 보호해줍니다.

### AWS Firewall Manager

- **AWS 조직에 있는 모든 계정의 방화벽 규칙을 관리하는 서비스**
- 보안 정책: 공통 보안 규칙의 집합
    - WAF 규칙: Application Load Balancer, API Gateways, CloudFront
    - AWS Shield Advanced: ALB, CLB, NLB, Elastic IP, CloudFront
    - EC2, Application Load Balancer 및 VPC의 ENI 리소스를 위한 보안 그룹
    - AWS Network Firewall (VPC 수준)
    - Amazon Route 53 Resolver DNS Firewall
    - 정책은 리전 수준에서 생성됩니다.

### Amazon GuardDuty

- AWS 계정을 보호하기 위한 지능형 위협 탐지 서비스

### **Amazon Inspector**

- **자동화된 보안 평가 도구**
- **EC2 인스턴스를 대상으로** 자동 보안 평가를 수행합니다.
    - **AWS Systems Manager (SSM) 에이전트**를 활용하여 실행합니다.
    - **의도하지 않은 네트워크 접근 가능성**에 대해 분석합니다.
    - **실행중인 운영체제에서 알려진 취약점**을 분석합니다.
- **Amazon ECR로 컨테이너 이미지를 푸시할 때**
    - 컨테이너 이미지에 대한 평가를 수행합니다.
- **Lambda 함수**가 배포될 때
    - 함수 코드 및 패키지 종속성에서 소프트웨어 취약성을 분석합니다.

### **AWS Macie**

<img width="439" height="103" alt="Image" src="https://github.com/user-attachments/assets/8348b53e-6487-4555-8a14-c6d2a15d73ea" />

- 민감 정보 보호 서비스
- 머신 러닝과 패턴 일치를 사용하여 AWS의 민감한 데이터를 검색하고 보호합니다.
- **개인 식별 정보(PII)**와 같은 민감한 데이터를 식별하고 경고하여 알려주어 데이터를 보호합니다.