## **IAM**: Identify and Access Management

- AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스
    - 사용자 인증 (Authentication)
    - 권한 관리(Authorization)

### Users & Groups

- 사용자를 생성하고 그룹에 배치하기 때문에 글로벌 서비스에 해당됩니다.
- **Root account**: 기본적으로 생성되며, 생성 후에는 루트 계정을 더이상 사용해서도, 공유해서도 안됩니다. 대신 사용자를 생성해야 합니다.
- **Users**: 하나의 사용자는 조직 내의 한 사람에 해당됩니다. 필요하다면 사용자들을 그룹으로 묶을 수도 있습니다.
- **Groups**: 그룹에는 사용자만 배치할 수 있으며 다른 그룹은 포함할 수 없습니다.
- 사용자와 그룹을 생성하는 이유는 이들이 aws 계정을 사용할 수 있게 하기 위함입니다.

### **Permissions**

- 사용자 또는 그룹에게 policy, IAM policy라고 불리는 Json 문서를 지정할 수 있습니다.
- 해당 정책을 사용하여 사용자들의 권한을 정의할 수 있으며 least privilege principle(최소 권한 원칙)을 지킬 수 있습니다.
    - 필요한 권한만 부여

```jsx
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*",
        }, 
      	{
            "Effect": "Allow",
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*",
        },
      	{
            "Effect": "Allow",
            "Action": [
              "cloudwatch:ListMetrics",
              "cloudwatch:GetMetricStatistics",
              "cloudwatch:Describe*"
            ],
            "Resource": "*",
        },
    ]
}
```

### Policies Structure

- version: 정책 언어 버전
- id: 정책 식별 id
- Statement: 한 개 이상의 statements
    - Sid: Statement id, Statement의 식별자
    - Effect: Statement가 특정 API 접근 허용유무에 대한 내용
    - Principal: 특정 정책이 적용될 사용자, 계정, 역할
    - Action: effect에 기반하여 허용 및 거부되는 API 호출목록
    - Resource: 적용될 action의 리소스 목록
    - Condition: Statement가 언제 적용될지 결정합니다.

### Password Policy

- Strong password = higher security for your account
- AWS에는 다양한 옵션을 활용하여 비밀번호 정책 생성이 가능합니다.
    - 최소길이 설정, 대소문자-숫자-특수문자, IAM 사용자들의 비밀번호 변경을 허용 및 금지하는 정책, 일정시간 이후 비밀번호 변경, 비밀번호 재사용 금지 정책 등..

### Multi Factor Authentication - MFA

- 비밀번호 이외에 사용할 수 있는 수단 (password you know + security device you own)
    - if a password is stolen or hacked, the account is not compromised!
- MFA device option
    1. Virtual MFA device: 하나의 장치에서도 토큰을 여러개 지원합니다. 루트 계정, IAM 사용자 등 원하는개수 만큼의 계정 및 사용자 등록이 가능합니다. (Google Authenticator, Authy)
    2. Universal 2nd Factor (U2F) Security Key:  하나의 보안 키에서 여러 루트 계정과 IAM 사용자를 지원합니다. (Yubikey by Yubico(3rd party))
    3. Hardware Key Fob MFA Device (Provided by Gemalto (3rd party))
    4. Hardware Key Fob MFA Device for AWS GovCloud (US) (Provided by SurePassID (3rd party))

### AWS CLI

- 사용자들이 AWS에 접근하는 방법
    1. AWS Management Console  (protected by password + MFA)
    2. AWS Command Line interface (protected by access keys) (AWS CLI)
        - Command-line shell에서 명령어를 사용하여 AWS 서비스들과 상호작용할 수 있도록 도와주는 도 구
        - AWS의 공용 API로 직접 액세스가 가능합니다.
        - 리소스를 관리하는 스크립트를 개발하여 일부 작업을 자동화할 수 있습니다.
    3. AWS Software Developer Kit (SDK) (protected by access keys) (AWS SDK)
        - 특정 언어로 된 라이브러리의 집합.
        - AWS SDK를 활용하면 AWS의 서비스나 API에 프로그래밍을 위한 액세스를 가능하게 합니다.
        - SDKs (JavaScript, Python, PHP, NET, Ruby, Java, Go, Node.js, C++), Mobile, IoT Device ..
- Access Keys는  AWS 콘솔을 통해 생성합니다.
    - Access Key ID = username
    - Secret Access Key = password

### IAM Role

- 몇몇 AWS는 본인 계정에서 실행해야 하는데, 사용자와 마찬가지로 권한이 필요합니다. (AWS 서비스에 권한 부여)
- EC2 Instance Roles, Lambda Function Roles ..

### IAM Security Tools

- IAM Credentials Report (account-level)
    - 계정에 있는 사용자와 다양한 자격 증명의 상태를 포함합니다.
- IAM Access Advisor (user-level)
    - 사용자에게 부여된 서비스의 권한과 해당 서비스에 마지막으로 액세스한 시간을 볼 수 있습니다.
    - 해당 도구를 사용하여 어떤 권한이 사용되지 않는지 볼 수 있고, 이를 통해 사용자의 권한을 줄여 최소 권한의 원칙을 지킬 수 있습니다.

### IAM 정리

- **Users**: 실제 물리적 사용자에 매핑되며, AWS 콘솔 로그인을 위한 비밀번호를 가집니다.
- **Groups**: 사용자만 포함하는 집합 (User들을 묶어서 관리)
- **Policies**: 사용자 또는 그룹의 권한을 정의하는 JSON 문서
- **Roles**: EC2 인스턴스나 AWS 서비스에 부여하는 권한
- **Security**: MFA(다중 인증) + 비밀번호 정책 적용
- **AWS CLI**: 명령어 기반으로 AWS 서비스를 관리
- **AWS SDK**: 프로그래밍 언어를 통해 AWS 서비스를 관리
- **Access Keys**: CLI 또는 SDK를 통해 AWS에 접근할 때 사용하는 키
- **IAM Credential Report**: 계정 자격 정보 리포트
- **IAM Access Advisor**: 사용자/역할의 권한 사용 현황 분석