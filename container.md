## Docker Containers Management on AWS

- **Amazon Elastic Container Service (Amazon ECS)**: 도커 관리를 위한 Amazon의 전용 플랫폼
- **Amazon Elastic Kubernetes Service (Amazon EKS)**: 쿠버네티스의 관리형 버전 (오픈 소스)
- **AWS Fargate**: Amazon의 서버리스 컨테이너 플랫폼. ECS, EKS 둘 다 함께 작동할 수 있습니다.
- **Amazon ECR**: 컨테이너 이미지를 저장하는데 사용합니다.

### Amazon ECS

- ECS = Elastic Container Service
- ECS 클러스터에서 **ECS 태스크**를 실행하여 AWS 상에서 Docker 컨테이너를 실행할 수 있습니다.
- ECS Service Auto Scaling을 지원합니다. (ECS 작업 수를 자동으로 증가/감소)

#### EC2 LaunchType

<img width="448" height="429" alt="Image" src="https://github.com/user-attachments/assets/435e9ca2-64d6-4a83-b8b6-f5ec79ab11ec" />

- ECS Launch Type으로 ECS 클러스터를 사용할 때는 인프라 (EC2 인스턴스)를 직접 프로비저닝하고 관리해야 합니다.
- 각 EC2 인스턴스는 ECS 에이전트를 실행하여 ECS 클러스터에 등록해야하며, 컨테이너의 시작과 종료는 AWS가 관리합니다.

#### Fargate LaunchType

<img width="421" height="382" alt="Image" src="https://github.com/user-attachments/assets/5ca46993-f894-4bfc-9a5c-e11e14231592" />

- Docker 컨테이너를 AWS에서 실행하는 또 다른 옵션
- 인프라 (EC2 인스턴스)를 직접 프로비저닝하거나 관리할 필요가 없습니다.
- 서버리스, 확장하기 위해서는 태스크의 수를 늘리면 됩니다.

#### **Load Balancer Integrations**

<img width="450" height="408" alt="Image" src="https://github.com/user-attachments/assets/fedde3ec-3340-4f96-8ca1-6407cf64995d" />

1. ECS 클러스터 안에서 여러 ECS 태스크들이 실행되고 있습니다.
2. HTTP나 HTTPS 엔드포인트로 태스크를 활용하기 위해 Application Load Balancer (ALB)를 앞에서 실행하면 모든 사용자가 ALB 및 백엔드의 ECS 태스크에 직접 연결됩니다.

#### Data Volumes (EFS)

<img width="426" height="552" alt="Image" src="https://github.com/user-attachments/assets/53c18436-239b-4911-b7e3-4219369bfbe6" />

- EFS 파일 시스템을 ECS 태스크에 마운트해서 데이터를 공유하며 launch 타입에 관계없이 모두 호환됩니다.
- Fargate + EFS = Serverless (다중 az가 공유하는 컨테이너의 영구 스토리지)

#### Amazon ECR

<img width="430" height="767" alt="Image" src="https://github.com/user-attachments/assets/13c65593-bf6f-4952-97fe-1fdadc58a83d" />

- ECR = Elastic Container Registry
- AWS에 도커 이미지를 저장하고 관리하는데 사용됩니다.
- **프라이빗** 및 **퍼블릭** 저장소
- ECR은 Amazon ECS와 완전히 통합되어 있고 이미지는 백그라운드에서 Amazon S3에 저장됩니다.
- iam을 통해 액세스 제어

#### Amazon EKS

<img width="453" height="203" alt="Image" src="https://github.com/user-attachments/assets/02f64f1f-de60-46e7-8d02-7256d826cc4e" />

- Amazon EKS = Amazon Elastic Kubernetes Service
- AWS에서 관리되는 Kubernetes 클러스터를 실행할 수 있는 서비스
- Kubernetes는 (일반적으로 Docker로 구성된) 컨테이너화된 애플리케이션의 자동 배포, 확장 및 관리를 위한 **오픈 소스 시스템**
    - 컨테이너를 실행한다는 목적은 ECS와 비슷하지만 사용하는 API가 다릅니다.
- 실행 모드
    - EC2: 워커 노드의 배포, Fargate: 서버리스 컨테이너 배포
- ex) 회사가 이미 온프레미스나 다른 클라우드에서 Kubernetes를 사용하고 있으며, Kubernetes를 사용하여 AWS로 마이그레이션하려는 경우

#### AWS App Runner

- 웹 애플리케이션과 API를 대규모로 배포하기 쉽게 만드는 완전 관리형 서비스
- 인프라 경험이 필요하지 않으며, 소스 코드나 Docker 컨테이너 이미지로 시작합니다.
- 웹 앱을 자동으로 빌드하고 배포합니다.
- auto scaling, 고가용성, 로드 밸런싱, 암호화 기능을 제공합니다.
- ex) 신속한 프로덕션 배포