## Database Types

- **RDBMS (= SQL / OLTP)**: RDS, Aurora - Join에 유용합니다.
- **NoSQL database – Join X, no SQL**: : DynamoDB (~JSON), ElastiCache (키/값 쌍), Neptune (그래프), DocumentDB (MongoDB용), Keyspaces (Apache Cassandra용)
- **객체 스토어**: S3 (대용량 객체용) / Glacier (백업/아카이브용)
- **데이터 웨어하우스** (= SQL 분석 / BI): Redshift (OLAP), Athena, EMR
- **검색**: OpenSearch (JSON) - 자유로운 텍스트 입력 및 비정형 데이터 검색
- **그래프**: Amazon Neptune - 데이터 세트 간의 관계 표시
- **원장(금융 트랜잭션 기록)**: Amazon Quantum Ledger Database(QLDB)
- **시계열**: Amazon Timestream

### RDS

- 관리형 PostgreSQL / MySQL / Oracle / SQL Server / MariaDB / 사용자 지정 RDS
- RDS 인스턴스 크기 및 EBS 볼륨 유형 및 크기를 프로비저닝해야 합니다.
- 스토리지 오토스케일링 기능을 제공합니다.
- 읽기 용량 확장을 위해 읽기 전용 복제본 및 다중 AZ를 지원합니다.
- IAM - RDS 데이터베이스 보안 / 보안 그룹 - 네트워크 보안 / KMS - 저장 데이터 암호화 / SSL & TLS - 전송 데이터 암호화
- 자동 백업 옵션 (최대 35일까지), 지정 시간 복구 기능
- 장기 보존 백업을 위한 수동 DB 스냅샷
- 유지 관리 기능을 예약할 수 있습니다. (다운타임 발생 - 프로비저닝하거나 AWS가 데이터베이스 엔진을 주기적으로 업데이트하고 기본 EC2 인스턴스에 보안 패치를 실행할 때 발생)
- RDS 프록시를 강제하여 RDS에 IAM 인증 추가, Secret Manager와 통합하여 DB 자격 증명을 제공합니다.
- Oracle 및 SQL Server의 기본 인스턴스 액세스 및 사용자 지정을 위한 RDS 사용자 지정 옵션을 제공합니다.
- ex) 관계형 데이터 세트 저장 (RDBMS / OLTP), SQL 쿼리 및 트랜잭션 수행

### Aurora

- PostgreSQL / MySQL과 호환되는 API, 컴퓨팅과 스토리지가 분리되어 있습니다.
- 스토리지: 데이터는 3개의 가용 영역에 걸쳐 6개의 복제본에 저장됩니다. (고가용성, 자가 복구 처리, 오토 스케일링 기능)
- 컴퓨팅: 클러스터화된 실제 데이터베이스 인스턴스를 여러 가용 영역에 걸쳐 저장합니다. 읽기 전용 복제본의 오토 스케일링
- 클러스터: 읽기와 쓰기를 위한 DB 인스턴스에 대한 사용자 지정 엔드 포인트
- RDS와 동일한 보안 / 모니터링 / 유지 관리 기능
- Aurora의 백업 및 복구 옵션
    - **Aurora Serverless**: 예측할 수 없는 / 간헐적인 워크로드에 적합, 용량 계획을 하지 않아도 됩니다.
    - **Aurora Multi-Master**: 쓰기 고가용성을 위해 쓰기 장애 조치가 지속해서 필요할 때 사용합니다. - DB 인스턴스 여러 개가 스토리지에 쓰기를 할 수 있도록 설정이 가능합니다.
    - **Aurora Global**: 데이터가 복제된 리전마다 최대 16개의 데이터베이스 읽기 전용 인스턴스를 제공합니다. 리전 간 스토리지 복제에 걸리는 시간 1초 미만이 걸립니다.
    - **Aurora Machine Learning**: Aurora에서 SageMaker 및 Comprehend를 사용하여 머신 러닝을 수행합니다.
    - **Aurora Database Cloning**: 스냅샷 복원보다 기존 클러스터에서 새로운 클러스터 생성이 더 빠릅니다.
- ex) RDS와 동일하지만 더 적은 유지 보수/더 큰 유연성/더 높은 성능/더 많은 기능을 활용해야 하는 경우에 사용합니다.

### ElastiCache

