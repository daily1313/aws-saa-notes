## **Amazon CloudWatch**

- CloudWatch는 AWS의 모든 서비스에 대한 지표를 제공합니다.
- 지표: 모니터링할 변수 (CPU, Mem)
- 지표는 시간을 기반으로 하기에 타임스탬프가 있어야 하며, 지표가 많아지면 CloudWatch 대시보드에 추가해 모든 지표를 할꺼번에 볼 수 있습니다.

### CloudWatch Metirc Streams

<img width="439" height="433" alt="Image" src="https://github.com/user-attachments/assets/dfa94c9a-1584-4364-98a8-f9c4cf4973be" />

- CloudWatch 지표는 CloudWatch 외부로 스트리밍할 수 있습니다.
- CloudWatch 지표를 선택한 대상으로 지속적으로 스트리밍하면 **거의 실시간으로 전송**되고 지연 시간을 짧아집니다.
    - Amazon Kinesis Data Firehose를 통해 다른 목적지로 이동시킬 수 있습니다.

### CloudWatch Logs

- **로그 그룹 (Log groups)**: 로그 그룹은 동일한 보존 기간, 모니터링 및 액세스 제어 설정을 공유하는 로그 스트림 그룹을 정의합니다. 각 로그 스트림은 하나의 로그 그룹에 속해야 합니다. 일반적으로 애플리케이션을 나타내는 임의의 이름입니다.
- **로그 스트림 (Log stream)**: 로그 스트림은 동일한 소스를 공유하는 로그 이벤트 시퀀스입니다. 애플리케이션 내의 인스턴스, 로그 파일, 컨테이너 등을 나타냅니다.
- 로그의 유효 기간을 지정할 수 있으며, 로그를 영원히 유지하거나 30일 등으로 설정할 수 있습니다.
- **CloudWatch Logs는 다양한 대상으로 로그를 전송할 수 있습니다.**
    - Amazon S3 (exports), Kinesis Data Streams, Kinesis Data Firehose, AWS Lambda, OpenSearch

### **CloudWatch Alarms**

- 경보는 모든 지표에 대한 알림을 트리거하는 데 사용됩니다.
- 다양한 옵션을 제공합니다. (샘플링, %, 최대값, 최소값 등)
- 경보의 타겟
    - ec2 instance: 중지, 종료, 재부팅, 복구에 사용합니다.
    - ec2 auto scaling: scale-out, scale-in
    - amazon sns: sns 서비스에 알림을 보낼 때 사용합니다.

### **Amazon EventBridge**

- 예전 이름: cloudwatch event
- schedule: cron 작업 (스크립트 예약)
- 대상이 다양하다면, lambda 함수를 트리거해서 sqs, sns 메시지 등을 보낼 수 있습니다.

<img width="440" height="64" alt="Image" src="https://github.com/user-attachments/assets/9c2eeac1-de9e-484b-99cc-1f03e1b080e4" />

### **CloudWatch Insights**

- **CloudWatch Container Insights에서는** 컨테이너에서 발생하는 **지표와 로그**를 수집, 집계, 요약하는 기능을 제공합니다.
    - ecs, eks, fargate를 통한 container에서 사용이 가능합니다.
- lambda에서는 cloudwatch lambda Insight를 통해 모니터링 기능을 제공합니다.
- 또한 상위 n개의 기여자에 대한 지표를 확인하는 cloudwatch contributor Insight, 애플리케이션에서 제공하는 cludwatch application lnsight 기능도 제공합니다.

### **AWS CloudTrail**

<img width="450" height="192" alt="Image" src="https://github.com/user-attachments/assets/a22972de-5c3d-4d99-b2d3-64322f14c106" />

- **AWS 계정의 거버넌스, 규정 준수 및 감사를 제공하는 서비스**
- CloudTrail은 기본적으로 활성화되어 있습니다.
- AWS 계정 내에서 이루어진 이벤트의 호출 및 API 호출의 이력을 얻을 수 있습니다.
    - 콘솔, SDK, CLI 뿐만 아니라 기타 AWS 서비스에서 발생한 AWS 계정 내의 모든 이벤트 및 API 호출을 기록합니다.
- CloudTrail은 로그를 CloudWatch Logs 또는 S3에 저장할 수 있습니다.
- 전체 또는 단일 리전에 적용되는 Trail(트레일)을 생성해 모든 리전에 걸친 이벤트 기록을 한 곳으로 모을 수 있습니다.
- AWS에서 리소스가 삭제되었을 경우에는 먼저 CloudTrail을 통해 조사해 보면 됩니다.
- CloudTrail Insights Event: 계정에서 비정상적인 행도 ㅇ감지 (부정확한 리소스 프로비저닝, 서비스 한도도달,  ..)

### **AWS Config**

- AWS 리소스의 감사 및 규정 준수 여부를 기록할 수 있게 해주는 서비스
- 시간에 따른 구성 및 변경 사항 기록을 지원합니다.
- AWS Config로 해결할 수 있는 문제:
    - 보안 그룹에 무제한 SSH 액세스가 있는지 여부
    - 내 버킷에 공개 액세스가 있는지 여부
    - 시간이 지나면서 ALB 구성이 어떻게 변경되었는지 등
- 변경 사항에 대한 알림 (SNS 알림)을 받을 수 있습니다.
- AWS Config는 리전별 서비스이기 때문에 모든 리전별로 구성해야 합니다.
- 리전과 계정 간 데이터를 통합할 수 있습니다.
- 구성 데이터를 S3에 저장하여 Athena로 분석할 수 있습니다.
- AWS 리소스가 규정을 미준수했을 때 마다 EventBridge를 사용해 알림을 트리거할 수 있습니다.

<img width="415" height="58" alt="Image" src="https://github.com/user-attachments/assets/22db0455-3f4d-4123-b018-babe072fb4e4" />

### **CloudWatch vs CloudTrail vs Config (In ELB)**

**CloudWatch**

- 들어오는 연결 수 모니터링
- 시간에 따른 오류 코드 비율 시각화
- 로드 밸런서 성능 파악을 위한 대시보드 생성

**CloudTrail**

- API 호출로 로드 밸런서에 대한 변경 사항을 수행한 사용자 추적

**Config**

- 로드 밸런서의 보안 그룹 규칙 추적
- 로드 밸런서의 구성 변경 추적
- SSL 인증서가 로드 밸런서에 항상 할당되어 있어야 한다는 규칙을 만들어 암호화되지 않은 트래픽이 로드 밸런서에 접근하지 못하게 할 수 도 있습니다.