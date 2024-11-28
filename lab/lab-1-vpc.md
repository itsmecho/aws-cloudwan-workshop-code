# Lab 1 - VPC 배포

### AWS 리전별 VPC 배포&#x20;

이번 실습에서는 AWS Cloud WAN을 설정하기에 앞서 리전 별로 각 업무 별로 VPC 생성하게 됩니다.&#x20;

신속한 VPC 배포를 위해 CloudFormation 템플릿을 사용하여 각 리전별로 VPC를 배포합니다.&#x20;

#### AWS VPC 배포 리전

* us-east-1
* eu-central-1&#x20;
* ap-southeast-1
* ap-northeast-1

#### 각 리전에 배포될 4가지 유형의 VPC

1. **Production VPC** - 라이브 프로덕션 시스템을 호스팅할 프로덕션 VPC입니다.
2. **HR VPC** - 이 vpc는 HR 사업부가 소유한 애플리케이션 및 워크로드를 호스팅합니다.
3. **Finance VPC** - 이 vpc는 재무 사업부가 소유한 애플리케이션 및 워크로드를 호스팅합니다.
4. **Shared VPC** - 이 vpc는 중앙 IT 팀이 소유하고 관리하는 NAT 게이트웨이와 같은 공유 서비스를 호스팅합니다.

### CloudFormation 템플릿 실행&#x20;

