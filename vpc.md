### VPC

**CIDR (Classless Inter-Domain Routing)**

- CIDR은 IP 주소를 할당을 위한 방법으로 사용됩니다.
- 보안그룹 규칙 및 AWS 네트워킹에서 일반적으로 사용됩니다.
- IP 주소 범위를 정하는데 도움을 줍니다.
    - WW.XX.YY.ZZ**/32** → 하나의 IP
    - 0.0.0.0**/0** → 모든 IP
    - 192.168.0.0**/26** → 192.168.0.0 - 192.168.0.63 (64 개의 IP 주소)
- 구성요소
    - **Base IP (기본 IP):** 범위 내에 포함된 IP를 나타냅니다.
    - **Subnet Mask (서브넷 마스크):** IP에서 변경 가능한 비트의 개수 정의

### **Default VPC Walkthrough**

- 모든 새로운 AWS 계정은 모두 기본 VPC를 가지고 있습니다.
- 서브넷이 지정되지 않은 경우 새로운 EC2는 기본 VPC에서 실행합니다.
- 기본 VPC는 인터넷 연결이 가능하며, 그 안에 있는 모든 EC2 인스턴스는 공개 IPv4 주소를 가지고 있습니다.
- 또한 EC2 인스턴스를 위한 퍼블릭 및 프라이빗 IPv4 DNS 이름도 가지고 있습니다.

### VPC in AWS – IPv4

- **VPC = Virtual Private Cloud**
    - 퍼블릭 클라우드 상에서 제공되는 고객 전용 사설 네트워크 공간
- 단일 AWS 리전에 여러 개의 VPC를 생성할 수 있습니다. (리전당 최대 5개 - 소프트 리밋)
- VPC다 최대 CIDR은 5개이며, 각 CIDR에 대해서는:
    - **최소 크기는 /28 (16개의 IP 주소)**
    - **최대 크기는 /16 (65,536개의 IP 주소)**
- VPC는 사설 리소스이기 때문에 프라이빗 IPv4 주소 범위만 허용됩니다.
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
- **VPC CIDR은 다른 VPC나 네트워크, 혹은 기업 네트워크와 겹치지 않도록 주의해야합니다.**

### 기본 상태

<img width="453" height="258" alt="Image" src="https://github.com/user-attachments/assets/ee4019a4-4903-4764-b0d2-ee61ff1030e7" />

### Subnet (IPv4)

- AWS는 각 서브넷마다 **5개의 IP 주소 (처음 4개와 마지막 1개)**를 예약합니다.
- 이 5개의 IP 주소는 사용할 수 없으며 EC2 인스턴스에 IP로 할당할 수 없습니다.
- ex. CIDR 블록이 10.0.0.0/24인 경우, 예약된 IP 주소는 다음과 같습니다.
    - **10.0.0.0** - 네트워크 주소
    - **10.0.0.1** - VPC 라우터 용으로 AWS가 예약한 주소
    - **10.0.0.2** - Amazon 제공 DNS에 매핑하기 위해 AWS가 예약한 주소
    - **10.0.0.3** - 미래 사용을 위해 AWS가 예약한 주소
    - **10.0.0.255** - 네트워크 브로드캐스트 주소.
- **시험 팁**: EC2 인스턴스에 29개의 IP 주소가 필요한 경우,
    - /27 크기의 서브넷을 선택할 수 없습니다. (32개의 IP 주소, 32 - 5 = 27 < 29)
    - /26 크기의 서브넷을 선택해야 합니다. (64개의 IP 주소, 64 - 5 = 59 > 29)

### Subnet 추가

<img width="451" height="257" alt="Image" src="https://github.com/user-attachments/assets/33a95973-57e3-48a6-9cee-4ab6e15efad1" />

### **Internet Gateway (IGW)**

<img width="445" height="224" alt="Image" src="https://github.com/user-attachments/assets/b80f93ec-e531-4f91-88a2-2bf6f18076fc" />

