## AWS Snow Family

- 보안성이 뛰어난 휴대 가능한 장치들로, 엣지에서 데이터를 수집하고 처리하거나 AWS 안팎으로 데이터를 마이그레이션할 수 있는 솔루션
- 데이터 마이그레이션: Snowcone, Snowball Edge, Snowmobile
- 엣지 컴퓨팅: Snowcone, Snowball Edge
- **AWS Snow Family:** 오프라인 장치를 사용하여 데이터 마이그레이션을 수행합니다. 네트워크를 통한 데이터 전송이 일주일이 넘게 걸린다면 Snowball 장치를 사용해야 합니다.

#### Snowball Edge (for data transfers)

- 물리적 데이터 이동 솔루션: TB 또는 PB 단위의 데이터를 AWS 안팎으로 전송합니다.
- 네트워크를 통한 데이터 이동 대안 (네트워크 비용 청구)
- 데이터 전송 작업 별로 비용 청구
- 블록 스토리지 및 Amazon S3 호환 객체 스토리지 제공
- **Snowball Edge Storage Optimized**: 블록 볼륨으로 사용할 수 있도록 80 TB HDD 용량을 제공하거나 S3 호환 객체 스토리지 제공
- **Snowball Edge Compute Optimized**: 블록 볼륨으로 사용할 수 있도록 42 TB HDD 또는 28TB NVMe 용량을 제공하거나 S3 호환 객체 스토리지 제공
- ex) 대용량 클라우드 마이그레이션, 데이터 센터 폐쇄, 재해 복구

#### 요약

<img width="459" height="203" alt="Image" src="https://github.com/user-attachments/assets/6d54b6df-6fd8-473d-b8f2-749383469038" />

#### Edge Computing

데이터가 **엣지 로케이션**에서 생성될 때 데이터를 처리하는 컴퓨팅 패러다임

- 엣지 로케이션은 인터넷이 없는 곳이나 클라우드에서 멀리 있는 곳 어디든 해당될 수 있습니다.
- 이러한 장소는 인터넷 접속이 제한적이거나 전혀 없을 수 있으며, 컴퓨팅 자원에 쉽게 액세스할 수 없는 경우가 많습니다.
- Snowball Edge, Snowcone과 같은 장치를 통해 엣지 컴퓨팅을 수행합니다.
- ex) 데이터 전처리, 클라우드로 보내지 않고 엣지에서 머신 러닝하는 경우

#### AWS OpsHub

- snow 장치 관리를 위한 소프트웨어
- 파일 전송, snow 장치에서 실행하는 인스턴스 관리, 장치 메트릭(저장 용량, 장치에서 활성화된 인스턴스 등) 모니터링
- AWS 호환 서비스를 실행할 수 있습니다. (EC2, DataSync, NFS)

#### **Solution Architecture: Snowball into Glacier**

- snowball을 통해 직접적으로 Glacier에 데이터를 가져올 수 없으며, s3의 수명 주기 정책을 통해 Glacier로 객체를 전환할 수 있습니다.

#### **Amazon FSx**

- **타사 고성능 파일 시스템을 실행시켜주는 서비스**
- AWS에서 제공하는 완전 관리형 서비스가령 RDS에서 AWS에 MySQL이나 Postgres를 실행하는 것과 같은 개념. RDS가 FSx로 바뀌었고, 파일 시스템을 실행한다는 점이 다릅니다.
- FSx for Lusture, FSx for Windows File Server, FSx for NetApp ONTAP, FSx for OpenZFS

**1.Amazon FSx for Windows (File Server)**

- 완전 관리형 Windows 파일 시스템 공유 드라이브
- SMB 프로토콜과 Windows NTFS를 지원합니다.
- Linux EC2에 마운트 할 수 있습니다.
- Microsoft의 분산 파일 시스템 (Distributed File System, DFS) 네임스페이스를 지원합니다.

**2.Amazon FSx for Lustre**

- Lustre는 원래 대규모 컴퓨팅을 위한 분산 파일 시스템으로 쓰였습니다.
- Lustre는 "Linux"와 "Cluster"를 합친 단어입니다.
- 머신러닝, HPC(High Performance Computing)에 활용됩니다.
- vpn 및 직접 연결을 통해 온프레미스 서버에서 사용이 가능합니다.

3.**Amazon FSx for NetApp ONTAP**

- AWS의 관리형 NetApp ONTAP 파일 시스템
- **NFS, SMB, iSCSI 프로토콜과 호환되는 파일 시스템**
- 스토리지는 자동으로 확장 및 축소됩니다 (오토 스케일링)
- **지정 시간 복제 기능을 통한 즉각적으로 복제합니다(새로운 워크로드 테스트에 유용)**

