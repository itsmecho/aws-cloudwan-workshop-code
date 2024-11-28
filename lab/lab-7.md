# \*Lab 7 - 네트워크 세그멘테이션 검증

이번 단계에서는 세그멘테이션과 라우팅 정책이 잘 동작하는 지 각 VPC내 EC2간의 연결을 확인하여 검증합니다.

각 리전 및 VPC 별 IP 대역과 각 VPC내 EC2의 Private IP 주소를 확인합니다.



### 1. LAB 네트워크 환경&#x20;



#### 리전별 VPC CIDR 주소 대역&#x20;

```
us-east-1: 
  prod:    10.11.0.0/1
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



#### 세그먼트별 접근 정책

<figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/18.png" alt=""><figcaption></figcaption></figure>

### 2. EC2 인스턴스 간 연결 테스트&#x20;

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

"ping"  명령어를 사용하여 Shared Instance에서 인터넷 연결이 가능함을 확인합니다.&#x20;

<pre><code><strong>sh-4.2$ ping -c 1 -W 1 www.amazon.com
</strong>PING d3ag4hukkh62yn.cloudfront.net (52.85.148.155) 56(84) bytes of data.
64 bytes from server-52-85-148-155.iad89.r.cloudfront.net (52.85.148.155): icmp_seq=1 ttl=235 time=0.684 ms
--- d3ag4hukkh62yn.cloudfront.net ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.684/0.684/0.684/0.000 ms
</code></pre>

"ping"  명령어를 사용하여 다른 리전내 다른 VPC내 인스턴스로  연결이 가능함을 확인합니다. (각 EC2 의 Private IP 주소는 EC2 메뉴에서 확인 가능합니다.) 연결 정책은 세그먼트 연결 정책을 참조하십시오.

```
sh-4.2$ ping -c 1 -W 1 10.11.0.26
PING 10.11.0.26 (10.11.0.26) 56(84) bytes of data.

--- 10.11.0.26 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 76ms

sh-4.2$ ping -c 1 -W 1 10.12.0.24
PING 10.12.0.24 (10.12.0.24) 56(84) bytes of data.

--- 10.12.0.24 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 74ms

sh-4.2$ ping -c 1 -W 1 10.13.0.15
PING 10.13.0.15 (10.13.0.15) 56(84) bytes of data.

--- 10.13.0.15 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 75ms
```



prod 인스턴스로 접속하여 위의 테스트를 반복합니다.\


### 3. 세그멘테이션 정책 검증&#x20;

설정된 정책의 상호 연결 상태는 Logical view를 통해 확인할 수 있습니다. 이를 통해 세그멘테이션 공유 상태를 확인합니다.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

### 4. 라우팅 테이블 확인

&#x20;라우팅 경로에 대한 트러블 슈팅이 세그먼트 별 라우팅 정보를 각 에지에서 확인하여 수행합니다.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>



### Lab 7 체크리스트

* [ ] Shared Instance에서 리전 내 다른 VPC(Prod, Finance, HR)의 인스턴스로 Ping이 가능합니까?
* [ ] Prod Instance에서 인터넷으로 Ping이 가능합니까? 불가능하다면 이유는 무엇입니까? 가능하다면 그 이유는 무엇입니까?
* [ ] Prod Instance에서 Finance Instance로 Ping이 가능합니까? 불가능하다면 어떻게 가능하게할 수 있을 까요?
