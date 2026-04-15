### EC2

- 클라우드 가상서버로서 가장 유명한 AWS 서비스 중 하나입니다.
- Elastic Compute Cloud의 약자입니다.
- EC2 내에서 다음과 같은 서비스가 포함됩니다.
    - EC2: 가상 머신 대여
    - EBS: 가상 드라이브에 데이터를 저장
    - ELB: 데이터를 분산
    - ASG: 서비스를 스케일링
- EC2를 이해하는 것이 클라우드 서비스를 이해하는 것입니다.

### EC2 Configuration

- OS: Linux, Windows or MacOS
- CPU: 가상 머신에 사용할 컴퓨팅 성능과 코어의 개수
- RAM: Random Access Memory(RAM)의 크기
- Storage: EBS & EFS, EC2 Instance Store
- Network Card: Speed, Public IP
- Firewall rules: Security Group
- Bootstrap script: EC2 User Data

### **EC2 User Data**

- EC2 인스턴스에 부팅될 때 실행되는 스크립트 또는 데이터를 의미합니다. 주로 EC2 인스턴스의 초기 설정을 자동화하는 데 사용됩니다.
- bootstrapping: 머신이 작동될 때 명령을 시작하는 것으로서 부트 작업을 자동화하기 때문에 부트스트래핑이라는 이름을 갖게 되었습니다.
- 스크립트는 처음 시작할 때 한 번만 실행되고 다시 실행되지 않습니다.
- ex) 업데이트, 소프트웨어 설치, 다운로드 등.
- User Data Script에 작업을 더 추가할 수록 부팅 시 인스턴스가 할 일이 더 늘어납니다.
- EC2 User Data Script는 root user에서 실행됩니다.

### EC2 Instance Types

- **General Purpose** (범용)
    - 웹 서버나 코드 레파지토리 같은 다양한 작업에 적합합니다.
    - 컴퓨팅, 메모리, 네트워크 간의 균형도 잘 맞습니다.
- **Compute Optimized** (컴퓨팅 최적화)
    - 컴퓨터 집약적인 애플리케이션에 최적화된 인스턴스이며 높은 퍼포먼스를 자랑합니다.
    - ex) 배치, 미디어 코딩, 머신 러닝 등
    - 전부 C로 시작합니다.
- **Memory Optimized** (메모리 최적화)
    - 메모리에서 대규모 데이터 세트를 처리하기 위해 사용합니다.
    - ex) High Performance RDBMS, In-memroy Database, Cache Store
- **Storage Optimized** (스토리지 최적화)
    - 로컬 스토리지에서 **매우 큰 데이터 세트**에 대해 많은 순차적 읽기 및 쓰기 액세스를 요구하는 워크로드를 위해 설계되었습니다.
    - ex) RDBMS, NoSQL, In-memory DB, Data Warehouse, File Systems..
- **Naming Convention**
    - ex) m5.2xlarge
        - m : 인스턴스 클래스
        - 5 : 제네레이션 - AWS에서 계속 업데이트
        - 2xlarge : 인스턴스 클래스의 사이즈 - CPU, Memory, RAM

### **Security Groups**

- 보안 그룹은 EC2 인스턴스의 "방화벽"입니다.
- 보안 그룹은 AWS에서 네트워크 보안을 실행하는데 핵심이 됩니다.
- EC2 인스턴스에 들어오고 나가는 트래픽을 제어합니다.
- 보안 그룹은 **허용(allow)** 규칙만 포함합니다.
- 규제 항목 - 포트, IP Ranges, inbound, outbound
    - 여러개의 인스턴스에 적용할 수 있습니다.
    - 리전과 VPC에 종속되어 있습니다.
    - inbound는 all block이 기본값입니다.
    - outbound는 all allowed가 기본값입니다.
    - SG간의 허용 규칙을 설정해줄 수 있습니다.

### Port

- **22 = SSH (Secure Shell)**: log into a Linux instance
- **21 = FTP (File Transfer Protocol)**: upload files into a file share
- **22 = SFTP (Secure File Transfer Protocol)**: upload files using SSH
- **80 = HTTP**: access unsecured websites
- **443 = HTTPS**: access secured websites
- **3389 = RDP (Remote Desktop Protocol)** - log into a Windows instance

### **EC2 Purchasing Options**

#### On Demand

- 사용한 만큼 지불합니다.
    - Linux or Windows: 1분 이후 초 단위로 청구합니다.
    - All other OS: 시간 단위로 청구합니다.
