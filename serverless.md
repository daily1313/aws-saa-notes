## **Serverless**

- 서버리스는 서버가 없다는 것을 의미하지 않고, 서버가 보이지 않거나, 관리하지 않고, 프로비저닝하지 않는 것을 의미합니다.
- ex) AWS Lambda, DynamoDB, AWS Cognito, AWS API Gateway • Amazon S3, AWS SNS & SQS, AWS Kinesis Data Firehose • Aurora Serverless, Step Functions, Fargate

### **AWS Lambda**

**Amazon EC2**

- 클라우드 가상 서버 - 프로비저닝이 필요합니다.
- RAM과 CPU 크기가 제한됩니다.
- 지속적으로 실행됩니다.
- 스케일링은 서버를 추가하거나 제거하는 작업을 해야 합니다.

**Amazon Lambda**

- **서버 관리 없이 코드를 실행하는 서버리스 컴퓨팅 서비스**
- 가상 **함수** - 관리할 서버가 없습니다.
- 제한 시간이 있습니다 - **짧은 실행 시간**
- **온디맨드**로 실행됩니다. (호출 시 실행)
- 스케일링 **자동화**

#### **Benefits of AWS Lambda**

- AWS의 전체 서비스와 통합이 가능합니다.
- 다양한 프로그래밍 언어 사용이 가능합니다.
    - ex) node.js, python, java, c#
- AWS CloudWatch를 통한 쉬운 모니터링 기능을 제공합니다.
- 임의의 Docker 이미지를 실행하기 위해서는 ECS 또는 Fargate를 사용하는 것이 권장됩니다.

### AWS Lambda Integrations Main ones

- API Gateway: REST API 생성, 람다 함수 호출
- Kinesis: Lambda를 이용해 바로 데이터를 변환합니다.
- DynamoDB: 데이터베이스에 트리거가 생기면 람다 함수가 작동합니다.
- S3: 언제든 람다 함수를 작동시킬 수 있습니다.
- CloudFront: Lambda@Edge
- CloudWatch Evnets & EventBridge: AWS의 인프라에 어떤 일이 생기고 그 상황에 대응하고자 할 때 상황에 따라 자동화를 실행하기 위해 람다 함수를 사용합니다.
- CloudWatch Logs: 어디든 해당 로그를 스트리밍합니다.
- SNS: 알림과 SNS 주제에 대처합니다.
- SQS: SQS 대기열 메시지 처리가 가능합니다.
- Cognito: 사용자가 데이터베이스에 로그인할 때마다 응답합니다.

#### **Edge Function: Lambda@Edge & CloudFront Functions**

- 보통은 함수와 애플리케이션을 특정 리전에서 배포하지만 CloudFront를 사용할 때에는 엣지 로케이션을 통해 콘텐츠를 배포합니다.
- 엣지 함수
    - CloudFront 배포에 연결하고 작성한 코드
    - 지연시간을 최소화하기 위해 사용자에게 가까운 위치에서 실행됩니다.
- 서버를 관리할 필요없이 전역으로 배포됩니다.

#### **CloudFront Functions vs. Lambda@Edge**

<img width="455" height="298" alt="Image" src="https://github.com/user-attachments/assets/6f78f20f-d00d-470a-9295-96aab68c5083" />

#### CloudFront Functions vs. Lambda@Edge - Use Cases

**CloudFront Functions**

- 캐시 키 정규화
    - 요청 속성 (헤더, 쿠키, 쿼리 문자열, URL)을 변환하여 최적의 캐시 키 생성
- 헤더 조작
    - 요청 또는 응답에서 HTTP 헤더 삽입/수정/삭제
- URL 재작성 또는 리디렉션
- 요청 인증 및 권한 부여
    - 사용자가 생성한 토큰 (JWT 등) 생성 및 유효성 검사를 통한 요청 허용/거부

**Lambda@Edge**

- 더 긴 실행 시간 (수 밀리초)
- 조정 가능한 CPU 또는 메모리
- 코드가 3rd party 라이브러리에 의존하는 경우 (예: 다른 AWS 서비스에 액세스하기 위한 AWS SDK)
- 데이터 처리를 위한 외부 서비스에 대한 네트워크 액세스를 통해 대규모 데이터 통합 수행
- 파일 시스템 액세스 또는 HTTP 요청의 본문에 대한 액세스

**Lambda in VPC**

<img width="438" height="380" alt="Image" src="https://github.com/user-attachments/assets/9b88e31e-c058-45ab-bc0f-324c4e5caa6b" />

1. 기본적으로 Lambda 함수는 사용자의 VPC가 아닌 AWS 소유의 VPC에서 실행됩니다.
2. 따라서 VPC 내에서 리소스(RDS, ElastiCache, 내부 ELB 등)에 액세스할 수 없습니다.
3. 이를 해결하려면 사용자의 VPC에서 Lambda 함수를 시작하면 됩니다.

