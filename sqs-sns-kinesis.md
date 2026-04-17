## SQS

<img width="446" height="193" alt="Image" src="https://github.com/user-attachments/assets/ccb15e29-15a3-4f7a-bb89-7d9cfd0e4343" />

- 대기열 서비스는 생산자와 소비자 사이를 분리하는 버퍼 역할을 합니다.
- AWS의 첫 번째 서비스 중 하나. 가장 오래된 서비스라고 할 수 있습니다.
- 완전 관리형 서비스. 애플리케이션을 **분리**하는데 사용됩니다.
- Attributes:
    - 무제한 처리량, 대기열에 저장 가능한 메시지 수의 제한이 없습니다.
    - 메시지 보존 기간의 기본 값은 4일, 최대 기간은 14일
    - 지연 시간이 짧음 (<10 ms on publish and receive)
    - 전송되는 메시지는 256 KB 로 제한됩니다.
- 중복 메시지가 발생할 수 있습니다. (at least once delivery, occasionally)
- ex) 쓰기 버퍼
- Producing Messages
    - SDK를 사용하여 SQS에 메시지를 생성하고, (SendMessage API) 소비자가 메시지를 읽고 삭제할 때까지 SQS 대기열에 유지합니다.
- Consuming Messages
    - SQS를 폴링하여 메시지를 처리(ex) RDS insert)하고 DeleteMessage API를 통해 메시지를 삭제합니다.

#### Multiple EC2 instances Consumers

<img width="441" height="296" alt="Image" src="https://github.com/user-attachments/assets/97ce2d2c-0ffa-45dd-b545-258fe729e461" />

- 소비자들은 메시지를 병렬로 수신 및 처리합니다.
- 최소 한번의 전송을 보장하게 되며, 최선의 노력으로 메시지 순서를 보장합니다.
- 소비자들은 메시지를 처리한 후 삭제해야 하며, 처리량 향상을 위해 소비자를 추가하고 수평 확장을 할 수 있습니다.

#### SQS with Auto Scaling Group (ASG)

<img width="450" height="223" alt="Image" src="https://github.com/user-attachments/assets/c21384b0-244f-4645-9937-d0a72d981501" />

1. 소비자는 asg 내부에서 ec2 인스턴스를 실행하고, sqs 대기열에서 메시지를 polling 합니다.
2. asg는 metric(대기열의 길이, approximateNumberOfMessages)에 따라 확장됩니다.
3. metric이 일정수준 이상 넘어가면 cloudwatch alarm을 설정하면 됩니다.

#### SQS to decouple between application tiers

<img width="442" height="162" alt="Image" src="https://github.com/user-attachments/assets/33facba7-87cb-4073-96fb-48e88a8340ee" />

* 계층 간 분리를 위해서도 SQS를 사용할 수 있습니다.

#### FIFO Queue

<img width="449" height="67" alt="Image" src="https://github.com/user-attachments/assets/36838511-bccc-43a1-adaf-43d423a1502c" />

- FIFO = First In First Out (ordering of messages in the queue)
- 초당 300개 메시지를 처리할 수 있지만, 배치를 적용할 경우 3000개까지 처리가 가능합니다.
- 중복 메시지를 제거하여 정확히 한번만 전송이 가능하며, 소비자에 의해 메시지는 순서대로 처리됩니다.

### **Amazon Kinesis**

- **실시간 스트리밍 데이터를 쉽게 수집하고 처리하여 분석**하기 위한 서비스
- **Kinesis Data Streams**: 데이터 스트림을 캡처, 처리 및 저장
- **Kinesis Data Firehose**: 데이터 스트림을 AWS 데이터 저장소로 로드
- **Kinesis Data Analytics**: SQL 또는 Apache Flink를 사용하여 데이터 스트림을 분석
- **Kinesis Video Streams**: 비디오 스트림을 캡처, 처리 및 저장
- Security
    - iam 정책을 사용하여 액세스 및 권한을 제어합니다.
    - HTTPS 암호화, vpc 내에 접근하기 위한 vpc endpoint, cloudtrail을 통한 모니터링 기능을 제공합니다.

#### Kinesis Data Streams

<img width="451" height="219" alt="Image" src="https://github.com/user-attachments/assets/20e9a0f9-4866-4679-941f-ac2f847d0fd1" />

- 보존 기간은 1일부터 365일 사이로 설정할 수 있습니다.
- 데이터를 재처리하거나 확인할 수 있습니다.
- Kinesis로 데이터가 삽입된 후에는 삭제할 수 없습니다. (불변성)
- 동일한 파티션을 공유하는 데이터는 동일한 샤드로 전송됩니다. (순서 보장). 키를 기반으로 데이터를 정렬할 수 있습니다.

#### Kinesis Data Firehose

<img width="455" height="238" alt="Image" src="https://github.com/user-attachments/assets/545709bd-4585-4ada-97df-9075ef4a8efc" />

- 완전 관리형 서비스로, 관리 작업이 필요하지 않으며 자동 스케일링 및 서버리스 기능을 제공합니다.
- Firehose를 통과하는 데이터를 제공합니다.
- 거의 실시간으로 데이터를 처리합니다.
- 데이터 전환, 변환, 압축, lambda, 백업 등 다양한 기능을 제공합니다.

#### SQS vs SNS vs Kinesis

**SQS**

- 소비자가 SQS 대기열에서 메시지를 요청하여 데이터를 가져오는 (pull) 모델
- 데이터를 처리한 후 소비자가 대기열에서 삭제하여 다른 소비자가 읽을 수 없도록 합니다.
- 소비자의 수에는 제한이 없으며, 관리형 서비스이기 때문에 처리량을 프로비저닝할 필요가 없습니다.
- FIFO 대기열을 사용해야 순서를 보장할 수 있습니다.
- 메시지에 지연 기능이 있습니다.

**SNS**

- 게시/구독 모델로 다수의 구독자에게 데이터를 푸시하면 메시지의 복사본을 받게 됩니다.
- SNS 주제별로 12,500,000 구독자를 가질 수 있습니다.
- 데이터가 SNS에 전송되면 지속되지 않습니다. (제대로 전달되지 않으면 데이터를 잃을 수 있음)
- 100,000 개의 주제로 확장 가능합니다.
- 처리량을 프로비저닝할 필요 없습니다.
- SQS와 결합하여 팬아웃 아키텍처 패턴을 이용할 수 있습니다.
- SNS FIFO 주제를 SQS FIFO 대기열과 결합할 수 있습니다.

**Kinesis**

- Standard: 소비자가 Kinesis로부터 데이터를 가져옵니다 (pull). 샤드당 2 MB/s
- Enhanced-fan out: Kinesis가 소비자에게 데이터를 푸시합니다. 샤드 하나에 소비자당 2 MB/s
- Kinesis 데이터 스트림에서는 데이터가 지속되기 때문에 데이터를 다시 재생할 수 있습니다.
- 실시간 빅데이터 분석, ETL 등에 사용됩니다. X days 후에 데이터가 만료됩니다.
- 프로비저닝 모드 혹은 온디맨드 용량 모드가 있습니다.
