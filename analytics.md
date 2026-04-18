## Athena

<img width="421" height="664" alt="Image" src="https://github.com/user-attachments/assets/d241e337-f13d-4d6d-8879-d0e975ff93e1" />

- Amazon S3에 저장된 데이터를 분석하기 위한 서버리스 쿼리 서비스
- 표준 SQL 언어를 사용하여 파일을 쿼리합니다. (Presto 엔진에 빌드)
- CSV, JSON, ORC, Avro, Parquet 등 다양한 형식을 지원합니다.
- 보고서 및 대시보드를 위해 QuickSight랑 함께 사용됩니다.
- tips: 서버리스 SQL 엔진을 사용한 Amazon S3 데이터 분석 (athena)

### Federated Query

<img width="448" height="489" alt="Image" src="https://github.com/user-attachments/assets/21de7c3f-7424-426f-9985-d521e577b772" />

- 관계형, 비관계형, 객체, 사용자 지정 데이터 소스 (AWS 또는 온프레미스)에 저장된 데이터를 대상으로 SQL 쿼리를 실행할 수 있도록 합니다.
- 연합 쿼리를 실행하기 위해 AWS Lambda에서 실행되는 데이터 소스 커넥터를 사용합니다.
- 쿼리 결과를 Amazon S3에 저장합니다.

### Redshift

<img width="447" height="171" alt="Image" src="https://github.com/user-attachments/assets/bf12f438-0486-4e36-8d7a-e92f289c5664" />

- 온라인 분석 처리 OLAP(Online Analytical Processing) 유형의 데이터베이스 - 분석 및 데이터 웨어하우징에 사용합니다.
- 다른 데이터 웨어하우스에 비해 10배 빠른 성능을 제공하며 PB 단위의 데이터까지 확장이 가능합니다.

### Amazon OpenSearch Service

- Amazon OpenSearch Service는 Amazon ElasticSearch의 후속 서비스입니다. (라이선스 문제로 이름이 변경되었습니다)
- DynamoDB에서는 기본 키나 데이터베이스의 인덱스로만 데이터를 쿼리할 수 있지만 **OpenSearch에서는 부분적으로 일치하는 필드를 포함해 모든 필드를 검색할 수 있습니다.**
- 일반적으로 OpenSearch는 다른 데이터베이스를 보완하기 위해 사용됩니다.
- OpenSearch를 생성하고 사용하기 위해서는 인스턴스의 클러스터를 생성해야 합니다. (서버리스 서비스 X)