**사용자의 VPC에서 Lambda 함수를 수행하려면**

<img width="432" height="562" alt="Image" src="https://github.com/user-attachments/assets/43a45771-8b0c-4d22-8dac-f821776d7d71" />

- Private subnet에 ENI 생성, 보안그룹을 추가해야 합니다.

#### **Lambda with RDS Proxy**

- DB Connection Pool
- 퍼블릭 액세스가 불가능하기에 Lambda 함수는 사용자의 VPC에 배포되어야 합니다.

#### **Amazon DynamoDB**

- 완전 관리형 데이터베이스
- 다중 AZ간의 데이터 복제를 통해 고가용성을 제공합니다
- NoSQL - 관계형 데이터베이스가 아닙니다. 트랜잭션을 지원합니다.
- 대규모 워크로드로 확장 가능한 분산 데이터베이스
- 성능이 빠르고 일관성을 유지합니다. (한 자릿수 밀리초의 성능)
- 보안, 인가 및 관리를 위해 IAM과 통합되어 있습니다.
- 저렴한 비용. 오토 스케일링 기능을 제공합니다.
- 유지 관리나 패치 작업이 필요 없으며 항상 사용 가능합니다.
- Standard 및 Infrequent Access (IA) 테이블 클래스를 지원합니다.
- **스키마를 빠르게 전개해야할 때 Aurora나 RDS보다는 DynamoDB가 더 나은 선택입니다.**

#### Read/Write Capacity Modes

- 테이블의 용량 (읽기/쓰기 처리량) 관리
- Provisioned Mode
    - 초당 읽기/쓰기 수를 예측하여 미리 지정합니다.
    - 용량을 사전에 계획해야 하며 프로비저닝된 읽기 용량 단위 (rcu), 쓰기 용량 단위 (wcu)에 대해서 비용을 지불합니다.
    - rcu, wcu에 대한 오토스케일링 기능을 제공합니다.
- On-Demand Mode
    - 읽기/쓰기 작업량에 따라 자동으로 확장/축소 됩니다.
    - 용량 계획이 필요하지 않으며, 예측할 수 없는 작업량이나 급격한 증가가 예상되는 경우에 적합합니다.

#### DynamoDB Accelerator

<img width="424" height="208" alt="Image" src="https://github.com/user-attachments/assets/239bc72c-204f-4972-9764-3d2c551da327" />

- DynamoDB를 위한 완전 관리형 무결졀 인메모리 캐시
- DynamoDB 테이블에 읽기 작업이 많을 때 DAX 클러스터를 생성하고 데이터를 캐싱하여 읽기 혼잡을 해결합니다.
- 캐시된 데이터에 대한 마이크로초 수준의 지연 시간이 걸립니다.
- 애플리케이션 로직 수정이 필요하지 않습니다. (기존 DynamoDB API와 호환)
- 캐시의 기본 TTL 기본값은 5분

#### Stream Processing

스트림 처리는 데이터의 변경 사항을 실시간으로 감지하고 이를 처리하는 기술입니다. DynamoDB의 스트림은 테이블에서 발생하는 모든 항목의 생성, 업데이트, 삭제 작업에 대한 변경 이벤트를 기록하는 일련의 이벤트 스트림입니다.

ex) 실시간으로 변경 사항에 대응하기 (사용자에게 환영 이메일 보내기), 실시간 사용 분석, 파생 테이블 삽입, 리전 간 복제 구현, DynamoDB 테이블 변경 시 AWS Lambda 호출

#### **DynamoDB Stream vs Kinesis Data Stream**

**DynamoDB Stream**

- 보존 기간: 24시간
- 소비자 수 제한
- Lambda 트리거 또는 DynamoDB Stream adapter 사용 가능

**Kinesis Data Stream**

- 보존 기간: 1년
- 더 많은 소비자의 수
- AWS Lambda, Kinesis Data Analytics, Kineis Data Firehose, AWS Glue Streaming ETL 등 사용 가능

#### **DynamoDB Global Table**

<img width="446" height="195" alt="Image" src="https://github.com/user-attachments/assets/32362e16-1121-4647-86eb-a45f5f16e0c4" />

- 여러 리전 간에 짧은 지연 시간으로 액세스 할 수 있도록 하는 기능입니다.
- active-active 복제
- 애플리케이션은 어떤 리전에서든 해당 테이블을 **읽고 쓸** 수 있습니다.
- Global Tables를 설정하기 위해서는 먼저 테이블에 DynamoDB Streams를 활성화해야 합니다.

### Time To Live (TTL)

- 만료 타임스탬프가 지난 항목을 자동으로 삭제하는 기능

#### Export S3

<img width="441" height="121" alt="Image" src="https://github.com/user-attachments/assets/badaf778-7e67-4595-84c2-e67d82748b61" />

