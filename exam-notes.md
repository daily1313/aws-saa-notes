1. S3 정적 웹사이트 + HTTPS = CloudFront + 인증서
2. 개발자 권한 관리 = IAM 역할 + 최소 권한
3. EC2 → S3 접근은 액세스 키 저장이 아니라 IAM Role 사용
4. 비용 태깅 분석 = 비용 할당 태그 활성화 + Cost Explorer 그룹화
5. S3 데이터 공유 시 내 비용 최소화 = Requester Pays
6. 루트 MFA 분실 대비 = 다중 MFA 등록
7. AWS Snowball Edge: 네트워크 연결이 제한적이거나 불안정한 환경을 위해 특별히 설계된 서비스
8. Amazon Macie: S3에 저장된 PII(개인 식별 정보) 를 탐지하기 위해 만들어진 서비스
9. Cluster Placement Group = 낮은 지연 시간과 높은 네트워크 처리량이 필요한 워크로드를 위해 설계된 기능 (단일 가용 영역)
10. EC2 인스턴스 CPU가 100%에 도달하고, 동시에 SQS 대기열 메시지가 쌓일 경우 = SQS의 ApproximateNumberOfMessages 지표
11. Workload Discovery on AWS = 여러 계정과 여러 리전에 걸쳐 있는 AWS 리소스를 자동으로 탐색
12. AWS STS(Security Token Service) = Amazon S3 같은 AWS 리소스에 대해 장기 자격 증명 없이 단기 접근 권한을 안전하게 제공
13. 파티션 키를 생성 타임스탬프로 바꾸면 레코드가 더 고르게 분산되어 샤드 간 부하 편중 감소
14. Amazon S3 File Gateway(AWS Storage Gateway) = Amazon S3 객체로 저장하는 서비스
15. 적절한 마이크로서비스로 라우팅 = AWS Load Balancer Controller
16. Amazon RDS 인스턴스의 가용성과 장애 복원력을 높이기 위한 권장 방식 = Multi-AZ 배포
17. DynamoDB Accelerator(DAX) = DynamoDB 전용으로 제공되는 완전관리형 인메모리 캐시
18. 다른 AWS 계정에 안전하게 데이터 접근 권한을 주는 방법 = 교차 계정 IAM 역할
19. 정확히 한 번만 처리, 데이터 손실 방지 = SQS FIFO Queue
20. 즉시 처리 + 짧은 실행 시간 = API Gateway + Lambda
21. Amazon RDS Proxy = 애플리케이션과 DB 사이의 연결을 효율적으로 관리
22. HTTPS, 사내 직원만 접근, S3 저장 = CloudFront + OAC + ACM + WAF ****
23. 개발 DB 인스턴스가 영업시간 동안만 일정에 따라 실행 = EventBridge 스케줄 + Lambda
24. EC2 Auto Scaling = 트래픽이 몰릴 때 인스턴스를 자동으로 늘리고, 유후 상태일 때는 인스턴스를 줄여 비용 절감
25. 성능 개선 = Read Replica, ElastiCache
26. S3 → Gateway VPC Endpoint, DynamoDB → Interface VPC Endpoint
27. DynamoDB = TTL, PITR 제공
28. Lake Formation = 열 수준 권한 제어
29. 팀별 비용 추적 = 각 리소스에 CostCenter 태그 지정, CostCenter 태그 활성화
30. Direct Connect Gateway와 Transit Gateway를 연결 =  온프레미스와 모든 VPC 간의 중앙집중형 연결이 가능
31. ElastiCache for Redis = 동일한 데이터를 RDS에서 반복 조회할 경우
32. DynamoDB의 프로비저닝 처리량을 소모하지 않을 경우 = DynamoDB 데이터를 S3로 내보내고, Redshift가 COPY 명령으로 S3에서 적재
33. Inventory 테이블 업데이트가 느려지는 것 = DynamoDB Streams의 배치 크기 최적화, 쓰기 용량(WCU) 증가
34. 트래픽 급증에 자동 대응, 주문 유실 방지, 고객 경험 저하 최소화 = ALB + Auto Scaling + SQS + Multi-AZ RDS
35. ALB → 퍼블릭 서브넷, EC2 인스턴스 → 프라이빗 서브넷, RDS 데이터베이스 → 프라이빗 서브넷
36. Amazon Route 53 Resolver = VPC 안의 EC2 애플리케이션이 온프레미스 Active Directory 도메인에 대한 DNS 질의
37. 전역 트래픽 최적화 → AWS Global Accelerator, TCP / UDP 지원 + 고성능 L4 로드밸런싱 → Network Load Balancer
38. EC2 + Fargate + 유연성 = 선불 없이 1년 컴퓨팅 절약 플랜
39. 원격 사무실에서 각 VPC로 Site-to-Site VPN을 각각 연결하면, 사무실은 두 VPC에 접근할 수 있고 VPC끼리는 서로 연결 X
40. 자체 암호화 키를관리하여 Amazon RDS의 데이터를 보호, 키 자동 교체 = AWS KMS, 자동 키 순환
41. 계약직 팀원들은 AWS 서비스에 대해 서로 다른 수준의 접근 권한 필요, 회사는 프로젝트가 완료되면 모든 계약직 팀의 권한을 취소할 경우 = AWS STS
42. 회사는 이벤트를 순서대로 전달하고 게시-구독 모델을 지원 = AWS SNS FIFO
43. 컴플라이언스 + 자동 수정 = AWS Config + SSM
44. 작은 데이터 + 짧은 실행 + 빠른 결과 = Lambda
45. CloudFront 인증서 ⇒ us-east-1, S3 보호 = OAC
46. 자격 증명이 안전하게 관리 = AWS Secrets Manager
47. 정적 웹 = S3+CloudFront, 웹 공격 방어 = WAF
48. 예측할 수 없는 트래픽, 높은 지연시간 = EC2 인스턴스 자동 확장, ALB
49. 여러 EC2가 같은 파일을 실시간 공유 = EFS
50. API Gateway + WAF = REST API, 그리고 WAF는 같은 리전
51. 액세스 키가 90일 이상 경과한 경우, 해당 키를 비활성화하고 삭제 = AWS Config, 키 제거 (EventBridge, Lambda)
52. DB Instance connection 관리 = AWS Proxy
53. S3 업로드 후 바로 후처리 = S3 Event + Lambda
54. 인프라 자동화는 CloudFormation, 변경 감사는 Config
55. 대용량 로그를 가끔만 분석 = S3 + Athena
56. 비밀번호 교체 자동화 = AWS Secrets Manager
57. 보안 위협 탐지(ex) 암호화폐 채굴) = GuardDuty, 자동 대응 = EventBridge + Lambda
58. ALB와 EC2 인스턴스 간에 종단 간 암호화 = ACM
59. 2분 이하의 복구 시간 목표(RTO)와 복구 지점 목표(RPO) = Multi-MZ
60. 멀티 계정 보안 + FSBP = Control Tower + Security Hub
61. HTTPS로 부족할 때 민감 데이터 보호 = Field-Level Encryption
62. 스냅샷 public 차단 = Block Public Access (Org 레벨)
63. Provisioned IOPS = 고성능 DB I/O, 낮은 지연시간, 일관된 처리량
64. S3 버킷 키 = KMS는 유지하고 비용만 줄이기 = 서버측 암호
65. 완전 관리형 컨테이너 + 파일 저장 = Fargate + EFS
66. Blue/Green = DB 무중단 배포 + 스키마 변경
67. 공용 IP 주소를 차단, 의심스러운 트래픽 차단 = WAF
68. 콜드 스타트 해결 + 비용 최적화 = 프로비저닝된 동시성 + Auto Scaling
69. 규정 준수 관리 솔루션 = Security Hub +  AWS Config
70. 온프레미스에서 AWS로 최고 성능 하이브리드 연결 = Direct Connect
71. 액티브-액티브 + 멀티 리전 쓰기 → DynamoDB Global Tables, 글로벌 읽기 + DR + 주 리전 쓰기 → Aurora Global Database
72. Region 제어 = RequestedRegion
73. T-SQL 유지 = Babelfish
74. DNS 응답 로그 = Route 53 쿼리 로깅
75. 코드 유지 + 운영 최소화 = App2Container + EKS + RDS
76. S3 Storage Class: 즉시 접근 = Glacier Instant, 장기 보관 = Deep Archive
77. 읽기 성능 문제 = Read Replica(읽기 복제본)로 분리
78. 고정 워크로드 3년 = EC2 Instance Savings Plans, 바뀌면 Computing Plans
79. 비동기 처리 → Lambda Event Invocation, API Key 필요 → REST API (API Gateway)
80. DX 최대 복원력은 전부 이중화
81. Amazon Kinesis Data Streams = 초당 최대 5,000개의 메시지를 처리
82. 위치 = Geolocation (지리적 위치 라우팅) / 속도 = Latency
83. S3 직접 접근 제한 = 사용자 → CloudFront → WAF 검사 → S3(OAC)
84. 버킷 하나, 접근 = S3 Access Point
85. Kubernetes 클러스터 자동 스케일러 = 클러스터 노드 수 관리
86. Amazon S3 Multi-Region Access Point: 단일 글로벌 DNS 제공, 사용자 위치 기반으로 가장 가까운 리전으로 자동 라우팅
87. Amazon FSx for Lustre: 고성능 병렬 파일 시스템, 초저지연, 고처리량
88. 대상 추적(Target Tracking) 스케일링 정책: 트래픽에 따라 자동 확장/축소
89. AWS Config: 리소스 구성의 변경 사항을 감지
90. AWS Config: 리소스 규정 준수, 볼륨 암호화 검사
91. 세션 데이터 = DynamoDB + TTL +  PITR (Point-In-Time Recovery, 복원력)
92. 전송 게이트웨이 생성(Transit Gateway) + VPN = VPC 많을 경우
93. 다중 AZ 배포 + Aurora = 높은 가용성 + 예측 불가능한 읽기 워크로드 수요 충족
94. DB 비밀번호 자동 변경 = Secrets Manager, RDS + Secrets Manager = 자동 로테이션
95. 변경 감지 → EventBridge → Lambda → SNS
96. Global Accelerator = 네트워크 속도 개선
97. NLB = 고정 IP + 온프레미스 연결
98. PrivateLink  = VPC 간 서비스
99.  Client → NLB (Public Subnet) → ECS Tasks (Private Subnet) → NAT Gateway (Public Subnet) → Internet
100. Lambda 함수 URL + IAM 인증 = HTTPS endpoint 제공
101. 온프레에서 여러 리전 TGW 연결 = DXGW + Transit VIF
102. 자동 확장 그룹 = 필요에 따라 애플리케이션 Amazon EC2 인스턴스를 추가 및
     제거하면서 병렬로 처리가 실행
