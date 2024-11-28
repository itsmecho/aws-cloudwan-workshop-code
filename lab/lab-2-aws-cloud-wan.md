# Lab 2 - AWS Cloud WAN 설정

AWS Cloud WAN을 시작하려면 [Network Manager](https://us-west-2.console.aws.amazon.com/networkmanager/home?region=us-east-1#/) 으로 이동하십시오.&#x20;

### **1. 글로벌 네트워크 배포**

* Network Manager 메뉴에서 Connecvity > Global Networks 를 선택합니다.
* Global Networks 화면에 "Create Global Network"을 선택하여 새로운 Global Network를 배포합니다.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

*   신규 Global Network 생성 화면에서 Global Network 이름을 입력하고 "Next"를 선택합니다.



    <figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### **2. 코어 네트워크 생성**

* Core Network general settings에서 Core Network의 이름과 설명을 추가합니다.
* Core Network policy settings에서 **ASN range** 64512-64555 를 입력하고, **Edge locations**에서 **us-east-1** 리전만 선택합니다. (다른 엣지 로케이션은 Lab 3의 정책 설정에서 추가 배포 됩니다.)
* 그리고 기본 Segment 할당을 위해 Segment Name에 공유 서비스를 호스팅하기 위한 공유 세그먼트 "shared" 를 입력합니다.
* "Next" 클릭하여 다음 화면 이동

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>그림 2 - 코어 네트워크 생성 화면</p></figcaption></figure>



### **3. 글로벌 네트워크 구성 검토**

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* 2번에서 입력한 설정이 옮바른지 확인 후 "Create global network"를  클릭 하여 배포를 시작합니다.
* 이제 글로벌 네트워크가 생성되고 아래와 같이 Core Network 가 만들어진 것을 확인할 수 있습니다. 하지만, 즉각 사용 가능한 것이 아니라 상태가 Pending 에서 Available 로 될 때까지는 15분 정도 소요될 수 있습니다.&#x20;

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

### **4. 코어 네트워크 구성 검토**

* 글로벌 네트워크를 생성되고 나면, **Core Network** 를 선택하여 세부 정보를 확인할 수 있습니다.&#x20;

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* **Policy versions** 를 클릭하고 Change Status가 "**Execution succeeded**" 상태로 변경된 것을 확인하고 다음 단계로 진행합니다.

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>



### Lab 2 체크리스트

* [ ] Network Manager를 사용하여 글로벌 네트워크를 생성할 수 있었습니까?
* [ ] Network Manager의 대쉬보드에서 글로벌 네트워크 상태를 확인하세요. 상태정보가  Available 된 것을 확인하였나요?
* [ ] 코어 네트워크의 상세 정보에서 네트워크 상태를 확인하세요. 상태정보가  Available 된 것을 확인하였나요?
*   [ ] 코어 네트워크에서 cloudwatch log가 활성화된 것을 확인하였나요?





\