- VPC 내의 리소스 (ex. EC2 인스턴스)를 인터넷에 연결하도록 허용하는 EC2 인스턴스나 람다 함수
- 수평으로 확장 가능하며 가용성과 중복성이 높습니다.
- VPC와는 별도로 생성해야 합니다.
- 하나의 VPC에는 하나의 인터넷 Gateway만 연결할 수 있으며, 그 반대의 경우에도 마찬가지입니다.
- ex) 공용 서브넷에 공용 EC2 Instance를 만들고, 라우팅 테이블을 수정하여 ec2 인스턴스를 라우터에 연결한 후, 인터넷 Gateway에 연결합니다.

### **Bastion Hosts**

<img width="438" height="580" alt="Image" src="https://github.com/user-attachments/assets/ae6f1669-ce59-4958-820f-5df8233da9cf" />

- 배스천 호스트는 프라이빗 EC2 인스턴스에 SSH로 액세스하기 위해 사용할 수 있습니다.
- 배스천 호스트는 퍼블릭 서브넷에 위치하며, 다른 모든 프라이빗 서브넷에 연결됩니다.
- **배스천 호스트의 보안 그룹**은 인터넷에서 제한된 CIDR(예: 회사의 **공용 CIDR**)로부터 포트 22로의 인바운드 연결을 허용해야 합니다.
    - **배스천 호스트의 보안 그룹은 반드시 인터넷 액세스를 허용해야 합니다.**

### **NAT Gateway**

<img width="446" height="233" alt="Image" src="https://github.com/user-attachments/assets/b15e0dfe-a569-4575-882e-d3d40ff41d33" />

- 프라이빗 인스턴스와 서브넷이 있고, 인터넷에 액세스할 수 없는 경우 NAT Gateway를 Public subnet에 배포합니다.
- 그러면 인터넷 연결이 가능해집니다.
- 특정 AZ에서 가용성이 높습니다. AZ 전체에서 고가용성을 필요로 한다면, 다른 AZ에 NAT Gateway를 별도로 만들어야 합니다.

### **Security Group vs. NACLs**

<img width="454" height="198" alt="Image" src="https://github.com/user-attachments/assets/e08e17d6-ad22-442f-8519-f32e6c9db94d" />

### **VPC Peering**

<img width="444" height="451" alt="Image" src="https://github.com/user-attachments/assets/60d4584e-b837-42a2-a438-90fe5bce03c5" />

- AWS의 네트워크를 사용하여 두 개의 VPC를 프라이빗하게 연결합니다.
- 두 VPC가 동일한 네트워크에 있는 것처럼 동작할 수 있습니다.
- VPC 피어링은 두 VPC 간에 발생하며 **전이(transitive)되지 않습니다**. (서로 통신해야하는 각 VPC에 대해 별도의 피어링 연결을 활성화해야 합니다)

### VPC Endpoints

<img width="455" height="252" alt="Image" src="https://github.com/user-attachments/assets/1ff63224-7f9c-4ffb-b8f4-ac05861a3676" />

- NAT Gateway의 경우 Public internet을 거쳐서 AWS 서비스에 접근할 수 있습니다.
- VPC Endpoints는 Public internet을 거치지 않고, AWS 서비스에 직접 접근할 수 있습니다.
    - ex) AWS PrivateLink
- 종류
- 인터페이스 엔드포인트
    - ENI (VPC의 프라이빗 엔드포인트)를 AWS의 엔트리 포인트로 프로비저닝합니다. (보안 그룹을 연결 필수)
    - 대부분의 AWS 서비스를 지원합니다.
    - 시간당 + 처리된 데이터의 GB당 비용이 발생합니다.
- **Gateway 엔드포인트**
    - 게이트웨이를 프로비저닝하며 **이 게이트웨이는 반드시 라우팅 테이블의 대상이 되어야 합니다. (보안 그룹을 사용 X)**
    - 게이트웨이 엔드포인트 대상으로는 **S3 및 DynamoDB**가 있습니다.
        - s3의 경우에는 게이트웨이 엔드포인트를 선택하는 것이 유리합니다. (라우팅 테이블만 수정하면 되고 무료이기 때문입니다.)

### **VPC Flow Logs**