- 비용이 가장 많이 들지만 바로 지불할 금액이 없습니다.
- 장기적인 약정이 필요 없습니다.
- **단기적이고 중단 없는 워크로드나 애플리케이션을 예측하기 힘들때 사용하면 좋습니다.**

#### **Reserved Instances**

- On Demand에 비해 최대 72% 저렴합니다.
- Instance Type, Region, Tenancy, OS를 미리 설정합니다.
- 1년과 3년 계약시 할인율이 상이합니다
- Convertible Reserved Instance - 위의 설정을 변경할 수 있지만 최대 할인폭이 66%입니다.

#### **Savings Plans**

- 장기간 사용 시 할인 받을 수 있음 (RIs와 같이 최대 72%)
- 1년 혹은 3년 동안 시간당 $10로 약정합니다.
- 사용량이 한도를 넘어서게 되면 On-demand 가격으로 청구합니다.

#### Spot Instance

- On-demand랑 비교했을 때 최대 90% 할인률을 자랑합니다.
- 최대 가격을 정의하고 스팟 가격이 사용자가 지정한 최대 가격을 넘어갈 경우 인스턴스가 **사라질 수 있습니다.**
- **인스턴스 고장에 회복력이 있다면 가장 비용적으로 효율적**입니다.

#### Dedicated Hosts

- EC2 인스턴스 용량이 완전히 사용자 전용으로 할당된 물리적 서버
- 법규 준수 요건 혹은 규정 준수 요구사항을 만족할 수 있으며, 기존의 서버 바운드  소프트웨어 라이선스를 사용할 수 있게 해줍니다.
- 구매 옵션
    - On-demand: 초당 비용 지불
    - Reserved:  1 or 3 years

#### Dedicated Instances

- 사용자 전용 하드웨어에서 인스턴스가 실행됩니다.
- 동일한 계정 내 다른 인스턴스와 하드웨어를 공유할 수 있습니다.
- Dedicated Instance는 사용자만의 인스턴스를 자신만의 하드웨어에 갖는 반면, Dedicated Hosts는 물리적 서버 자체에 접근권을 갖고, 낮은 수준의 하드웨어에 대한 가시성을 제공합니다.

#### Capacity Reservation

- 지정된 특정 A-Z에서 필요한 기간동안 On-demand 인스턴스 용량을 예약할 수 있습니다.
- 필요할 때 언제든지 EC2 용량에 액세스할 수 있습니다.
- 약정기간이 없으며, 요금 할인이 없습니다.

#### 상황에 따라서 어떤 구매 옵션이 적합한가

- On-demand: 원할 때 사용하고 전액 지불할 경우
- Reserved Instance: 미리 계획을 세우고 오랜 시간 사용할 경우
- Savings Plan: 일정 기간 매시간 일정 금액을 지불할 경우
- Spot lnstance: 최대한 싼 값에 구매하는 대신, 리스크를 감수할 경우
- Dedicated Hosts: 전체 예약
- Capacity Reservations: 일정 기간동안 예약하고, 필요할 때 마다 사용할 경우

### EC2 Spot Instance Requests

- On-demand에 비해 90%의 할인율을 제공합니다.
- max spot price 설정합니다.
    - current spot price < max spot price일 경우 인스턴스를 얻을 수 있습니다.
    - current spot price > max spot price일 경우 인스턴스가 중지 및 종료될 수 있습니다.

#### Spot Instances 종료 방법
<img width="439" height="253" alt="Image" src="https://github.com/user-attachments/assets/e3d572e7-4e0a-43b7-81c0-16511f6a6f86" />

- One-time request
    - spot request가 이행되는 즉시 인스턴스가 시작됩니다.
    - 일회성 요청이기에 spot-request가 사라져도 괜찮은 경우 One-time Request를 사용하면 됩니다.
- Persistent request
    - Spot request의 Valid from ~ until 까지의 유효기간 동안 사용자가 요청한 개수의 인스턴스들이 유효합니다.
    - 외부 이유로 인하여 인스턴스가 중지될 경우, Spot reuqest가 다시 검증되고 Spot Instance가 재시작됩니다.
    - Spot request가 취소되기 위해서는 open, active, disabled 상태여야 하며, Spot Request 요청을 취소한 이후 연관된 Spot Instance를 종료해야 합니다.

#### Elastic IP

