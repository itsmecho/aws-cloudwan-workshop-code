# Lab 4 - VPC attachment(연결)

attachments에서 중요한 것 중 하나는 태그를 잘 활용하는 것입니다. Cloud WAN은 첨부 태그를 사용하여 첨부 정책에 정의된 대로 첨부 파일을 원하는 세그먼트에 자동으로 연결할 수 있습니다. 리소스(예: vpc-id)를 세그먼트에 명시적으로 매핑하거나 첨부 파일의 태그를 사용하는 두 가지 방법을 사용하여 첨부 파일을 세그먼트에 매핑하도록 선택할 수 있습니다.&#x20;

이 워크샵에서는 각 VPC에 대한 첨부 파일을 생성한 다음 언급한 대로 이러한 첨부 파일의 태그를 사용하여 올바른 네트워크 세그먼트에 매핑할 것입니다.

### **1. VPC** attachment 설정

1. 네트워크 관리자 에서 Core Network 섹션 아래의 attachments를 클릭합니다.
2.  create attachment 클릭하여 새로운 연결을 추가합니다.\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/19.png" alt=""><figcaption></figcaption></figure>
3.  Attachment의 이름, 엣지 위치를 선택하고 attachment type을 VPC로 선택합니다. \


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/20.png" alt=""><figcaption></figcaption></figure>
4.  VPC attachment 섹션에서 공유 vpc의 vpc-id를 선택하십시오 .각 가용 영역 의 서브넷 ID 에서 연결할 서브넷 을 선택합니다.(\*-attachment-subnet-\* 선택)\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/21.png" alt=""><figcaption></figcaption></figure>
5.  태그 섹션에서 공유 세그먼트 정책 정의에 지정된 것과 일치하는 태그 키 및 값을 입력합니다.\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/22.png" alt=""><figcaption></figcaption></figure>
6. Create attachments 를 클릭 후 상태를 확인합니다.
7. Tag가 제대로 설정된 경우 attachment 상태는 첨부 **수락 보류 로 표시됩니다.**
8.  attachment 를 선택하고 **수락 을 클릭하십시오.**\


    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/23.png" alt=""><figcaption></figcaption></figure>
9. us-east-1 , eu-central-1 , ap-southeast-1, ap-norhteast-1 리전 의 모든 VPC에 대해 1-9단계를 반복합니다.
10. 최종 결과는 다음과 같아야 합니다.



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/25.png" alt=""><figcaption></figcaption></figure>

11\. VPC 라우팅 테이블 업데이트&#x20;

VPC 를 Core Network와 연결할 때에는 각 VPC내 라우팅 테이블에 Core Network Edge를 추가하는 것이 매우 중요합니다.&#x20;

**VPC - prod, finance, hr의 라우팅 테이블**&#x20;

그림과 같이 각 리전 내 서비스별 VPC - **prod**, **finance**, **hr** 의 기본 라우팅(0.0.0.0/0) 테이블이 Cloud WAN의 코어 네트워크의 ARN을 target을 향하도록 설정합니다.&#x20;

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/26.png" alt=""><figcaption></figcaption></figure>

**VPC - shared의 라우팅 테이블**&#x20;

그리고, 인터넷으로 연결되는 Shared VPC 의 퍼블릭 라우팅 테이블에는 내부 IP 대역에 대한 **hared-Public-RT )에 10.0.0.0/8** 경로를 추가 합니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/53.png" alt=""><figcaption></figcaption></figure>



### **2. 각 VPC EC2 인스턴스에서 상호 연결 확인**&#x20;

### EC2 인스턴스 연결&#x20;

각 VPC 내의 EC2 인스턴스에 Session Manager를 통해 연결하여 각 지역, 각 VPC내 EC2에 연결 상태를 Ping을 사용하여 확인합니다. 각 리전에 연결하여 EC2의 Private IP를 먼저 확인하고 ping을 수행하세요.&#x20;

세그멘테이션 정책에 따라 동일 리전내 다른 VPC의 EC2 연결 상태, 다른 리전 간 VPC의 EC2 연결 상태가 달라지는 것을 확인합니다.&#x20;

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption><p>Shared Instance에 연결 </p></figcaption></figure>



검증 예: Shared EC2에서 다른 리전내 각 VPC로 ping 수행

```
sh-4.2$ sudo -s
[root@ip-10-14-0-27 bin]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 02:ff:59:f0:af:6b brd ff:ff:ff:ff:ff:ff
    inet 10.14.0.27/27 brd 10.14.0.31 scope global dynamic eth0
       valid_lft 2943sec preferred_lft 2943sec
    inet6 fe80::ff:59ff:fef0:af6b/64 scope link
       valid_lft forever preferred_lft forever
[root@ip-10-14-0-27 bin]# pign 10.21.0.10
bash: pign: command not found
[root@ip-10-14-0-27 bin]# ping 10.21.0.10
PING 10.21.0.10 (10.21.0.10) 56(84) bytes of data.
64 bytes from 10.21.0.10: icmp_seq=1 ttl=253 time=94.0 ms
^C
--- 10.21.0.10 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 94.069/94.069/94.069/0.000 ms
[root@ip-10-14-0-27 bin]# ping 10.22.0.27
PING 10.22.0.27 (10.22.0.27) 56(84) bytes of data.
64 bytes from 10.22.0.27: icmp_seq=1 ttl=253 time=93.3 ms
^C
--- 10.22.0.27 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 93.383/93.383/93.383/0.000 ms
[root@ip-10-14-0-27 bin]# ping 10.23.0.12
PING 10.23.0.12 (10.23.0.12) 56(84) bytes of data.
64 bytes from 10.23.0.12: icmp_seq=1 ttl=253 time=94.2 ms
64 bytes from 10.23.0.12: icmp_seq=2 ttl=253 time=92.5 ms
^C
--- 10.23.0.12 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 92.548/93.402/94.257/0.907 ms
[root@ip-10-14-0-27 bin]# ping 10.24.0.28
PING 10.24.0.28 (10.24.0.28) 56(84) bytes of data.
64 bytes from 10.24.0.28: icmp_seq=1 ttl=253 time=94.3 ms
^C
--- 10.24.0.28 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 94.366/94.366/94.366/0.000 ms
[root@ip-10-14-0-27 bin]#
```

### Lab 4 체크리스트

* [ ] Shared Instance서 인터넷으로 Ping이 가능합니까?
* [ ] Shared Instance에서 서로 다른 리전 내 다른 VPC(Prod, Finance, HR)의 인스턴스로 Ping이 가능합니까?
* [ ] Prod Instance에서 인터넷으로 Ping이 가능합니까?
* [ ] Prod Instance에서 서로 다른 리전 내 다른 VPC(Prod, Finance, HR)의 인스턴스로 Ping이 가능합니까?