- 인터페이스로 들어오는 IP 트래픽에 대한 정보를 포착하는 기능
    - VPC 플로우 로그
    - 서브넷 플로우 로그
    - Elastic Network Interface (ENI) 플로우 로그
- 흐름 로그는 연결 문제를 모니터링하고 트러블 슈팅하는데 유용합니다.
- 흐름 로그 데이터는 S3, CloudWatch Logs로 전송할 수 있습니다.

### **NAT Gateway vs Gateway VPC Endpoint**

- Region에 VPC가 있고, 프라이빗 서브넷 두개는 각기 다른 EC2를 사용한다고 가정하고, S3 버킷을 통해 데이터를 액세스할 경우

<img width="449" height="189" alt="Image" src="https://github.com/user-attachments/assets/b7017f7b-5406-42a1-9c54-6562c0cc12e7" />

- 공용 인터넷 사용 시 (NAT Gateway)
    - NAT Gateway가 있는 퍼블릭 서브넷 생성, 이때 인터넷 게이트웨이로 향하는 라우터가 필요합니다.
    - 라우팅 테이블을 활용하여 라우터를 구축합니다.
    - EC2 인스턴스에서 NAT Gateway와 인터넷 게이트웨이를 지나 인터넷과 직접 연결됩니다.
    - S3 리전간 데이터 송신 비용 : $0.09 / h
- VPC Endpoint 사용
    - S3 버킷에 비공개로 데이터를 액세스할 수 있고, 다른 라우팅 테이블을 생성합니다.
    - VPC 엔드포인트에 액세스하기 위해서는 여기로 연결되는 라우트만 설정하면 됩니다.
    - 데이터는 사설 서브넷에서 VPC 엔드포인트와 S3 버킷으로 직접 흐르게 됩니다.
    - 게이트웨이 엔드포인트: 비용 X
    - 동일 리전에서 S3로 데이터 주고 받을 때: $0.01 / GB
- 비용적인 측면에서는 VPC Endpoints가 저렴합니다.

### **AWS Site-to-Site VPN**

<img width="453" height="194" alt="Image" src="https://github.com/user-attachments/assets/1d3b91be-d842-4b12-93e0-67aac850b058" />

- VPC는 구축했으나 특정 구조가 있는 기업 데이터 센터를 AWS와 비공개로 연결하려면 기업은 고객 게이트웨이를, VPC는 VPN 게이트웨이를 갖춰야 합니다.
- 퍼블릭 인터넷을 통해 프라이빗 Site-to-Site VPN을 연결해야 합니다.
- 퍼블릿 인터넷을 거치긴 하지만 VPN 연결이기 때문에 암호화되어 있습니다.

### **Direct Connect (DX)**

<img width="447" height="209" alt="Image" src="https://github.com/user-attachments/assets/336606fb-7209-4087-99ad-4fd3ee3d3057" />

- 원격 네트워크와 VPC간의 전용 프라이빗 연결을 제공합니다.
- Direct Connect를 사용할 때는 전용 연결을 생성해야 하고, AWS Direct Connect 로케이션을 사용합니다.
- VPC에 가상 프라이빗 게이트웨이(VPW)를 설정해야 합니다.
- **AWS Direct Connect를 사용하면 동일한 연결을 통해 퍼블릭 리소스(ex. S3)와 프라이빗 리소스(ex. EC2)에 접근이 가능합니다.**
- 사용 사례:
    - 대역폭 처리량이 증가할 때 (대용량 데이터 세트 작업 시) 비용 절감 가능
        - 퍼블릭 인터넷을 거치지 않기 떄문입니다.
    - 실시간 데이터 피드를 사용하는 애플리케이션에서 더 일관된 네트워크를 경험할 수 있게 합니다.
    - 하이브리드 환경 지원 (온프레미스 + 클라우드)

### **Transit Gateway**


<img width="449" height="528" alt="Image" src="https://github.com/user-attachments/assets/2a639594-89e6-44dc-9f4f-50cbf585373f" />

- **수천 개의 VPC와 온프레미스 간에 전이적 연결을 제공하기 위한 허브 앤 스포크 (스타) 연결**