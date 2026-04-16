## Advanced Amazon S3

- 스토리지 클래스 간에 객체 이동이 가능합니다.
    - 자주 액세스 되지 않는 객체: Standard IA
    - 빠른 액세스가 필요하지 않은 아카이브 객체: Glacier, Glacier Deep Archive
    - 객체는 수동으로 옮기거나 수명 주기 규칙을 이용해서 자동화할 수 있습니다.

#### Lifecycle Rule

Transition Actions 전환 작업: 객체를 다른 스토리지 클래스로 전환하기 위해 구성

- 객체 생성 후 60일 이후 Standard IA로 이동 → 6개월 이후 아카이빙을 위해 Glacier로 이동

Expiration Actions 만료 작업: 일정 시간 후에 객체를 삭제 또는 만료되도록 구성

#### **Amazon S3 Analytics - Storage Class Analysis**

- 객체를 스토리지 클래스로 전환할 때 도움되며 Standard or Standard IA용으로 권장합니다.
- 보고서는 매일 업데이트되며, 데이터 분석 시간은 24 ~ 48 시간 정도 소요됩니다.

#### S3 - Requester Pays

<img width="439" height="317" alt="Image" src="https://github.com/user-attachments/assets/214a29a2-1b4a-474a-88df-4436d6709f00" />

- 버킷 소유자는 버킷과 관련된 모든 S3 스토리지 및 데이터 전송 비용을 부담합니다.
- 요청자가 비용을 부담하는 Requester Pays 버킷의 경우, 버킷 소유자 대신 요청자가 요청 및 버킷에서 객체 다운로드 비용을 지불해야 합니다.

#### S3 Event Notifications

<img width="426" height="517" alt="Image" src="https://github.com/user-attachments/assets/33007c71-1823-4003-b203-bbfe26713aac" />

- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication 등의 이벤트 유형 필터링
- 원하는 만큼 많은 S3 event를 생성할 수 있습니다.

#### S3 Event Notifications with Amazon EventBridge

<img width="438" height="102" alt="Image" src="https://github.com/user-attachments/assets/1c890271-fd7e-4d88-b59d-7d0bf20864e3" />

- Json 규칙을 사용한 Advanced filtering 옵션
- Multiple Destinations: Step Functions, Kinesis Streams / Firehose 등

#### **S3 - Baseline Performance**

- Amazon S3는 요청이 아주 많을 때 자동으로 확장되며, 지연 시간도 100-200ms 수준으로 아주 짧습니다.
- 버킷 내 접두사(prefix)당 초당 적어도 3,500개의 PUT/COPY/POST/DELETE 요청 또는 5,500개의 GET/HEAD 요청을 처리할 수 있습니다.

#### **S3 Batch Operations**

- 단일 요청으로 기존 S3 객체에 대한 대량 작업을 수행하는 서비스
    - 객체 메타데이터 및 속성 수정, S3 버킷 간 객체 복사, 암호화되지 않은 객체 암호화, ACL, 태그 수정, S3 Glacier에서 객체 복원, Lambda 함수를 호출하여 사용자 지정 작업 수행
    - 사례: S3 inventory를 사용하여 암호화되지 않은 객체를 찾은 후 S3 Batch Operations를 사용해 한번에 모두 암호화