4.**Amazon FSx for OpenZFS**

- AWS의 관리형 OpenZFS 파일 시스템
- 여러 버전의 NFS 프로토콜과 호환 가능 (v3, v4 ..)
- ZFS에서 실행되는 워크로드를 내부적으로 AWS로 옮길 때 사용합니다.

#### **AWS Storage Gateway**

<img width="430" height="192" alt="Image" src="https://github.com/user-attachments/assets/75d01364-6a27-4bc4-8b4c-a2861885d761" />

- 온프레미스 데이터와 클라우드 데이터 간의 연결을 제공합니다.
- ex) 재해 복구, 백업 및 복원, 계층화된 스토리지, 온프레미스 캐시 및 파일 액세스 지연 시간 감소

#### **AWS Transfer Family**

<img width="445" height="209" alt="Image" src="https://github.com/user-attachments/assets/d6d64b91-572d-41eb-8cc4-ea94f88aac56" />

- AWS 전송 제품군: Amazon S3 또는 EFS 안팎으로의 파일(데이터) 전송을 위한 완전 관리형 서비스
- S3 APIs나 EFS 네트워크 파일 시스템을 사용하지 않고 FTP 프로토콜만 사용합니다.
- 지원하는 프로토콜
    - **AWS Transfer for FTP (File Transfer Protocol (FTP))**
    - **AWS Transfer for FTPS (File Transfer Protocol over SSL (FTPS))**
    - **AWS Transfer for SFTP (Secure File Transfer Protocol (SFTP))**
- 완전 관리되는 인프라, 확장성, 안정성, 고가용성
- ex) 파일 공유, 공개 데이터셋 공유 등

#### **AWS DataSync**

<img width="438" height="186" alt="Image" src="https://github.com/user-attachments/assets/96d65ef0-21c2-42e9-808f-fe5b02007700" />

- 대용량의 데이터를 한곳에서 다른 곳으로 옮길 때 사용합니다.
- 온프레미스 / 다른 클라우드에서 AWS로 데이터 이동 (NFS, SMB, HDFS, S3 API등) - **DataSync 에이전트 필요합니다.**
- s3, efs, fsx에 동기화를 지원합니다.

#### Summary

- **S3**: 객체 스토리지. 대부분의 AWS와 연결 가능을 제공합니다.
- **S3 Glacier**: 객체 아카이브 스토리지
- **EBS volumes**: 한 번에 한 개의 EC2 인스턴스에만 스토리지를 연결 할 때에는 EBS 볼륨을 사용합니다.
- **Instance Storage**: EC2 인스턴스에 직접 연결된 물리적 스토리지 (고 IOPS)
- **EFS**: Linux 인스턴스용 네트워크 파일 시스템, 다중 가용 영역 간 마운트 하며 POSIX 파일 시스템을 사용합니다.
- **FSx for Windows**: Windows 서버용 네트워크 파일 시스템으로, Windows와의 원활한 호환성과 통합 기능을 제공합니다.
- **FSx for Lustre**: 고성능 병렬 분산 파일 시스템으로, HPC에서 계산 집약적인 워크로드에 적합합니다.
- **FSx for NetApp ONTAP**: 관리형 NetApp ONTAP 파일 시스템으로, 다양한 운영 체제와의 높은 호환성을 제공합니다.
- **FSx for OpenZFS**: 관리형 ZFS 파일 시스템으로, Linux에 대한 원활한 호환성과 데이터 관리 기능을 제공합니다.
- **Storage Gateway**: 온프레미스 환경과 AWS 간의 연결을 제공하는 하이브리드 스토리지 서비스로, S3 및 FSx 파일 게이트웨이, 볼륨 게이트웨이 (캐시 및 저장), 테이프 게이트웨이를 지원합니다.
- **Transfer Family**: FTP, FTPS, SFTP 프로토콜을 사용하여 Amazon S3 또는 Amazon EFS 위에서 파일 전송을 제공하는 완전 관리형 서비스
- **DataSync**: 온프레미스 시스템과 AWS 또는 AWS 서비스 간의 예약 및 자동화된 데이터 전송을 지원하는 서비스
- **Snowcone / Snowball / Snowmobile**: 대량의 데이터를 클라우드로 물리적으로 안전하고 효율적으로 이동하기 위한 장치
- **Database**: 특정 워크로드에 대한 특화된 서비스로, 인덱스 및 쿼리 등의 기능을 제공합니다.