103. AWS DataSync = 온프레미스 → AWS 파일 시스템 전송, NFS 마이그레이션 = EFS + DataSync
104. S3 보안 4종 조합 = KMS + SecureTransport + IAM + CloudTrail
105. Amazon Kinesis Data Streams = 대량 스트리밍 데이터 수집, 트래픽 폭주 해결, Lambda 함수 호출
106. Amazon API Gateway: REST API로 엔드포인트 제공, Amazon DynamoDB: API Gateway에서 직접 PutItem 호출
107. AWS Control Tower에서 정책 위반 시 CloudFormation 차단
108. EFS 리전 간 동기화 + 양방향 = DataSync 양방향 구성
109. Amazon FSx for Lustre = HPC/ML에 최적화된 파일 시스템 (ML 고속 I/O에 적합)
110. 고가용성 필요 없는 비프로덕션은 NAT 하나만 둬서 비용 절감
111. Kinesis 스트림 실시간 처리 및 이상 탐지 → Managed Service for Apache Flink
112. 소셜 그래프·추천 = Neptune, 그래프 변경 감지 = Neptune Streams.
113. DynamoDB 500 간헐적 오류 = 재시도 및 지수 백오프
114. S3 Gateway VPC Endpoint:  S3 Private 접근 → Gateway VPC Endpoint, 인터넷 우회 → Endpoint 사용
115. Amazon FSx for NetApp ONTAP (Multi-AZ): 멀티 AZ 공유 블록 = ONTAP(iSCSI)
116. 로그 분석 기본 공식 = S3 + Athena
117. 세밀한 성능 분석 = 자세한 모니터링 + CloudWatch
118. DynamoDB + S3 실시간 적재 = Streams + Lambda
119. 항상 켜져야 하는 것 = 예약 인스턴스, 중단 가능 / 배치 = 스팟 인스턴스
120. EKS 권한 분리 = IAM + OIDC
121. RTO/RPO 24 = 자동 스냅샷
122. Aurora Serverless = 사용자 증가 예상, 읽기 중심, 가끔 쓰기, RDS 자동 확장
123. ElastiCache(Redis OSS), 재해 복구(DR)를 위한 저지연 복제 및 장애 조치 기능 = 글로벌 데이터 저장소
124. AWS SSO + 온프레미스 AD(Active Directory) = IAM Identity Center
125. 트래픽 폭주 개선 = API + SQS + Lambda
126. Tag Policy + SCP = 태그 강제
127. Secrets Manager = 비밀번호, 민감정보, 운영 최소화
128. Comprehend = 감정분석
129. 예측적 확장 정책 = 작업이 시작되기 30분전 프로비저닝
130. Performance Insights = DB 부하, 쿼리, CPU 사용률 분석, 데이터베이스 사용 패턴 파악
131. 컨테이너 이미지 CVE 스캔 + 최소 변경 → ECR 기본 스캔
132. 멀티 계정 자동 생성 + 표준화 = Control Tower + Account Factory
133. Route 53 Resolver Outbound Endpoint → 온프레미스 DNS 서버
134. ECS(Fargate) + RDS + SQS: 지속적인 요청 처리, 순차 실행되는 프로세스, 다운타임 없이 확장, 서비스 간 독립성 (느슨한 결합)
135. 예상 비용이 지정된 예산을 초과할 때 알림을 받고 싶어할 경우 = Amazon SNS + Cost Explorer
136. 다양한 수준의 트래픽에 적응할 수 있는 확장 가능한 솔루션 = 온디맨드
137. S3 버킷 보안 = OAC (Origin Access Control) 사용, CloudFront를 통해서만 S3 접근(업로드 포함) 허용 가능
138. AWS WAF = 대용량 HTTP POST 요청 검사
139. stateless + 트래픽 증가 =  AMI → Launch Template → Auto Scaling Group
140. 트래픽 폭증을 처리하고, 접수된 순서대로 비동기적으로 주문을 처리 = SQS FIFO
141. S3 데이터 레이크로 수집 = Streams + Lambda
142. 리소스 사용량에 대한 대시보드 제공 = AWS QuickSight
143. 분산 그룹 배치 전략: 가용성 99.99%, 동일한 기본 하드웨어에서 두 노드를 실행 x
144. pgvector = 벡터 임베딩
145. RDS 할인 최적화 → Reserved Instances, EC2 할인 최적화 → Savings Plans (Upfront)
146. VPN RTT·패킷손실 최소운영비 모니터링 = CloudWatch Network Monitor
147. S3 스토리지 비용을 절감하고 자주 액세스하는 객체에 대한 즉각적인 가용성을 제공 = ****S3 Intelligent-Tiering
148. 리전별 사용자 역할 관리 = AWS IAM Identity Center + IAM
149. CloudFront 서명 URL = 특정 사용자에게 웹 콘텐츠 제공
150. Amazon Data Firehose + Lambda = S3 버킷에 데이터를 저장한 후 Apache Parquet 형식으로 변환
151. AWS Storage Gateway 파일 게이트웨이 = NFS 프로토콜을 사용하여 데이터에 액세스
152. SMB + NFS + 자동 계층화 + 비용 최적화 = FSx for ONTAP
153. 대용량 분석, ML, 컴퓨팅 집약 처리 가속 = EMR + Spark
154. DynamoDB Accelerator(DAX) = 아키텍처 변경 최소화, 성능 개선
155. Lambda → RDS 안전 연결 = VPC + Security Group 참조 허용
156. AWS Certificate Manager(ACM) = 인증서 생성 및 관리, 갱신 자동화
157. 주 서버가 여러 컴퓨팅 노드에 작업 조정 = Auto Scaling, 자동 크기 조정
158. DynamoDB = 대규모 + 빠른 조회 + 자동 확장 + 고가용성
159. S3 Object Lambda = 호출한 애플리케이션이 객체를 회수하기전에 데이터를 처리하고 변환
160. S3 Gateway endpoint = 인터넷을 통과하지 않고 Amazon S3에 액세스
161. ELB = 마이크로서비스 호스팅, Rest API 관리
162. 여러 Linux EC2가 같은 데이터 공유, 여러 EC2가 함께 쓰는 공유 파일 스토리지 = EFS
163. 프로비저닝된 IOPS SSD Amazon Elastic Block Store(Amazon EBS) 볼륨 = 애플리케이션에는 일관되고 지연 시간이 짧은 성능을 갖춘 내구성 있는 스토리지
164. 장기 보관 + 몇 분 내 데이터 검색, 장기 보관을 위한 비용 효율적인 백업 솔루션 = S3 Glacier Flexible Retrieval
165. AWS Schema Conversion Tool(AWS SCT), AWS Database Migration Service(AWS DMS) = 스키마 변경사항 포착 + 마이그레이션
166. AWS Fargate = 서버리스 컴퓨팅, EKS에서 노드 프로비저닝/관리 불필요, 인프라 관리 불필요
167. AWS Shield Advanced = DDoS 공격을 완화
168. DynamoDB에서 S3로 직접 Export 하려면 지정 시점 복구가 활성화 유무 확인
169. 장애 발생 시 복구되고 레코드 손실 없이 처리를 계속할 수 있도록 해야할 경우 = 지수 백오프를 통한 재처리
170. 캐시된 볼륨: 온프레미스에 자주 사용하는 데이터 캐시 저장
171. AWS Glue = 최소한의 운영 노력, 분석 사용자가 여러 소스의 데이터를 쉽게 검색, 준비, 이동, 통합할 수 있도록 하는 서버리스 데이터 통합 서비스
172. Global Accelerator = Anycast 고정 IP 기반 → 빠른 글로벌 failover
173. HTTP 헤더와 본문이 정확하고 응답이 기대에 부합하는지 확인하기 위해
     워크플로를 테스트할 경우: 상태머신 로그 ALL
