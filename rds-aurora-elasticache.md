## RDS

- RDS: 관계형 데이터베이스 서비스(Relational Database Service)의 약자.
- SQL을 쿼리 언어로 사용하는 데이터베이스용 관리형 데이터베이스 서비스.
- AWS에서 관리되는 클라우드 상의 데이터베이스를 생성할 수 있도록 해줍니다.
    - PostgresSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server, Aurora (AWS Proprietary database)

### **RDS – Storage Auto Scaling**

- 데이터베이스 스토리지가 부족해지고, RDS 스토리지 Auto Scaling 기능이 활성화되어 있으면 RDS가 이를 감지하여 자동으로 스토리지를 확장해 줍니다.
- 수동으로 스케일링 할 필요가 없습니다.
- 스토리지 임계값을 설정
    - ex) 스토리지 잔여 공간 10%, 스토리지 부족 상태가 5분 이상 지속
- 워크로드를 예측할 수 없는 애플리케이션에 적합합니다.

### **RDS Read Replicas for read scalability**

<img width="447" height="307" alt="Image" src="https://github.com/user-attachments/assets/4b138e1a-1261-40e1-8bf0-651502aa7e8f" />

- 최대 15개 복제본 생성이 가능합니다. (Read Replica)
- 동일한 가용 영역 or 가용 영역이나 리전을 걸쳐 생성이 가능합니다.
- 복제는 비동기 방식으로 진행되며 Read Replica를 사용하기 위해 애플리케이션에 모든 연결을 업데이트 해야합니다.

<img width="449" height="478" alt="Image" src="https://github.com/user-attachments/assets/861f0532-70e5-4cfe-b667-01747f382a7b" />

- Production DB에서는 read, write를 모두 처리하지만, read replica에서는 read만 처리합니다.

### Read Replica Cost

<img width="446" height="158" alt="Image" src="https://github.com/user-attachments/assets/9cf40d4e-4d69-417a-86a3-2574ddb8764b" />

- AZ → AZ 이동 시: 비용 발생
- 동일한 Region 내에서는 비용 발생 X

### **RDS Multi AZ (Disaster Recovery)**

- 동기식 복제, 가용성 향상
- AZ 손실, NET 손실 시 Failover → standby DB가 master로 승격합니다.
- 읽기 전용 복제본은 DR을 위해 Multi-AZ로 설정될 수 있습니다.

## Aurora

- AWS 고유기술로 오픈 소스는 아닙니다.
- Postgres, MySQL이 호환됩니다.
- AWS 클라우드 최적화되었으며, RDS MySQL, Postgres에 비해 성능이 각각 5배, 3배 뛰어납니다.
- 스토리지는 10G 단위로 자동으로 증가하며 최대 128TB까지 확장이 가능합니다.
- Failover는 즉시 이루어지며 가용성이 뛰어납니다.
- RDS보다 비용이 20% 정도 높지만, 스케일링 측면에서 훨씬 뛰어납니다.

### Aurora Serverless

- 실제 사용량이 기반하여 자동으로 데이터베이스 인스턴스화 및 자동 스케일링이 이루어집니다.
- 비정기적, 간헐적, 또는 예측할 수 없는 워크로드에 적합합니다.
- 용량 계획이 필요하지 않으며, 초당 결제로 인해 더 경제적일 수 있습니다.

### **Aurora Multi-Master**

- 쓰기 노드의 즉각적 장애 조치(failover)가 필요한 경우. (쓰기 노드에 높은 가용성을 갖추고자 할 때)
- Aurora 클러스터의 모든 노드에서 읽기 및 쓰기를 수행합니다.

### **Global Aurora**

#### **Aurora Cross Region Read Replicas**

- **재해 복구에 유용**하며 간단하게 구성이 가능합니다.

**Aurora Global Database (recommended)**

- 하나의 기본 리전 (읽기 / 쓰기 모두 가능)
- 최대 5개의 읽기 전용 리전. 복제 지연이 1초 미만 걸립니다.
- 각 보조 리전당 최대 16개의 읽기 전용 복사본을 생성할 수 있습니다.

### **Aurora Machine Learning**

- ML 기반 예측을 SQL 인터페이스로 애플리케이션에 적용하는 것.
- Aurora와 다양한 AWS 머신 러닝 서비스 간에 쉽고 최적화된 방식으로 안전하게 통합할 수 있습니다.
- ex) Amazon SageMaker (모든 ML과 함께 사용), Amazon Comprehend(감정 분석)

## Backups

### RDS Backups

**자동 백업**

- 매일 자동으로 데이터베이스 유지 관리 시간에 데이터베이스 전체 백업
- 매 5분마다 트랜잭션 로그 백업
    - 가장 오래된 백업부터 5분 전 백업까지 어느 시점으로도 복원이 가능합니다.
- 자동 백업 보유 기간은 1일부터 35일까지 설정 가능. 이 기능을 비활성화하면 0으로 설정하면 됩니다.

**수동 DB 스냅샷**

- 사용자가 수동으로 트리거해야 하며, 원하는 기간 동안 백업을 보존할 수 있습니다.

### Aurora Backups

**자동 백업**

- 1일부터 35일까지 (비활성화 불가능)
- 지정 시간 복구 기능 - 정해진 시간 범위 내의 어느 시점으로도 복구가 가능합니다.

**수동 DB 스냅샷**

