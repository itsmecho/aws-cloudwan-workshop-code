# AWS Cloud WAN 워크샵 소개

이번 AWS Cloud WAN 워크샵에서는 중앙 대쉬 보드를 통해 네트워크 정책을 사용하여 한 위치에서 글로벌 네트워크에 대한 관리 및 보안 작업을 자동화하는 것을 실습하게 됩니다.

### 아키텍쳐&#x20;

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### 네트워크 세그멘테이션&#x20;

이번 워크샵에서는 아래와 같이 각 업무별로 상호 연결이 한되는 것을 실습하게됩니다. AWS Cloud WAN에서는 Core Network 내의 트래픽을 세그먼트로 분리할 수 있으며, 각각의 세그먼트 별로 라우팅 정책을 부여하여 접근 정책을 더욱 세분화할 수 있습니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/18.png" alt=""><figcaption></figcaption></figure>

### IP 주소&#x20;

#### 리전별 VPC CIDR 주소 대역&#x20;

```
us-east-1: 
  prod:    10.11.0.0/16
  finance: 10.12.0.0/16
  hr:      10.13.0.0/16
  shared:  10.14.0.0/16
eu-central-1: 
  prod:    10.21.0.0/16
  finance: 10.22.0.0/16
  hr:      10.23.0.0/16
  shared:  10.24.0.0/16
ap-southeast-1: 
  prod:    10.31.0.0/16
  finance: 10.32.0.0/16
  hr:      10.33.0.0/16
  shared:  10.34.0.0/16
ap-northeast-1: 
  prod:    10.41.0.0/16
  finance: 10.42.0.0/16
  hr:      10.43.0.0/16
  shared:  10.44.0.0/16
```

#### OnPrem VPN CIDR 주소 대역

```
us-east-1: 172.16.0.0/16
eu-central-1: 172.17.0.0/16
```



### 세그멘테이션&#x20;

아래 그림은 이번 워크샵에서 VPC 및 온프레미스 VPN이 어떻게 각각의 세그멘테이션으로 연결(attach)되는 지를 보여줍니다. 이번 워크샵에서는 세그멘테이션은 Tag를 사용하여 VPC 또는 VPN 연결에 대한 세그멘테이션이 자동으로 이루어지도록 합니다. &#x20;

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/cloudwan_components.png" alt=""><figcaption></figcaption></figure>