- S3로 내보려면, PITR 기능을 활성화해야 하며, Athena를 통해 Query 할 수 있습니다.

#### Import S3

<img width="449" height="160" alt="Image" src="https://github.com/user-attachments/assets/057e52be-f22b-4af0-bed4-b107e5f64557" />

- S3에서 CSV, DynamoDB JSON 또는 ION 형식의 데이터를 가져와 DynamoDB에 새로운 테이블로 가져옵니다.
- 쓰기 용량을 소모하지 않으며, import중 발생하는 오류는 cloudwatch logs에 기록됩니다.

#### AWS API Gateway

<img width="430" height="80" alt="Image" src="https://github.com/user-attachments/assets/9cb41394-3147-4e69-ad57-a7e09fbeacc8" />

- 클라이언트와 Lambda 함수 사이에 애플리케이션 로드 밸런서를 배치하면 Lambda 함수가 HTTP 엔드포인트에 노출됩니다.
- 이때 API Gateway를 사용하는 방법도 있는데, AWS 서버리스 서비스로 REST API를 생성할 수 있으므로 클라이언트가 퍼블릭 액세스할 수 있게 됩니다.
- API Gateway에 클라이언트가 직접 소통하며 다양한 작업을 할 수 있고 Lambda 함수에 요청을 프록시합니다.


#### AWS API Gateway 제공 기능

- AWS Lambda + API Gateway: 완전 서버리스 애플리케이션이 구축되므로 인프라 관리가 필요없습니다.
- WebSocket 프로토콜 지원: 양방향 통신이 가능합니다.
- API 버전 관리 (v1, v2 ...)
- 환경 관리 (dev, test, prod ...)
- 보안 관리
- API 키 생성, 요청 스트롤링

#### **AWS Service Integration Kinesis Data Streams example**

<img width="428" height="73" alt="Image" src="https://github.com/user-attachments/assets/d0d0a0b4-6984-4574-8eef-c2eb10c5d455" />

1. Kinesis Data Stream에 사용자가 데이터를 전송할 수는 있지만 AWS 자격 증명은 가질 수 없도록 보안 설정을 할 수 있습니다.
2. Kinesis Data Straem과 클라이언트 사이에 API Gateway를 둡니다.
3. 클라이언트가 API Gateway로 HTTP 요청을 보내면 Kinesis Data Stream에 전송하는 메시지로 구성해 전송 - 따로 서버를 관리할 필요가 없습니다.
4. Kinesis Data Stream에서 Kinesis Data Firehose로 레코드를 전송합니다.
5. 최종적으로 JSON 형식으로 Amazon S3 버킷에 저장합니다.

#### **AWS Step Functions**

- Lambda 함수를 조율하기 위한 서버리스 워크플로우를 시각적으로 구축할 수 있는 서비스
- **기능**: 시퀀스, 병렬 처리, 조건부 실행, 타임아웃, 오류 처리 등
- Lambda 함수 뿐만 아니라 EC2, ECS, 온프레미스 서버, API Gateway, SQS 큐 등 다양한 AWS 서비스와 통합이 가능합니다.

#### **MicroService Architecture**

<img width="454" height="201" alt="Image" src="https://github.com/user-attachments/assets/e8dc5612-73c7-4e62-b035-7a8d48fc0f62" />

- 서비스의 독립 확장, 코드 리포지토리를 갖출 수 있습니다.

#### **Software updates offloading**

EC2에서 작동하는 애플리케이션이 있고 종종 소프트웨어 업데이트를 배포합니다. 소프트웨어가 새로 업데이트되면 요청을 많이 받게되고, 콘텐츠는 네트워킹을 통해 다수에게 배포되는데 이는 비용적인 소모가 큽니다.

소프트웨어 업데이트 비용과 CPU 사용량을 최적화하고 EC2에서 실행 중인 응용 프로그램을 변경하지 않고 소프트웨어 업데이트를 분산하는 방법은 무엇입니까?

#### 애플리케이션을 전세계로 확장하고 CPU 사용량을 줄이며 비용을 절감하는 방법: CloudFront

<img width="447" height="283" alt="Image" src="https://github.com/user-attachments/assets/5221492c-3d41-4784-a7b8-7821d190004a" />

- CloudFront를 두면 CPU 소모량을 줄이고 애플리케이션을 확장할 수 있습니다.
- 엣지에서 소프트웨어 파일은 캐시에 저장된다. 업데이트 파일은 변하지 않기 때문입니다. (동적이 아닌 정적이기 때문에 절대 바뀌지 X)
- EC2 인스턴스는 서버리스가 아니지만 CloudFront는 서버리스이기 때문에 확장할 수 있습다.
- ASG가 많이 확장하지 않아 EC2, 네트워크, EFS 비용을 크게 절감할 수 있습니다.