- 관리형 Redis / Memcached (RDS와 비슷한 기능 제공, 캐싱 작업에 활용)
- 인메모리 데이터 스토어, 데이터를 읽을 때 1밀리초 미만의 지연 시간 을 제공합니다.
- 캐시를 위한 EC2 인스턴스 유형을 프로비저닝 해야 합니다. (ex. cache.mg6.large)
- Redis용 클러스터 생성, 다중 AZ, 샤딩을 위한 읽기 전용 복제본 사용이 가능합니다.
- IAM, 보안 그룹, KMS, Redis 인증을 통한 보안 기능을 제공합니다.
- 백업, 스냅샷, 지정 시간 복구 기능을 제공합니다.
- 관리형 및 예약된 유지 관리 가능을 제공합니다.
- 애플리케이션 코드가 ElastiCache를 활용하도록 코드를 수정해야 합니다.
- ex) 키-값 스토어, 가 높은 읽기, 적은 쓰기, DB 쿼리 결과를 캐싱, 웹 사이트의 세션 데이터 저장

### DynamoDB

- AWS의 독점 기술, 관리형 서버리스 NoSQL 데이터베이스, 밀리초 수준의 지연 시간 제공, SQL 쿼리 사용 불가
- 용량 모드:
    - 선택형 오토 스케일링이 탑재된 프로비저닝된 용량 모드 - 점진적으로 늘어나거나 점진적으로 줄어드는 이중(double) 워크로드가 있을 때 유용합니다.
    - 온디맨드 용량 모드 - 용량 프로비저닝 필요 없음. 오토 스케일링이 실행되므로 워크로드를 예측하기 어려울 때나 데이터베이스의 수요가 급증할 때 유용합니다.
- ElastiCache 대신 DynamoDB에 키-값을 저장할 수 있습니다. (ex. 웹사이트 세션 데이터 저장, TTL 기능 사용)
- 고가용성, 기본적으로 다중 가용 영역에 걸쳐있기 때문, 읽기와 쓰기가 완전히 분리되어 있고 트랜잭션 기능을 제공합니다.
- 읽기 캐시용 DAX 클러스터, 마이크로초 읽기 지연 시간
- 보안, 인증, 권한 부여는 IAM을 통해 처리합니다.
- 이벤트 처리: AWS Lambda 또는 Kinesis Data Streams와 통합하기 위한 DynamoDB Streams
- GlobalTable 기능: 어느 리전에서든 읽고 쓸 수 있습니다. 다중 활성 (active-active)
- 백업 옵션:
    - PITR을 사용한 최대 35일까지의 자동 백업 - 새 테이블로 복원
    - 온디맨드 백업
- PITR 창 내에서 RCU(읽기 용량 단위)를 사용하지 않고 S3로 내보내기(export), WCU(쓰기 용량 단위)를 사용하지 않고 S3에서 가져오기(import)
- 스키마를 빠르게 전개할 때 유용합니다.
- ex) 400KB 미만의 문서를 다루는 작은 서버리스 애플리케이션 개발, 서버리스 캐시 분산

### S3

- 객체를 키-값으로 저장됩니다.
- 큰 객체를 저장할 때는 유용하지만, 여러 개의 작은 객체를 저장할 때는 유용하지 않습니다.
- 서버리스이며 무한한 확장이 가능합니다. 객체의 최대 크기는 5TB, 버전 관리 기능을 제공합니다.
- **계층**: S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier / 계층을 전환하려면 수명 주기 정책 사용을 사용해야 합니다.
- **기능**: 버저닝, 암호화, 복제, MFA-Delete, 액세스 로그
- **보안**: IAM, 버킷 정책, ACL, 액세스 포인트, S3 Object Lambda를 사용해 객체를 애플리케이션에 전송하기 전에 수정 가능, CORS, Glacier의 객체/볼트 잠금
- **암호화**: SSE-S3, SSE-KMS, SSE-C, 클라이언트 측 암호화, 전송 중 TLS, S3 버킷에 기본 암호화 설정이 가능합니다.
- **S3 Batch**를 사용한 일괄 작업, S3 인벤토리를 사용한 파일 목록 조회
- **성능**: 멀티파트 업로드, S3 Transfer Acceleration, S3 Select
- **자동화**: S3 Event Notifications (SNS, SQS, Lambda, EventBridge)
- ex) 정적 파일, 큰 파일을 위한 키-값 저장소, 웹사이트 호스팅