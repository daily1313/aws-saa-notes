### **AWS Organizations**

<img width="451" height="247" alt="Image" src="https://github.com/user-attachments/assets/a29537c4-3065-40da-ae96-60c00a48ddc0" />

- 글로벌 서비스로서 여러 개의 aws 계정을 관리할 수 있습니다.
- 조직을 생성하면, 조직의 메인 계정이 관리계정이며, 다른 계정은 멤버계정입니다.
- AWS 계정 생성을 자동화하기 위한 API를 제공합니다.
- Advantages
    - 다수의 계정을 가지므로 다수의 VPC를 가진 단일 계정에 비해 보안이 뛰어납니다. (계정이 VPC보다 독립적이기 때문입니다.)
    - 청구 목적을 위한 태그 기준을 적용할 수 있습니다.
    - 한 번에 모든 계정에 대해 CloudTrail 활성화 및 로그를 중앙 S3 계정으로 전송합니다.
    - CloudWatch Logs를 중앙 로깅 계정으로 전송합니다.
- **Security: 서비스 제어 정책 (Service Control Policies, SCP)**
    - OU 또는 계정에 적용되는 IAM 정책으로 사용자 및 역할 제한
    - SCP는 모든 계정에 적용되나 관리 계정에는 적용되지 않습니다.
    - 명시적 허용이 필요합니다.(IAM과 마찬가지로 기본적으로는 아무것도 허용 X)

### **IAM Conditions**

- aws:SourceIP: ip 주소 범위에 포함되지 않는 ip를 거부합니다.
- aws:RequestedRegion: 특정 region의 요청을 제한합니다.
- ec2:ResourceTag, aws:PrincipalTag
- aws:MultiFactorAuthPresent: MFA 인증이 되어야 있어야 처리가 가능합니다.
- s3:GetObject, s3:PutObject, s3:DeleteObject

### **Amazon Cognito**

- 웹 또는 모바일 애플리케이션과 상호작용하는 사용자에게 식별 정보를 제공합니다.
- Cognito 사용자 풀
    - 모바일 사용자를 위한 로그인 기능을 제공합니다.
    - API Gateway, ALB와의 통합 기능을 제공합니다.
- **Cognito 자격 증명 풀 (Federated Identity)**
    - 사용자에게 AWS 자격 증명을 제공하여 직접 AWS 리소스에 액세스할 수 있도록 합니다.
    - Cognito 사용자 풀과 통합하여 신원 제공자로 사용합니다.

### **AWS IAM Identity Center**

- AWS Single Sign-On 서비스의 후속 제품
- 단일 로그인 기능
    - **AWS Organization 내의 모든 AWS 계정**
    - 비즈니스 클라우드 애플리케이션 (예: Salesforce, Box, Microsoft 365 등)
    - SAML 2.0을 지원하는 애플리케이션
    - EC2 Windows 인스턴스
- ID 공급자
    - IAM Identity Center에 내장된 ID 저장소
    - 서드파티 ID 공급자: Active Directory (AD), OneLogin, Okta 등
- 제공 기능
    - 다중 계정 권한 (Multi-Account Permissions)
    - 애플리케이션 할당 (Application Assignments)
    - 속성 기반 액세스 제어 (Attribute-Based Access Control, ABAC)

### AWS Directory Service

<img width="440" height="431" alt="Image" src="https://github.com/user-attachments/assets/84ad7c15-8194-4463-a09b-f8337a2453db" />

- AD 도메인 서비스를 사용하는 모든 Windows 서버에 사용되는 소프트웨어
- **객체**의 데이터베이스: 사용자 계정, 컴퓨터, 프린터, 파일 공유, 보안 그룹이 객체가될 수 있습니다.
- 중앙 집중식 보안 관리: 계정 생성, 권한 할당 등의 작업
- 종류
    - AWS Managed AD: AWS 클라우드에서 사용자를 관리하고 MFA를 사용할 경우
    - AD Connector: 온프레미스에서 사용자를 프록시할 경우
    - Simple AD: 온프레미스 X

### **AWS Control Tower**

- 모범 사례를 기반으로 안전하고 규정을 준수하는 다중 계정 AWS 환경을 손쉽게 설정하고 관리할 수 있습니다.
- AWS Control Tower 서비스는 AWS Organizations를 사용하여 계정을 생성합니다.
- 장점:
    - 몇 번의 클릭만으로 환경 설정 자동화
    - 가드레일을 사용하여 지속적인 정책 관리 자동화
    - 정책 위반 감지 및 조치
    - 대화형 대시보드를 통한 규정 준수 모니터링

### Guardrails

<img width="442" height="154" alt="Image" src="https://github.com/user-attachments/assets/20772ed7-4ced-4c69-bb4b-7d4699b7e799" />

- AWS Control Tower의 가드레일은 Control Tower 환경 내의 모든 AWS 계정에 대한 지속적인 거버넌스를 제공합니다.
- **예방적 가드레일 (Preventive Guardrail)**
    - SCP(Security Control Policies)를 사용하여 구현합니다.
    - ex. 모든 계정에서 리전 제한을 설정하여 특정 리전의 사용을 제한
- **탐지적 가드레일 (Detective Guardrail)**
    - AWS Config를 사용하여 구현합니다.
    - ex. 태그가 지정되지 않은 리소스 식별