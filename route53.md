## Amazon Route 53

<img width="445" height="394" alt="Image" src="https://github.com/user-attachments/assets/59e5ba49-698a-4ad4-9c5c-8edc0c785c95" />

- Route 53은 고가용성이 높고, 확장 가능하며, 완전히 관리되는 권위 (Authoritative) 있는 DNS 서비스입니다.
    - Authoritative = 고객이 DNS 레코드를 업데이트할 수 있다는 뜻
- Route 53은 Domain Registrar (도메인 등록 대행자)로도 사용됩니다.
- 리소스의 상태를 확인할 수 있는 기능을 제공합니다.
- AWS에서 100% SLA 가용성을 제공하는 유일한 서비스
- 53은 전통적인 DNS 포트입니다.

### Records

- 레코드를 통해 특정 도메인으로 라우팅하는 방법을 정의합니다.
- 각 레코드는 다음과 같은 정보를 포함.
    - **Domain/subdomain Name**: ex) example.com
    - **Record Typoe**: ex) A 또는 AAAA
    - **Value**: ex) 123.456.789.123
    - **Routing Policy**: Route 53이 쿼리에 응답하는 방식
    - **TTL**: 레코드가 DNS 리졸버에서 캐싱되는 시간
- Record Types
    - **A**: 호스트 이름을 IPv4로 매핑합니다.
    - **AAAA**: 호스트 이름을 IPv6로 매핑합니다.
    - **CNAME**: 호스트 이름을 다른 호스트 이름에 매핑합니다.
        - 대상 호스트 이름은 A 또는 AAAA 레코드가 있는 도메인 이름
        - DNS 네임스페이스의 최상위 노드 (Zone Apex)에 대한 CNAME 레코드를 생성할 수 없습니다.
        - 예) example.com에 대한 CNAME 레코드는 생성할 수 없지만, www.example.com에 대한 CNAME 레코드는 생성할 수 있습니다.
    - **NS**: 호스팅 존의 네임 서버
        - 트래픽이 도메인으로 라우팅 되는 방식을 제어합니다.

### **Hosted Zones**

- Public Hosted Zone: 인터넷에서 트래픽을 라우팅하는 방법을 지정하는 레코드 (공개 도메인)
- Private Hosted Zone: 하나 이상의 VPC 내에서 트래픽을 라우팅하는 방법을 지정하는 레코드 (비공개 도메인)

### **Routing Polices**

- Route 53이 DNS 쿼리에 응답하는 방식을 정의합니다.
- 트래픽을 라우팅하는 로드밸런서의 라우팅과는 다릅니다.
- 라우팅 정책
    - Simple, Weighted, Latency based, Failover, Geolocation, IP-based, Multiple Value..

### **Health Checks**

- HTTP Health Checks는 **공용 리소스**에만 사용할 수 있습니다.
- Health Checks → 자동 DNS 장애 조치
    - 공용 엔드포인트 모니터링 (애플리케이션, 서버, 기타 AWS 리소스)
    - 다른 상태 확인을 모니터링하는 상태 확인 (계산된 상태 확인)
    - CloudWatch 경보의 상태를 모니터링하는 상태 확인
- 상태 확인은 각자의 메트릭을 사용하는데 CloudWatch의 지표에서도 확인 가능합니다.

### **Domain Registar vs. DNS Service**

- Domian Registrar를 통해 도메인 이름을 구입하거나 등록하며, 이를 위해 연간 요금을 지불합니다다. (ex. GoDaddy, Amazon Registrar Inc., ...)
- Domain Registrar는 DNS 레코드를 관리를 위한 DNS 서비스를 제공합니다.하지만 다른 DNS 서비스를 사용하여 DNS 레코드를 관리할 수 있습니다.
- + 3rd Party Registrar에서 도메인을 구입한 경우에도 Route 53을 DNS 서비스 제공자로 사용할 수 있습니다.
- 예) GoDaddy에서 도메인을 구입하고 Route 53을 사용하여 DNS 레코드를 관리할 경우