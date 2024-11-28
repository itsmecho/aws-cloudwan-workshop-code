# AWS Cloud WAN 소개

### 개요

AWS Cloud WAN은 클릭 몇 번으로 글로벌 네트워크를 구축하여 지점, 데이터 센터 및 Amazon Virtual Private Cloud(Amazon VPC)를 연결할 수 있는 중앙 대시보드를 제공합니다. 네트워크 정책을 사용하여 한 위치에서 네트워크 관리 및 보안 작업을 자동화할 수 있습니다. Cloud WAN은 온프레미스 및 AWS 네트워크에 대한 전체 보기를 생성하여 네트워크 상태, 보안 및 성능의 모니터링을 지원합니다.&#x20;

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

### 주요 기능&#x20;

* 확장 가능하고 탄력적인 글로벌 네트워크
* 다중 지역 VPC-VPC 연결
* 클라우드와 온프레미스 네트워 연결
* 네트워크 세그멘테이션 (망 분리)
* 네트워크 시각화 및 제공&#x20;

### 주요 구성 요소

* **AWS Network Manager** – 글로벌 네트워크를 중앙에서 관리하기 위한 AWS Management 콘솔 및 관련 API의 사용자 인터페이스입니다.
* **글로벌 네트워크** – AWS 글로벌 네트워크상에서 구현되는 사설 글로벌 네트워크 인프라스트럭처입니다.  글로벌 네트워크는 Transit Gateway와 Core Network를 모두 포함할 수 있으며, AWS Newtwork Manager를 통해 관리됩니다.
* **코어 네트워크** – AWS에서 관리하는 글로벌 네트워크에 속하는 단일 네트워크 입니다.
* **코어 네트워크 정책** – 코어 네트워크의 모든 요소(CNE 배치 리전, Attachement, 라우팅 및 세그먼테이션 정책 등)를 정의하는 문서(json)입니다. AWS network manager를 통해 글로벌 네트워크에 배포됩니다.
* **Attachment(연결)** – 코어 네트워크에 추가하려는 모든 연결 또는 리소스입니다. 지원되는 연결에는 VPC, VPN, SD-WAN, TGW(Transit Gateway) 등이 있습니다.
* **CNE(코어 네트워크 에지)** – 확장하고자하는 각 리전에 배치되는 가상의 에지 장비입니다. Transit Gateway와 유사한 기능을 제공하고 있지만, 동적 라우팅을 지원하고 CNE 간 자동으로 peering이 되는 것과 같이 좀 더 개선된 기능이 제공합니다. (단, 아직 Direct Connect에 대한 직접 연결은 지원하지 않기 때문에 TGW(Transit Gateway)를 통해 연결해야합니다.
* **네트워크 세그먼트** – 기본적으로 글로벌 네트워크 전체에서 일관되게 세그먼트 내에서만 통신을 허용하는 라우팅 도메인입니다. 완전히 독립적인 레이어 3 라우팅 도메인으로 제공되기 때문에 네트워크 정책에서 공유 관계를 생성하지 않는 한 각 세그먼트간 트래픽은 서로 격리되게 됩니다. (논리적인 망 분리 환경 제공)

### 비용

이 실습에서는 Cloud WAN 서비스, 첨부 파일 및 데이터 처리 요금에 대해 계정 소유자에게 비용이 발생합니다. 자세한 내용은 [https://aws.amazon.com/cloud-wan/pricing](https://aws.amazon.com/cloud-wan/pricing/) 참조 바랍니다.
