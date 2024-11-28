# \*Lab 5 - VPN attachment(연결)

### 1. on premise VPN 연결&#x20;

이번 단계는 **us-east-1** 및 **eu-central-1** 지역에서 온프레미스 고객 게이트웨이를 Site-to-Site VPN으로 연결하게됩니다. 고객사의 온프레미스 게이트웨이(CGW)와 Site-to-Site IPSec VPN 연결을 위해 오픈 소스기반의 strongSwan을 사용되며, 이와 동시에 BGP(Border Gateway Protocol) 연결을 위해 오픈 소스 기반의 Quagga가 함께 사용됩니다.

Cloudformation 스택은 2개의 EC2 인스턴스가 있는 1개의 VPC를 생성합니다. 하나의 인스턴스는 strongswan VPN 애플리케이션을 실행하고 다른 하나는 온프레미스 서버로 작동합니다.&#x20;

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/27.png" alt=""><figcaption></figcaption></figure>

### 2. CloudFormation 템플릿 배포&#x20;

다음 CloudFormation 템플릿을 다운로드하고 실행하여 시뮬레이션된 데이터 센터 환경을 배포합니다.

1. 워크샵에서 제공된 CloudFormation 템플릿 파일을 준비합니다.&#x20;
   * [ ] **파일 다운로드 -** [**VPN.YAML**](https://www.dropbox.com/scl/fi/2bs5g4jgqrqpv6lsv7jme/vpn.yaml?rlkey=uifk26chdw6xz6wr8dcpwc55w\&st=sozmuhpm\&dl=0)
2. [CloudFormation 콘솔](https://console.aws.amazon.com/cloudformation/home?#/stacks) 로 이동[ ](https://console.aws.amazon.com/cloudformation/home?#/stacks)
3. **us-east-1** 지역 에 있는지 확인
4.  **스택 생성** 을 클릭하고 **새 리소스 사용(표준)** 을 선택 합니다.\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/cft1.png" alt=""><figcaption></figcaption></figure>
5. **템플릿** 선택 준비 완료
6. **템플릿 파일 업로드**
7.  파일 선택 을 클릭 **합니다.**\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/cft2.png" alt=""><figcaption></figcaption></figure>
8. 방금 다운로드한 `vpn.yaml`파일을 사용하고 **다음** 을 클릭 합니다.
9. 스택에 이름을 지정하십시오(예: `cloudwan-vpn`.)
10. 매개변수를 검토합니다.
11. **스택 옵션 구성** 에서 기본값을 수락하고 다음 을 클릭합니다 **.**
12. 파라미터와 IAM 리소스 공지를 검토하십시오. **AWS CloudFormation이 IAM 리소스를 생성할 수 있음을 인정합니다를** 선택 합니다.
13. **스택 생성** 을 클릭합니다 .
14. 계속하기 전에 스택이 생성될 때까지 기다리십시오. CloudFormation 콘솔의 상태가 스택 생성 성공을 나타내는 CREATE\_COMPLETE로 표시되는지 확인합니다.
15. **eu-central-1** 리전 에서 위 단계를 반복합니다.



### 3. Site-to-Site VPN attachment&#x20;

이제 AWS 환경에 대한 사이트 사이트 VPN 연결을 통해 **us-east-1** 및 **eu-central-1 리전에서 시뮬레이션된 데이터 센터 환경을 시작했습니다.**

마지막 섹션의 배포는 strongswan VPN 어플라이언스와 AWS 환경 간에 분리된 IPsec VPN을 생성했습니다. 아래와 같이 생성된 VPN의 상태를 확인합니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/28.png" alt=""><figcaption></figcaption></figure>

이제 Site-to-Site VPN을 Cloud WAN 네트워크에 연결할 것입니다.

1. 네트워크 관리자 에서 코어 네트워크 섹션 아래의 attachment 를  클릭합니다.
2.  attachment 클릭\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/19.png" alt=""><figcaption></figcaption></figure>
3. attachment  섹션 에서 예를 들어 설명이 포함된 이름을 입력합니다. 예: `datacenter-vpn-us`
4. **Edge Location** 드롭다운 에서 **us-east-1** 을 선택합니다.
5.  attachment 드롭다운에서 **VPN** 선택\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/29.png" alt=""><figcaption></figcaption></figure>
6.  **VPN 연결** 섹션에서 배포된 사이트 사이트 VPN의 vpn-id를 선택합니다 .\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/30.png" alt=""><figcaption></figcaption></figure>
7.  태그 섹션에서 공유 세그먼트 정책 정의에 지정된 것과 일치하는 태그 키 및 값을 입력합니다. 이때 태그 키 및 값은 세그먼트 정책 정의에 지정된 것과 일치해야 합니다.\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/31.png" alt=""><figcaption></figcaption></figure>


8. Create Attachment 클릭
9. Tag가 정확히 매치된 경우에는 Attachment 상태가  수락 보류 로 표시됩니다.
10. Attachment를 선택하고 수락 을 클릭하십시오.\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/32.png" alt=""><figcaption></figcaption></figure>
11. **eu-central-1** 지역 에서 1-10단계를 반복합니다.

최종 결과는 다음과 같이 VPN 세그먼트로 엣지 eu-central-1 과 us-east-1에서 VPN 연결이 되어야합니다.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

VPC>Site-to-Site 메뉴를 통해 VPN 연결이 생성되는 동안 VPN 연결이 아래와 같이 수정되고 있음을 알 수 있습니다. 수정이 완료된 것을 확인하고 다음 단계로 진행합니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/34.png" alt=""><figcaption></figcaption></figure>

VPN 연결 배포가 완료되면 VPN 연결을 사용할 수 있습니다만, 아직 CGW에서 설정이 완료되지 않았기 때문에 터널 상태는 다운으로 표시됩니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/35.png" alt=""><figcaption></figcaption></figure>

### 4. OnPremise CGW 설정&#x20;

VPN 터널을 불러오려면 아래 단계를 따르십시오.

**us-east-1** 리전 :

1. 세션 관리자를 통해 strongswan VPN 어플라이언스에 연결
2. 아래 스크립트를 사용하여 VPN 터널을 불러옵니다.

```bash
sudo bash
strongswan up AWS1
```

VPN 터널이 up될때 에러가 발생할 경우 아래와 같이 재시작 또는 터널을 down, up하여 복구합니다.

```
strongswan restart
      또는
strongswan down AWS1; strongswan up AWS1
```

* AWS 환경에 대한 IPsec 터널이 설정됩니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/36.png" alt=""><figcaption></figcaption></figure>

* 아래 스크립트를 사용하여 BGP 관계의 상태를 확인하십시오.

```bash
sudo bash
vtysh
sh ip route bgp
```

* BGP 세션을 설정하고 AWS 환경에서 경로를 학습해야 합니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/37.png" alt=""><figcaption></figcaption></figure>

* VPN 연결이 AWS에서 작동하는지 확인

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/38.png" alt=""><figcaption></figcaption></figure>

**eu-central-1** 에서 1-5단계를 반복합니다.



참고 - US CGW에서 라우팅 정보 확인 예

```
h-4.2$ sudo -s
[root@CGW01 bin]# ip a
1: lo: <LOOPBACK, UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 02:9f:3c:0c:97:1b brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.43/27 brd 172.16.0.63 scope global dynamic eth0
       valid_lft 3159sec preferred_lft 3159sec
    inet6 fe80::9f:3cff:fe0c:971b/64 scope link
       valid_lft forever preferred_lft forever
3: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
4: vti11@NONE: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1400 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ipip 172.16.0.43 peer 34.228.124.249
    inet 169.254.100.2 peer 169.254.100.1/32 scope global vti11
       valid_lft forever preferred_lft forever
    inet6 fe80::5efe:ac10:2b/64 scope link
       valid_lft forever preferred_lft forever
5: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
[root@CGW01 bin]# ip ro
default via 172.16.0.33 dev eth0
10.11.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.12.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.13.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.14.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.21.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.22.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.23.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
10.24.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
169.254.100.1 dev vti11 proto kernel scope link src 169.254.100.2
169.254.169.254 dev eth0
172.16.0.32/27 dev eth0 proto kernel scope link src 172.16.0.43
172.17.0.0/16 via 169.254.100.1 dev vti11 proto zebra metric 100
[root@CGW01 bin]# vtysh

Hello, this is Quagga (version 0.99.22.4).
Copyright 1996-2005 Kunihiro Ishiguro, et al.

CGW01# sho ip bgp
BGP table version is 0, local router ID is 169.254.100.2
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*> 10.11.0.0/16     169.254.100.1          100             0 64512 i
*> 10.12.0.0/16     169.254.100.1          100             0 64512 i
*> 10.13.0.0/16     169.254.100.1          100             0 64512 i
*> 10.14.0.0/16     169.254.100.1          100             0 64512 i
*> 10.21.0.0/16     169.254.100.1          100             0 64512 64513 i
*> 10.22.0.0/16     169.254.100.1          100             0 64512 64513 i
*> 10.23.0.0/16     169.254.100.1          100             0 64512 64513 i
*> 10.24.0.0/16     169.254.100.1          100             0 64512 64513 i
*> 172.16.0.0       0.0.0.0                  0         32768 i
*> 172.17.0.0       169.254.100.1          100             0 64512 64513 65002 i

Total number of prefixes 10
CGW01#
```



### Lab 5 체크리스트

* [ ] eu-central-1의 OnPrem VPN Gateway에서 전달 받은 BGP 경로는 무엇인가요?
* [ ] &#x20;eu-central-1의 엣지에서 전달받은 라우팅 경로를 어떻게 확인할 수 있나요?
* [ ] Site-to-Site VPN 연결 상태 이벤트는 어떻게 확인 할 수 있나요?

