## S3 Encrytion

- Server-Side Encryption (SSE)
    - Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
        - Amazon S3 관리형 키를 사용하는 서버 측 암호화
    - **Server-Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)**
        - **AWS KMS 키를 사용하는 서버 측 암호화**
    - Server-Side Encryption with Customer-Provided Keys (SSE-C)
        - 고객이 제공하는 키를 사용하는 암호화
- Client-Side Encryption (CSE)

#### Encryption in transit (SSL/TLS)

- 전송 중 암호화: SSL 또는 TLS
- Amazon S3 Endpoint
    - HTTP 엔드포인트: 암호화 X
    - HTTPS 엔드포인트: 전송 중 암호화
- HTTPS가 권장됩니다.

#### CORS

- Cross-Origin Resourse Sharing (CORS)
- Origin = scheme (protocol) + host (domain) + port
- 웹 브라우저에서 메인 오리진(main origin)을 방문하면서 다른 오리진으로의 요청을 허용하기 위한 기능
- 요청 프로토콜의 일부로 다른 웹사이트에 요청을 보낼 때 CORS Header를 사용해서 요청을 보내야 합니다.
- CORS는 웹 브라우저 보안 매커니즘으로 다른 출처에서 한 S3 버킷에 들어있는 검색된 이미지, 자산, 파일을 요청할 수 있게 해줍니다.

#### MFA Delete

- MFA (Multi-Factor Authentication): 사용자가 중요한 S3 작업을 수행하기 전에 장치(일반적으로 휴대폰이나 하드웨어)에서 코드를 생성하도록 요구하는 기능
    - MFA 필요한 상황: 객체 버전 삭제, 버킷에서 버전 관리를 중단할 경우
    - MFA Delete를 사용하려면 버킷에서 버전 관리를 활성화해야 합니다.
- 해당 기능의 권한은 버킷 소유자만 가질 수 있습니다.

#### **S3 Access Logs**

- 감사 목적으로 S3 버킷의 모든 액세스를 로깅할 수 있습니다.
- 승인 또는 거부 여부와 상관없이 모든 계정에서 S3에 대한 요청이 로그로 남습니다.
- 이 데이터는 데이터 분석 도구로 분석할 수 있습니다.
- 로깅 대상 버킷은 동일한 AWS 리전에 있어야 합니다.

#### **Pre-Signed URLs**

- **S3 콘솔, AWS CLI 또는 SDK**를 사용하여 생성할 수 있는 URL
- 미리 서명된 URL을 받은 사용자는 GET / PUT에 대한 권한을 URL을 생성한 사용자로부터 상속받습니다.

#### **S3 Access Points**

<img width="446" height="128" alt="Image" src="https://github.com/user-attachments/assets/8663384a-8d80-4c55-9c85-2ac963df3458" />

- 각 액세스 포인트마다 고유의 DNS 이름과 정책을 가지고 있다. 이를 통해 사용자나 그룹의 액세스를 제한할 수 있습니다.
    - 특정한 IAM 사용자 / 그룹
    - 액세스 포인트마다 하나의 정책만을 가지기 때문에 복잡하고 고유한 버킷 정책보다 훨씬 다루기 쉽습니다.

#### S3 Object Lambda

<img width="443" height="335" alt="Image" src="https://github.com/user-attachments/assets/cdbac8a4-7705-4c95-8736-73a72f7724d1" />

- AWS Lambda 함수를 사용하여 호출한 애플리케이션이 객체를 회수하기 전에 객체를 수정합니다.
- 한 개의 S3 버킷만 필요하며, S3 버킷 상위 수준에 S3 액세스 포인트를 생성하고 Lambda 함수과 연결합니다.
- 사용 사례:
    - 분석이나 비프로덕션 환경에서 개인 식별 정보 삭제
    - XML을 JSON으로 변환하는 등 데이터 형식을 변환