- EC2 인스턴스를 중지하고 다시 시작하면, 해당 인스턴스의 퍼블릭 IP가 변경될 수 있습니다.
- 고정된 퍼블릭 IP가 필요한 경우 탄력적 IP가 필요합니다.
    - Elastic IP: 인스턴스를 삭제하지 않는 한 사용자가 소유하고 있는 퍼블릭 IPv4
- 탄력적 IP 주소를 사용하면 인스턴스나 소프트웨어의 오류를 빠르게 대처하여 주소를 계정 내 다른 인스턴스에 빠르게 다시 매핑할 수 있습니다.
- 1개의 AWS 계정은 최대 5개의 Elastic IP를 가질 수 있다.
- 일반적으로 권장하지 않습니다. (DNS 권장)

#### **Private vs Public IP In AWS EC2**

- Private IP: 내부 AWS 네트워크
- Public IP: 인터넷(WWW)을 위한 퍼블릭 IP

#### **Placement Groups(배치 그룹)**

1. 클러스터(cluster): 단일 가용 영역 내에서 지연 시간이 짧은 하드웨어 설정으로 인스턴스를 그룹화합니다.

<img width="410" height="112" alt="Image" src="https://github.com/user-attachments/assets/9a9d745c-47b1-4328-8cda-2627d6b3cb2e" />

- 장점 : 네트워크 속도가 빠릅니다.
- 단점 : 하나의 렉에 전부 존재하기 때문에 하드웨어 Fail시 전체에 장애가 발생합니다.

2. 분산(Spread): 인스턴스를 다른 하드웨어에 분산시킵니다.

<img width="402" height="286" alt="Image" src="https://github.com/user-attachments/assets/e4948023-f26c-47f9-ab00-195a72b02b70" />

- 장점: 여러개의 AZ에 인스턴스를 생성하여 가용성을 높일 수 있습니다. (동시 실패 위험 감소)
- 단점: 1개의 AZ당 7개의 인스턴스로 제한합니다.

3. 파티션(Partition): 분산

<img width="417" height="343" alt="Image" src="https://github.com/user-attachments/assets/e1929eb5-f717-4280-bcca-e12756465b24" />

- 한개의 AZ에서 총 7개의 파티션을 생성할 수 있습니다.
- 총 100개의 인스턴스까지 지정 가능합니다.
- 하나의 파티션에 존재하는 인스턴스들은 하드웨어를 공유하지 않습니다.
- ex) HDFS, HBase, Cassandra, Kafka

#### Elastic Network Interfaces (ENI)

- 탄력적 네트워크 인터페이스. EC2 인스턴스가 네트워크에 액세스할 수 있게 도와줍니다.
- 가상 네트워크 카드를 나타내는 VPC의 논리적 구성 요소입니다.
- ENI 속성
    - VPC의 IPv4 주소 범위 중 기본 프라이빗 IPv4 주소
    - VPC의 IPv4 주소 범위 중 하나 이상의 보조 프라이빗 IPv4 주소
    - 프라이빗 IPv4 주소당 한 개의 탄력적 IP 주소 (IPv4)
    - 한 개의 퍼블릭 IPv4 주소
    - 한 개 이상의 IPv6 주소
    - 하나 이상의 보안 그룹
    - MAC 주소
- EC2 인스턴스와 독립적으로 ENI를 독립적으로 생성하고 즉시 연결하거나, 장애 조치를 위해 ENI를 EC2 인스턴스에서 이동시킬 수 있습니다.

#### EC2 Hibernate (최대 절전 모드)

- 인스턴스를 다시 시작하면, 운영체제가 부팅되고 EC2 사용자 데이터 스크립트가 실행됩니다.
- 운영체제 부팅 → 애플리케이션 실행 → 캐시 구성 과정이 끝날 때 까지 시간이 오래 걸립니다.
- 이러한 단점을 보완하기 위해 EC2 Hibernate가 존재합니다.
- EC2 Hibernate
    - RAM에 있던 인메모리 상태는 그대로 보존됩니다.
    - 인스턴스의 부팅이 빨라집니다. (OS 재시작 / 중지 X)
    - 내부 동작: RAM에 기록되었던 인메모리 상태는 루트 경로의 EBS 볼륨에 암호화되어기록됩니다.
    - 지원 타입은 별개로 존재하며, RAM Size는 150GB 를 지원합니다. 절전 모드는 최대 60일까지 사용할 수 있습니다.