1. 워크샵에서 제공된 CloudFormation 템플릿 파일을 준비합니다.&#x20;
   * [ ] **파일 다운로드 -** [**VPC.YAML**](https://www.dropbox.com/scl/fi/hh5w0jxkjwar98j3stx4k/vpc.yaml?rlkey=63vmzq3cidyutmw31byn182v7\&st=g291yjre\&dl=0)
2. [CloudFormation 콘솔](https://console.aws.amazon.com/cloudformation/home?#/stacks) 로 이동[ ](https://console.aws.amazon.com/cloudformation/home?#/stacks)
3. **us-east-1** 지역 에 있는지 확인
4. **스택 생성** 을 클릭하고 **새 리소스 사용(표준)** 을 선택 합니다.\
   ![](<../.gitbook/assets/image (46).png>)
5.  **템플릿** 선택 준비 완료

    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/cft2.png" alt=""><figcaption></figcaption></figure>
6. **템플릿 파일 업로드**
7. 파일 선택 을 클릭 **합니다.**
8. **템플릿** 선택 준비 완료
9. **템플릿 파일 업로드**
10. 파일 선택 을 클릭 **합니다.**
11. 방금 다운로드한 `vpc.yaml`파일을 사용하고 **다음** 을 클릭 합니다.
12. 스택에 이름을 지정하십시오 `cloudwan-vpc`. 이 스택은 서브넷, 라우팅 테이블, EC2 인스턴스 및 인스턴스에 대한 SSM 연결을 위한 VPC 엔드포인트가 있는 4개의 VPC를 생성합니다.
13. **스택 옵션 구성** 에서 기본값을 수락하고 다음 을 클릭합니다 **.**
14. 파라미터와 IAM 리소스 공지를 검토하십시오. **AWS CloudFormation이 IAM 리소스를 생성할 수 있음을 인정합니다를** 선택 합니다.
15. **스택 생성** 을 클릭합니다 .
16. 계속하기 전에 스택이 생성될 때까지 기다리십시오. CloudFormation 콘솔의 상태가 스택 생성 성공을 나타내는 CREATE\_COMPLETE로 표시되는지 확인합니다.
17. **eu-central-1, ap-southeast-1, ap-northeast-1** 지역 에서 위의 단계를 반복합니다

### 각 리전별 VPC 배포 확인&#x20;

CloudFormation을 사용하여 4개의 리전에 각각 배포되는 VPC는 아래와 같습니다. AWS의 Cloud WAN은 각 VPC별 attachment subnet으로 연결(attach) 되게 됩니다.

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/vpc.png" alt=""><figcaption></figcaption></figure>

AWS Console을 통해 VPC 메뉴로 접속하여 아래와 같이 각 지역에 대해 4개의 VPC가 생성된 것을 확인합니다. 이때 CIDR이 각각의 리전에 맞게 적용되었는지 확인합니다.&#x20;

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption><p>VPC </p></figcaption></figure>

Shared VPC에서 인터넷 연결을 위해 인터넷 게이트웨이와 NAT 게이트웨이가 성공적으로 배포되었는 지 확인합니다. (Shared VPC 이외의 다른 VPC에서는 인터넷 연결이 불가능하기 때문에 인터넷 게이트웨이와 NAT 게이트웨이는 Shared VPC 에서만 배포됩니다.)

인터넷 게이트웨이와 NAT 게이트웨이 "VPC > Internet Gateways" 와 "VPC > NAT Gateways" 메뉴를 선택하여 확인합니다.

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p>Shared VPC에 Internet Gateway 연결 확인 </p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>Shared VPC에 NAT Gateway 연결 확인  </p></figcaption></figure>

### EC2 인스턴스 연결&#x20;

이제 4개의 각 리전에서 4개의 VPC를 시작했으며 각 VPC 내에는 Session Manager를 통해 액세스할 수 있는 EC2 인스턴스가 있습니다.  아래와 같이 **세션 관리자를 사용하여 us-east-1 리전의 Shared-Instance** 에 로그인 합니다.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption><p>Shared Instance에 연결 </p></figcaption></figure>

Session Manager를 통해 EC2에 연결한 후, "ip a" 명령어를 사용하여 Shared Instance의 IP 주소(eth0)를 확인합니다.

<pre><code>sh-4.2$ ip a
1: lo: &#x3C;LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: &#x3C;BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 02:ff:59:f0:af:6b brd ff:ff:ff:ff:ff:ff
    inet 10.14.0.27/27 brd 10.14.0.31 scope global dynamic eth0
       valid_lft 3523sec preferred_lft 3523sec
    inet6 fe80::ff:59ff:fef0:af6b/64 scope link
       valid_lft forever preferred_lft forever
<strong>
</strong></code></pre>

"ping"  명령어를 사용하여 Shared Instance에서 인터넷 연결 상태를 확인합니다.&#x20;

<pre><code><strong>sh-4.2$ ping -c 1 -W 1 www.amazon.com
</strong>PING d3ag4hukkh62yn.cloudfront.net (52.85.148.155) 56(84) bytes of data.
64 bytes from server-52-85-148-155.iad89.r.cloudfront.net (52.85.148.155): icmp_seq=1 ttl=235 time=0.684 ms
--- d3ag4hukkh62yn.cloudfront.net ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.684/0.684/0.684/0.000 ms
</code></pre>

"ping"  명령어를 사용하여 동일 리전내 다른 VPC내 인스턴스 간 연결 상태를 확인합니다. (각 EC2 의 Private IP 주소는 EC2 메뉴에서 확인 가능합니다.)

```
sh-4.2$ ping -c 1 -W 1 10.11.0.26
PING 10.11.0.26 (10.11.0.26) 56(84) bytes of data.

--- 10.11.0.26 ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 0ms

sh-4.2$ ping -c 1 -W 1 10.12.0.24
PING 10.12.0.24 (10.12.0.24) 56(84) bytes of data.

--- 10.12.0.24 ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 0ms

sh-4.2$ ping -c 1 -W 1 10.13.0.15
PING 10.13.0.15 (10.13.0.15) 56(84) bytes of data.

--- 10.13.0.15 ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 0mse
```



### Lab 1 체크리스트

* [ ] 4개의 리전에서 4개의 VPC(prod, finance, hr, shared)가 성공적으로 배포되었습니까?
* [ ] Shared Instance에서 인터넷으로 Ping이 가능합니까?
* [ ] Shared Instance에서 서로 다른 리전 내 다른 VPC(Prod, Finance, HR)의 인스턴스로 Ping이 가능합니까?
* [ ] Prod Instance에서 인터넷으로 Ping이 가능합니까?
* [ ] Prod Instance에서 서로 다른 리전 내 다른 VPC(Prod, Finance, HR)의 인스턴스로 Ping이 가능합니까?