- 사용자가 수동으로 트리거해야 하며, 원하는 기간 동안 백업을 보존할 수 있습니다.

### RDS & Aurora Restore options

- **RDS / Aurora 백업 또는 스냅샷**을 새로운 데이터베이스로 복원할 수 있습니다.
- **S3에서 MySQL RDS DB** 복원
    - 온프레미스 DB의 백업을 생성
    - 객체 스토리지인 Amazon S3에 저장
    - MySQL을 실행하는 새로운 RDS 인스턴스로 백업 파일 복원
- **S3에서 MySQL Aurora 클러스터** 복원
    - 온프레미스 DB의 백업 생성 - Percona XtraBackup 소프트웨어 사용
    - 백업 파일을 Amazon S3에 저장
    - MySQL을 실행하는 새로운 Aurora 클러스터로 백업 파일 복원

### Aurora Database Cloning

- 기존 데이터베이스로부터 새로운 Aurora DB 클러스터를 생성합니다.
- 스냅샷 및 복원보다 빠릅니다.
- 새 DB 클러스터는 원래 클러스터와 동일한 클러스터 볼륨과 데이터를 사용하며, 데이터 업데이트가 끝나면 변경됩니다.
- 데이터베이스 복제는 매우 빠르고 비용 면에서 효율적입니다.
- 프로덕션(production) 데이터베이스에 영향을 주지 않고 스테이징(staging) 데이터베이스를 생성하는데 유용합니다.

### RDS & Aurora Security

- **At-rest encryption**
    - AWS KMS를 사용하여 데이터베이스 마스터 및 복제본 암호화 - 이는 데이터베이스를 처음 실행할 때 정의됩니다.
    - 마스터가 암호화되지 않은 경우 읽기 복제본을 암호화할 수 없습니다.
    - 암호화되지 않은 기존 데이터베이스를 암호화하려면 DB 스냅샷을 통해 암호화된 DB 형태로 데이터베이스 스냅샷을 복원해야 합니다.
- **In-flight encryption**: 기본적으로 TLS 사용 가능. 클라이언트는 AWS TLS 루트 인증서 사용해야 합니다.
- **IAM 인증**: 사용자 이름/암호 대신 IAM Role을 사용하여 DB에 연결이 가능합니다.
- **보안 그룹**: RDS/AUrora DB에 대한 네트워크 액세스 제어
- **SSH 액세스 불가능** (RDS Custom 제외)
- **감사 로그 활성화** - CloudWatch Logs로 전송하여 장기간 보관이 가능합니다.

### Amazon RDS Proxy

<img width="447" height="541" alt="Image" src="https://github.com/user-attachments/assets/38b956c3-b8c1-4fa9-86ce-36a4150f3fd7" />

- Fully managed database proxy for RDS
- DB Connection Pool 생성 및 공유가 가능합니다.
- DB 리소스 부하를 줄일 수 있으며, DB의 효율성을 향상시킵니다.
- DB에 대한 IAM 인증을 강제함으로써 IAM 인증을 통해서만 RDS DB 인스턴스에 연결할 수 있습니다. 이때 자격 증명은 AWS Secrets Manager에 안전하게 저장됩니다.
- RDS 프록시는 퍼블릭 액세스가 절대 불가능합니다. (VPC에서만 접근 가능)

## **ElastiCache**

- RDS와 동일한 방식으로 관계형 데이터베이스를 관리할 수 있습니다.
- ElastiCache는 Redis 또는 Memcached와 같은 캐시 기술을 관리할 수 있습니다.
- Cache: 매우 높은 성능과 낮은 지연 시간을 가진 인메모리 데이터베이스
- 읽기 집약적인 워크로드에서 데이터베이스의 부하를 줄이는데 도움이 됩니다.
- 애플리케이션을 stateless로 만드는데 도움이 됩니다.
- AWS는 OS 유지 관리/패치, 최적화, 설정, 구성, 모니터링, 장애 복구 및 백업을 처리
- **ElastiCache를 사용하는 것은 애플리케이션 코드 변경이 많이 필요합니다.**
- Security
    - IAM 인증을 지원하지 않습니다.
    - ElastiCache에서 정의할 IAM 정책은 AWS API 수준의 보안에서만 사용됩니다.

### ElastiCache Solution Architecture

- **DB Cache**
    - 애플리케이션은 ElastiCache에 쿼리를 보내고, ElastiCache에 데이터가 없는 경우 RDS에서 가져와 ElastiCache에 저장합니다.
    - 만료 전략을 가져야 합니다. (invalidation strategy)
- **User Session Store**
    - 사용자가 애플리케이션에 로그인 할 경우 애플리케이션은 세션 데이터를 ElastiCache에 기록합니다.
    - 이후 애플리케이션의 다른 인스턴스로 redirect 될 때 캐시에 존재하는 로그인 정보를 활용할 수 있습니다.

### **Redis vs Memcached**

- Redis
    - 자동 장애 조치(Auto-Failover)를 위한 **Multi AZ**
    - 읽기 스케일링에 사용되며 가용성이 높은 읽기 전용 복제본입니다.
    - 백업 및 복원 기능을 제공합니다.
    - 세트(Sets)와 정렬 세트(Sorted Sets)를 지원합니다.
- Memcached
    - 데이터 파티셔닝(sharding)을 위한 다중 노드 를 사용합니다.
