## EBS Volume

<img width="542" height="286" alt="Image" src="https://github.com/user-attachments/assets/cd434337-c8c6-4532-90e9-e42b024ae8d3" />

- **EBS (Elastic Block Store) 볼륨:** 인스턴스가 실행되는 동안 연결할 수 있는 **네트워크** 드라이브
- 인스턴스가 종료된 후에도 데이터를 지속적으로 유지할 수 있도록 도와주는 도구입니다.
- CCP 레벨에서 한 번에 하나의 인스턴스에만 마운트될 수 있습니다.
    - CCP 레벨: 하나의 EBS는 하나의 EC2 인스턴스에만 마운트가 가능합니다.
    - Associate 레벨: 일부 EBS 다중 연결이 가능합니다,
- Free tier: 매달 30GB의 EBS 스토리지를 범용 SSD 혹은 마그네틱 유형으로 제공

### EBS Volume

- network drive: 인스턴스와 통신하기 위해 네트워크를 사용하기 때문에 지연이 발생할 수 있습니다.
- locked to AZ(Availability Zone): us-east-1a의 EBS 볼륨은 us-east-1b에 연결할 수 없습니다. (다른 가용 영역 이동하기 위해 스냅샷이 필요합니다.)
- provisioned capacity: 할당 용량에 따라 청구되며 드라이브 용량을 증가시킬 수 있습니다.
- EBS가 종료될 때는 Root EBS Volume은 삭제되지만, 다른 EBS 볼륨은 삭제되지 않습니다.
    - AWS Console, AWS CLI에서 제어가 가능합니다.

### EBS Snapshots

- EBS Volume의 특정 시점에 대한 백업(스냅샷) 생성이 가능한데, 볼륨을 분리할 것을 권장합니다.
- AZ or Region을 거쳐 복사가 가능합니다.
- **EBS Snapshot Archive: 75%까지 저렴한 archieve tire로 스냅샷 이동 기능 (아카이브 복원시간: 24~72시간)**
- **Recycle Bin for EBS Snapshots: 실제로 삭제하는 경우를 대비해 이미 삭제된 스냅샷을 보관하고 필요시 복원도 가능합니다. (1일~1년까지 보존 가능)**

### AMI (Amazon Machine Image)

- AMI are a **customization** of an EC2 instance.
    - 소프트웨어, 구성, 운영 체제, 모니터링 등을 직접 추가할 수 있습니다.
    - 모든 소프트웨어가 미리 패키지되어 있으므로 **부팅 및 구성 시간이 더욱 빠릅니다**.
- 종류
    - A Public AMI: AWS에서 제공하는 AMI
    - Your own AMI: 직접 만들고 유지 관리하는 AMI
    - AWS Marketplace AMI: 다른 사람이 구축한 AMI (혹은 판매하는 AMI)

### **ECS Instance Store**

- EC2 Instance Store: 고성능 하드웨어 디스크
- EBS Volume의 한계를 커버
- 특징
    - i/o 성능 향상
    - 인스턴스 중지 및 최대 절전 모드 전환 및 종료 시 스토리지 블록이 리셋됩니다.
    - 버퍼/캐시/스크래치 데이터/일시적인 콘텐츠에만 적합합니다.
    - 데이터 손실 위험을 감수해야 합니다.

### EBS Volume Type

- **gp2 / gp3 (SSD)**: 범용 SSD 볼륨. 다양한 작업에 대해 가격과 성능을 균형있게 유지합니다.
- **io1 / io2 (SSD)**: 최고 성능의 SSD 볼륨. 미션 크리티컬한 저지연 또는 고처리량 작업에 사용합니다.
- **st1 (HDD)**: 저비용 HDD 볼륨. 자주 액세스되는 처리량 중심 작업을 위해 설계됩니다.
- **sc1 (HDD)**: 최저 비용 HDD 볼륨. 접근 빈도가 낮은 워크로드를 위해 설계됩니다.

### EBS Multi-Attach - io1/io2

- 동일한 EBS 볼륨을 **동일한 가용 영역 내**의 여러 EC2 인스턴스에 연결
- 각 인스턴스는 고성능 볼륨에 대한 읽기 및 쓰기 권한을 전부 가집니다.
- ex) 클러스터링된 Linux 응용 프로그램(Teradata), 애플리케이션의 동시 쓰기 작업 관리
- 최대 16개의 ec2 인스턴스 동시 연결 기능을 제공합니다.

### EBS Encrytion

- 암호화된 EBS를 생성하면 암호화가 동시다발적으로 일어납니다.
    1. 저장 데이터가 볼륨 내부에 암호화.
    2. 인스턴스와 볼륨 간의 전송 데이터 또한 암호화.
    3. 모든 스냅샷 암호화.
    4. 스냅샷에서 생성된 볼륨 암호화.
- 암호화 및 복호화 매커니즘은 보이지 않게 처리되고, 암호화는 지연 시간에 영향을 거의 미치지 않습니다.
- KMS에서 암호화 키를 생성하여 AES-256 암호화 표준을 갖습니다.
- 암호화되지 않은 스냅샷 복사를 통해 암호화할 수 있습니다.


### Amazon EFS

<img width="636" height="490" alt="Image" src="https://github.com/user-attachments/assets/0d72269b-6029-4416-a1ad-76ac151ea000" />

- 여러 EC2 인스턴스에 마운트할 수 있는 관리형 NFS, 네트워크 파일 시스템
- 이 EC2 인스턴스들은 여러 가용 영역에 배치가 가능
- 고가용성 및 확장성이 뛰어나며, 비용이 비쌉니다. (gp2의 3배), 사용량에 따라 비용이 청구되므로 미리 용량을 프로비저닝하지 않아도 됩니다.
- ex) 콘텐츠 관리, 웹서핑, 데이터 공유
- Linux 기반 AMI와 호호환됩니다.
- KMS를 사용하여 EFS 드라이브에 저장 데이터 암호화를 활성화할 수 있습니다.

### EBS vs EFS

- EBS (Elastic Block Storage)
    - 한번에 하나의 인스턴스에만 연결이 가능하며, 특정 가용 영역에만 한정됩니다.
    - EBS를 다른 가용 영역으로 옮기려면,
        - 스냅샷 생성 → 다른 AZ에서 스냅샷 복원 → 해당 AZ에 EBS 볼륨 생성 과정을 거쳐야 합니다.
    - 인스턴스의 루트 EBS 볼륨은 EC2 인스턴스가 종료되면 기본적으로 종료됩니다. (이를 비활성화할 수 있음)
    - EBS는 실제 사용한 양이 아닌 EBS 드라이브 크기에 따른 사용량을 지불합니다.
- **EFS (Elastic File System)**
    - 여러 가용 영역에서 수백 개의 인스턴스를 마운트할 수 있습니다.
    - ex) wordPress
    - 가격: EFS > EBS, 비용절감 모드: EFS-IA
    - Linux 인스턴스 전용 (POSIX)

### EBS vs EFS vs Instance Store

- EBS(**EC2의 하드디스크)**: 네트워크 볼륨을 한 번에 하나의 인스턴스에 연결할 수 있고 특정 AZ 내로 한정합니다.
- EFS(**여러 EC2가 쓰는 NAS)**: 다수의 인스턴스에 걸쳐 연결해야 하는 네트워크 파일 시스템에 적합합니다.
- Instance store(**날아가는 초고속 임시 디스크)**: EC2 인스턴스에 IO를 최대로 사용하게끔 해주지만, 인스턴스가 망가지면 함께 망가지는 임시 드라이브입니다.