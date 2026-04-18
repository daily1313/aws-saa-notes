1. 적은노력 → 서버리스
2. TCP/UDP → NLB
3. DDOS → WAF
4. 실시간 데이터처리 → Kinesis
5. 확장성 → Auto Scaling
6. 비용효율성 → S3
7. 분리, 비동기 → SQS
8. NLP → Amazon Comprehend
9. 로그 누가?붙으면 → CloudTrail
10. 수백 명의 사용자, 모바일사용자, SAML을 통한 인증 → Cognito
11. RDS와 Aurora의 통합이나 암호화(비밀) → Secrets Manager
12. IP 멀티 캐스트 → Transit Gateway
13. smb → S3 File Gateway, 파일서버 시스템용
14. 퍼블릭 인터넷을 거치지 않고도 인스턴스 → VPN Endpoint
15. 민감한 정보 → macie
16. SSL/TLS 생성관리배포 → Certificate Manager (ACM)
17. 컨테이너 → ECS, fargate, EKS
18. 향상된 보안 → CloudFront
19. 배치 동적 → 스팟인스턴스
20. 운영 최소화 → Auto Scaling X
21. 언제든지 중단,종료 → 스팟 인스턴스
22. 가까운 엣지 로케이션, 고성능 → Global Accelerator
23. 운영 오버헤드 최소화 → ECS, Fargate, ECR 및 EKS + Lambda
24. 데이터 분석을 한곳으로 모은다 → lake Formation
25. 전세계 사용자 → S3 + CloudFrount 원본으로
26. 시간초과 최소화 → RDS 프록시
27. 동적호스팅 → API Gateway + lamdba
28. .csv → Glue
29. 여러서버에서 동시에 액세스 → EFS
30. 보안평가 → Inspector
31. SQL 주입 → WAF
32. 말도안되는 대규모 → Lambda@Edge + Cloud Front
33. NAT Gateway → 무조건 퍼블릭서브넷에 존재
34. 여러 VPC 연결 → Transit Gateway
35. 수동승인 → Step Function
36. Saas → App Flow