174. AWS 다중리전 키: 자체적으로 Key 관리 X, 여러 Region에서 키 사용 및 키 액세스 제어
175. Amazon Athena DynamoDB + SQL = 성능 지표 계산
176. 사전 서명된 URL = 사용자와 공유
177. vpc endpoint = VPC 내의 Amazon EC2 인스턴스만 S3 버킷에 액세스
178. 여러 리전에서 S3 버킷에 대한 접근 권한 제공 = 다중 리전 키
179. 규정 준수 =  AWS Config 및 AWS Backup Audit Manager
180. S3를 VPC 내부에서만 사용 = S3 VPC Endpoint + 버킷 정책
181. 지연 시간 기반 라우팅 = 최상의 성능 제공 (dns 요청 → route53이 가장 낮은 네트워크 지연 시간으로 리전의 endpoint 응답)
182. EKS + AWS Fargate = 각 Pod가 전용 실행 환경에서 동작
183. 웜풀 = 예컨대, 인스턴스가 많은 양의 데이터를 디스크에 기록해야 하기 때문에 부팅 시간이 매우 길어지는 애플리케이션의 지연 시간을 줄일 수 있습니다.
184. Firehose COPY → Redshift 로드 (성능 개선)
185. EFS File System = 여러 인스턴스간 파일 공유
186. 마이크로서비스별 개별 IAM 역할 생성 이후 EC2 인스턴스와 연결된 프로필에 추가
187. vpc endpoints = 인터넷 없이 aws 서비스 접속
188. S3 규정 준수 모드 = 일정 기간 동안 삭제 또는 수정되지 않도록 보호
189. aws site-to-site vpn = on-premise, aws간 보안 연결
190. ACM = 저장 및 전송 중 암호화
191. EFA(Elastic Fabric Adapter = 네트워크에서 노드간 지연시간 최소화
192. 트랜짓 게이트웨이 = 여러 vpc 확장
193. 루트 사용자 보호 = IAM 사용자 추가, 루트 사용자에 다중 요소 인증 적용
194. IAM Access Analyer = 회사의 모든 리소스와 계정 분석
195. QLDB = 회계기록 유지관리
196. direct connect = 온프레미스에서 AWS로 연결 설정
197. EFS = NFS를 사용하여 파일 공유
198. X-ray = 추적
199. s3 데이터 덮어쓰기 방지 = s3 버전관리 활성화, s3 서버측 데이터 암호화
200. 권한부여 방지 = IAM + SCP
201. S3 Stroage Lens = 버전이 부여되지 않은 s3 버킷 식별
202. 테이프 백업 유지 + 초장기 저비용 보관(7년) = Tape Gateway + Glacier Deep Archive
203. JWT + 사용자 권한을 부여하는 API = REST API + Lambda Authorizer
204. AWS AppConfig = 애플리케이션 구성 저장 및 관리, AWS Secret Manager = 자격증명관리
205. WAF + ACL = ALB + ACL + ECS + Serverless
206. Amazon EMR = 독보적인 유연성과 확장성으로 분석 워크로드를 가속화하는 빅 데이터 처리 서비스
207. EMR 관리형 확장기능 = 클러스터의 크기를 자동으로 조절
208. 지연시간 라우팅 정책 = 웹사이트 로딩 시간을 최대한 최적화
209. AWS WAF = 레이어 7 공격 방어 (Ddos 나와도 무시할 것)
210. S3 지능형 계층화 = 동적 트래픽
211. DynamoDB On-demand - 오토 스케일링이 실행되므로 워크로드를 예측하기 어려울 때나 데이터베이스의 수요가 급증할 때 유용합니다.
212. 마이그레이션, 스키마 변경 = SCT +  DMS
213. 사용자 정의 엔드포인트 = 하나의 실시간 보고 쿼리를 Aurora Replicas 3개에 자동으로 분산
214. AWS STS (Security Token Service) = AWS 리소스에 대한 임시 액세스 권한을 부여하는 안전하고 확장 가능한 방법
215. IAM Identity Center = 중앙 집중식 ID 시스템을 사용
216. 프라이빗 EC2 SSH + 콘솔 접속 = EC2 Instance Connect Endpoint (443)
217. S3 Express One Zone = 여러 개의 컴퓨팅 노드와 10밀리초 단위의 데이터 접근 속도
218. AWS LakeFormation =  IAM 역할만 PII 데이터에 액세스할 수 있도록 데이터에 대한 세분화된
     액세스 제어
219. 사용자 정의 태그, 조직 관리 계정에서 선택된 태그 활성화
220. 키네시스 = 온라인 게임의 실시간 분석과 상호작용 , dynamoDB = 밀리초 응답
221. s3 standard 버킷 = 자동 확장 가능, 높은 내구성 제공, 사용자가 직접 업로드
222. textract → Comprehend
223. ElastiCache = 데이터베이스 구조변경 없이 성능 개선
224. aws 서비스 카탈로그 제품 = 고객이 셀프 서비스 목적으로 사용하는 솔루션
225. 회사 계정에 속한 s3 버킷 안전 액세스 = s3 게이트웨이 엔드포인트 + IAM 인스턴스 프로필 정책
226. WAF + ALB = 악의적인 인터넷 활동 및 공격으로부터 복원력을 갖추고, 새로운 일반적인 취약점 및 노출로부터 보호
227. snowball edge storage optimized = 180TB DB 마이그레이션
228. Amazon Aurora MySQL = 읽기, 쓰기 작업량 시기에 따라 변동, 감사 기록 7일 보존, RPO 5시간 이내
229. 읽기 성능 개선 = 읽기 복제본 생성 및 쿼리
230. aws control tower = 모든 계정에 보안 모범 사례를 적용하기
     위해 중앙 집중식 거버넌스 및 제어를 구현
231. global accelerator = api 응답 속도 개선
232. cloudwatch logs + firehose = 네트워크 인터페이스로 들어오고 나가는 트래픽 정보를 거의 실시간으로 수집
233. 수동 개입 최소화, 높은 가용성 = multi-az rds db 인스턴스, fargate + ecs
234. 연결 해제 모드 실행 시 높은 가용성 제공 = aws sonwball edge 기본 및 보조 사이트로 사용
235. ECR (Elastic Container Registry) = 컨테이너 이미지 저장
236. Lambda@Edge = 데이터 전송 비용 절감
237. sticky session: 트래픽이 많을 때 여러 대의 서버가 분산처리하여 서버의 로드율을 증가, 부하량, 속도 저하 등을 고려하여 분산 처리하여 해결해주는 서비스
238. WAF = 특**정 IP 주소에서 원치 않는 요청을 대량으로 받을 경우 해결책**
239. [AWS Outposts](https://aws.amazon.com/ko/outposts/)는 AWS 서비스, 인프라 및 운영 모델을 사실상 모든 데이터 센터, 코로케이션 공간 또는 온프레미스 시설로 옮길 수 있습니다.
240. Control Tower → 자동 계정 + Guardrails, RAM → 네트워크 구성요소를 중앙에서 관리
241. 여러 EC2가 파일 공유 = EFS, 한 EC2 고성능 디스크 = EBS, 오브젝트  저장= S3, 장기 보관 = Glacier
242. TCP, 자동확장 = 네트워크 로드 밸런서, 자동 크기 조정 그룹
243. vpc의 모든 네트워크 리소스가 애플리케이션에 액세스 = 전송 게이트웨이
244. kinesis data streams = 초당 5000개 메시지 처리
245. 게이트웨이 vpc 엔드포인트 = s3 버킷 액세스
246.  전송중 데이터 암호화 = ACM + AWS KMS
247. 인증된 사용자 처리 = 사용자가 인증될 때 미리 서명된 url 생성
248. athena = 보고서
249. 도면 업로드 시간 최소화, 페타바이트 규모의 데이터 저장 = s3 + cloudfront
250. smb 프로토콜 = **네트워크상에서 파일, 프린터, 포트 등 자원을 공유하는 클라이언트-서버 통신 프로토콜, 네트워크 상 존재하는 노드들 간에 자원을 공유할 수 있도록 설계된 프로토콜 (Amazon FSx for windows)**
251. 새 계정에 대한 루트 사용자 보호 = 루트 사용자 비밀번호 업데이트, MFA
252.  재시작 가능한 배치 작업 = Spot + Batch
253. AWS site-to-site VPN = VPC는 구축했으나 특정 구조가 있는 기업 데이터 센터를 AWS와 비공개로 연결
254. 애플리케이션 A가 애플리케이션 B로 트래픽을 보내는 것을 방지 = 애플리케이션 A 서브넷의 사용자 지정 네트워크 ACL에 아웃바운드 규칙을 추가
255. 단일 리전에서 운영 중인데, 그 리전을 못 쓰게 돼도 계속 동작 = 다중 리전 배포 + dns 상태 확인
256. aurora serverless  + s3 = 자동확장 + 자동백업
257. aws workload discovery = 워크로드에 대한 작성 및  아키텍처 다이어그램 자동 생성, 수집기능, 아키텍처 자동 시각화
258. Amazon Athena = S3 로그 분석
259. rds db에 접근이 필요한 IAM 역할 생성 → ec2 프로파일에 연결
260. efs = 모든 ec2 인스턴스간 데이터 공유
261. kinesis data streams → 원격 측정 데이터 수집, apache flink → 실시간 데이터 처리, cloudwatch → 사용자 지정 지표게시
262. apache kafka (MSK) = 다양한 스키마 / 형식, 실시간 스트리밍 처리 기능 / 데이터 변환
263. S3 증분 방식 = 데이터 장기 분석
264. 결제 메시지 처리 = kinesis + SQS FIFO
265. amazon ec2 온디맨드 - 중단 X, s3 standard - 자주 접근, s3 glacier deep archive - 장기 보관
266. S3 파일 게이트웨이 = nfs 인터페이스 제공, s3 쓰기
267. AWS Secret Manager = 사용자 이름, 비밀번호 변경, 자격 증명 관리와 관련된 운영 비용을 최소화
268. LiveAnalytics + SageMaker + QuickSight = **기계 학습과 보고를 위해 대규모로 차량의 원격 측정 데이터를 수집하고 분석**
269. Compute Optimizer = 최적화를 위한 ebs 볼륨 권장 사항 생성
270. AD Connector = 온프레미스 AD를 그대로 연결, ALB 인증과 연동 기능
271. SCT + DMS = 서로 다른 DB간 마이그레이션
272. WAF + WAS FireWall Manager + AWS Config = 퍼블릭 엔드포인트 보호
273. Cost Explorer = 보고서 정보 확인
274. CloudFront = 웹사이트 호스팅비용 최소화
275. blue/green = db 손실 없이 업그레이드, 테스트
276. 이미지 + 빠른 조회 = S3 + dynamoDB
277. cloudfront 캐시 무효화 = 웹사이트 업데이트 표시 솔루션
278. EMR 보안 구성 = 데이터 전송 및 저장 중 암호화, 클러스터 로컬 볼륨 암호화
279. AWS WAF = sql 주입 및 크로스 스크립팅 방어
280. NAT GATEWAY → s3 gateway endpoint (vpc 내부에서 s3 접근 가능)
281. 교차 리전 복제 = 데이터 백업을 위한 재해 복구 솔루션
282. storage gateway = 로컬 전체 데이터 접근 유지 + AWS 백업
283. GWLB = 보안 어플라이언스 + 모든 트래픽 검사
284. REST API + Lambda = 보안 어플라이언스 + 모든 트래픽 검사
285. S3 Intellijgent-Tiering = 검색에 비용 X
286. **Auto Scaling 환경 패치 = AMI를 업데이트해서 배포 (EC2 이미지 빌더 사용)**
287. SAML = 회사 네트워크의 Active Directory 사용자 그룹을 통해 온프레미스 리소스에 대한 액세스를 관리
288. AWS Transfer Family + Amazon S3 + AWS Lambda = 파일 전송 자동화
289. 퍼블릭 서브넷에 NAT 게이트웨이 생성, 프라이빗 서브넷 라우트 테이블에서 인터넷 대상 트래픽을 NAT 게이트웨이로 전달
290. direct connect + gateway (VIF) = Transit Gateway 연결
291. EFS 비용 절감 + 간헐적 접근 + 자동 확장 = IA + 자동 복귀 + Bursting
292. vpc endpoints = 공용 IP X
293. glacier flexible retrieval = 12년 동안 2.5TB 데이터 생성, 비용 효율적, 몇분 안에 데이터 검색
294. gp3 = 15,000 IOPS
295. datasync = 대용량 데이터 마이그레이션
296. route53 resolver 아웃바운드 엔드포인트 = vpc를 → 온프레미스 조회
297. resolver inbound endpoint = 온프레미스 → vpc 조회
298. NAT Gateway = 리전 전체 가용성 제공,  퍼블릭 서브넷
299. AWS IAM Identity Center = 영구 자격 증명 사용을 최소화, 최소권한원칙에 따라 접근권한 관리
300. Amazon Data Lifecycle Manager = 불필요한 스냅샷 비용 절감
301. AWS Config + Lambda = 키 사용 확인 및 제거
302. snowball edge 스토리지 최적화 장치 = 200TB 데이터 마이그레이션
303. 웜풀 = 인스턴스 부팅 시간 절감
304. 프로덕션 인스턴스 = Compute Saving Plans, 비프로덕션 인스턴스 = On-Demand Instance
305. 온프레미스 ↔ AWS 데이터 연결 (private VIF + Direct Connect, VPC peering)
306.  AWS 글로벌 액셀러레이터 = 최고 품질의 스트림 전송 기능
307. ALB + 가용영역 배포 = 고가용성 + 확장성
308. Amazon FSx for Lustre = 지속적인 높은 처리량 + 낮은 지연시간 제공 (고성능 파일 시스템)
309. EBS Elastic Volumes = 고객별 저장 공간 용량 할당
310. 사전 서명 URL = S3 버킷에 임시 액세스를 허용하여 시간을 쉽게 관리
311. s3 데이터 가끔씩 쿼리 = Athena
312. Aurora 글로벌 데이터베이스 = 재해 복구 솔루션 제공
313. Fargate + RDS + SQS +ECS = 독립 확장, 다운타임 없이 진화
314. step function’s Express Workflow = 처리량이 많고 지속시간이 짧으며 비용이 적게 드는 워크플로에 최적화
315. s3 Trigger = 동영상 데이터 즉시 처리
316. 웹사이트 고가용성 구축 = multi-az db instance, alb
317. 클러스터 배치 그룹 = 네트워크 처리량 극대화, 지연 시간 최소화
318. dns 호스팅 서비스를 신속히 마이그레이션 = route53 퍼블릭 호스팅 영역 생성
319. lambda’s serverless architecture = 독립적, 병렬적, 오버헤드 최소화