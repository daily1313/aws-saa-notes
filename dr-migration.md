### **Disaster Recovery**

- 회사의 사업 지속이나 재정에 부정적인 영향을 미치는 모든 사건은 재해로 간주됩니다.
- 재해 복구(DR)는 이러한 재해에 대비하고 재해가 발생할 시 복구하는 작업을 의미합니다.
- 유형
    - on-premise → on-premise : 비용이 가장 많이 듭니다.
    - 온프레미스 → AWS 클라우드: 하이브리드 복구
    - AWS 클라우드 리전 A → AWS 클라우드 리전 B
- 핵심 용어
- RPO: Recovery Point Objective - 복구 시점 목표
    - 얼마나 자주 백업을 실행할지, 시간 상 어느 정도 과거로 되돌릴 수 있는지 결정합니다.
    - 재해 발생 시 데이터 손실을 얼마만큼 감수할지 설정하는 것
- RTO: Revobery Time Objective - 복구 시간 목표
    - 재해 발생 후 복구할 때 사용합니다.

### Strategies

- **시간에 따라 데이터를 백업** - AWS Storage Gateway
- **수명 주기 정책**을 만들어 비용 최적화 목적 - Glacier
- 일주일에 한 번씩 많은 양의 데이터를 **Glacier**로 전송 - AWS Snowball
    - RPO는 대략 일주일
- AWS Cloud를 사용하는 경우 (EBS 볼륨, Redshift, RDS) 정기적으로 스냅샷을 예약하고 백업해두면 스냅샷을 만드는 기간에 따라 RPO가 달라집니다.

### **Disaster Recovery Tips**

**백업**

- EBS 스냅샷, RDS 자동화된 백업 / 스냅샷 등 사용
- S3 / S3 IA / Glacier로의 정기적인 백업, 수명 주기 정책 실행, 교차 리전 복제 가능
- 온프레미스에서 클라우드로 데이터 공유 시 Snowball이나 Storage Gateway를 사용합니다.

**고가용성**

- Route53을 사용하여 DNS를 다른 리전으로 마이그레이션
- RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
- Direct Connect에서의 복구용 Site to Site VPN

**복제**

- RDS 복제 (리전간 복제), AWS Aurora, Aurora Global Database
- 온프레미스에서 RDS로의 데이터베이스 복제 시 복제 소프트웨어 사용 가능
- Storage Gateway

**자동화**

- CloudFormation / Elastic Beanstalk을 사용하여 전체 환경 재생성
- CloudWatch 경보가 실패했을 때 EC2 인스턴스 복구 / 다시 시작

### DMS – Database Migration Service

- 빠르고 안전하게 DB를 AWS로 마이그레이션하며, 복원성이 좋고 자가 복구 기능을 제공합니다.
- 마이그레이션 중에도 원본 DB는 사용이 가능합니다.

### **AWS Schema ConversionTool (SCT)**

<img width="441" height="92" alt="Image" src="https://github.com/user-attachments/assets/906c52a2-c51b-4910-b158-8f0e0022bf4e" />

데이터베이스의 스키마를 다른 엔진으로 변환합니다.