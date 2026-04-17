## Amazon CloudFront

- Content Delivery Network (CDN) 콘텐츠 전송 네트워크
- 콘텐츠를 서로 다른 엣지 로케이션에 미리 캐싱하여 읽기 성능을 향상시키는 것
- 원본 서버 부하 감소 및 저지연 속도를 제공

#### **CloudFront - Origins**

**S3 bucket**

- 파일을 분산하고 엣지에 캐싱하는데 사용됩니다.
- Origin Access Control (OAC)를 통한 강화된 보안
- OAC는 Origin Access Identity(OAI)를 대체합니다.
- CloudFront를 통해 버킷에 데이터를 보내는 방법 (Ingress)으로 사용될 수 있습니다.

#### **CloudFront at a high level**

<img width="446" height="198" alt="Image" src="https://github.com/user-attachments/assets/66b0ac06-0da4-400b-ba0a-e77ac595e95c" />

1. 전세계에 퍼져있는 CloudFront의 엣지가 있고, 여기에 연결할 원본 (S3 혹은 HTTP 서버)가 있습니다.
2. 클라이언트가 연결해서 엣지 로케이션에 HTTP 요청을 보내면 엣지는 이것이 캐싱되어 있는지 확인합니다.
3. 캐싱되어 있지 않는 경우에는 원본으로 가서 요청 결과를 가져오고, 이를 로컬 캐시에 저장해서 다른 클라이언트가 같은 콘텐츠를 같은 엣지에 요청할 시 사용합니다.

+**CloudFront**와 **엣지 로케이션**을 이용해 특정 리전에 속한 S3 버킷을 전세계의 엣지 로케이션으로 분산시킬 수 있습니다.

#### **CloudFront vs S3 Cross Region Replication**

- CloudFront
    - Global Edge network, TTL 기간동안 파일 캐싱, 전세계적으로 사용하는 정적 콘텐츠에 적합합니다.
- S3 Cross Region Replication (S3 교차 리전 복제)
    - 원하는 리전 선택, 파일은 실시간으로 갱신, 읽기 전용, 일부 리전을 대상으로 동적 컨텐츠를 낮은 지연시간으로 제공합니다.

#### **CloudFront – ALB or EC2 as an origin**

**ALB**

<img width="443" height="107" alt="Image" src="https://github.com/user-attachments/assets/34d4273c-10d7-4189-a7b4-74dc45a289c6" />

- ALB: Public, EC2: Public or Private
- EC2의 보안그룹이 로드밸런서를 허용하게 설정하면 됩니다.
- 사용자는 Edge Location으로 접근하기에 애플리케이션의 공용 IP가 로드밸런서의 보안 그룹 정책에 허용이 되어 있어야 합니다.

**EC2**


<img width="442" height="101" alt="Image" src="https://github.com/user-attachments/assets/8e73152b-ed16-45b6-acd2-e2bf070a6700" />

- 사용자들이 CloudFront를 통해 접근하길 원합니다.
- 이 둘은 모두 Public으로 설정되어 있어야 합니다.
- Edge Location의 모든 공용 IP가 EC2에 접근할 수 있도록 보안그룹을 설정해줘야 합니다.

#### **Cache Invalidations**

- 백엔드 오리진을 업데이트할 때, CloudFront는 이 업데이트 사항을 알지 못하며, TTL이 만료된 후에서야 백엔드 오리진으로부터 업데이트된 콘텐츠를 받게 됩니다.
- CloudFront invalidation을 통해 캐시를 강제 새로고침하여 TTL을 모두 제거할 수 있습니다.

#### Unicast IP vs Anycast IP

- **유니캐스트 IP**: 하나의 서버가 하나의 IP 주소를 보유합니다.
- **애니캐스트 IP:** 모든 서버가 동일한 IP 주소를 가지며 클라이언트는 가장 가까운 서버로 라우팅됩니다.

#### AWS Global Accelerator

<img width="435" height="222" alt="Image" src="https://github.com/user-attachments/assets/e782c0eb-cf43-4d07-85b5-bbbb58b51e6a" />

- 전세계 사용자가 액세스하는 애플리케이션을 배포하였는데, 사용자들의 트래픽이 몰리게 되면 수많은 latency가 발생할 수 있습니다.
- 이러한 문제점을 개선하기 위한 AWS Network가 Global Accelerator입니다.
- 애플리케이션을 라우팅하기 위해 AWS 내부 글로벌 네트워크를 활용하며 2개의 애니캐스트 IP가 생성됩니다.
- ex) 가장 짧은 지연 시간과 신속한 리전 장애 극복을 위한 지능형 라우팅, Health Checks, 재해복구
- 보안
    - 단 2개의 외부 IP만 화이트리스트에 등록하면 됩니다.
    - AWS Shield를 통한 Ddos 보호 기능이 존재합니다.

#### **AWS Global Accelerator vs CloudFront**

**공통점**

- Global Accelerator와 CloudFront는 둘 다 글로벌 네트워크를 사용하고 AWS가 생성한 전세계의 엣지 로케이션을 사용합니다.
- 둘 다 AWS Shield와 통합하여 DDos 보호를 제공합니다

**CloudFront**

- 캐시 가능한 콘텐츠(이미지 및 동영상 등)의 성능을 개선합니다.
- API 가속 및 동적 사이트 전달과 같은 동적 콘텐츠에도 적용됩니다.
- 콘텐츠는 엣지에서 제공됩니다.

**Global Accelerator**

- TCP 또는 UDP를 통해 다양한 애플리케이션의 성능을 개선합니다.
- 패킷은 엣지 로케이션으로부터 하나 이상의 AWS 리전에서 실행되는 애플리케이션으로 프록시됩니다. 이 경우 모든 요청이 애플리케이션 쪽으로 전달되며 캐싱은 불가능합니다.
- 사례: UDP 기반의 게임, MQTT를 사용하는 IoT, 음성 통화
- 글로벌하게 고정 IP 주소가 필요한 HTTP 사용 사례에 적합합니다.
- 결정적이고 신속한 리전 장애 조치가 필요한 HTTP 사용 사례에 적합니다.
