# \*Lab 6 - 라우팅 정책 설정

### **1. 네트워크 정책 수정**

* **us-east-1** 에 있는지 확인하십시오.
* 네트워크 관리자 의 **핵심 네트워크** 섹션 에서 **정책 버전 을 클릭합니다.**
* 현재 정책 버전 ID를 선택하고 **편집 을 클릭합니다.**

#### **a. 공유 인터넷 egress 위한 경로**

인터넷 연결을 위해 기본 경로(0.0.0.0/0)에 대해서 shared attachment를 target으로 하는 경로 정책을 적용합니다.

이렇게 하면 각 지역의 공유 VPC에 있는 NAT 게이트웨이를 통해 인터넷에 대한 egress 액세스가 허용됩니다.

* **세그먼트 작업** 탭 을 클릭합니다.
* **경로** 섹션에서 **만들기** 를 클릭 합니다 .
* 드롭다운 에서 prod 세그먼트 선택
* Destination CIDR 블록에 기본 경로 _**0.0.0.0/0 추가**_
* **attachment 의** 드롭다운 에서 us-east-1 및 eu-central-1 공유 vpc 첨부 파일을 선택 합니다.
*   **구간 경로 생성을** 클릭 합니다.



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/52.png" alt=""><figcaption></figcaption></figure>
* **finance, hr** 세그먼트 에 대해 위 단계를 반복합니다.

**b. 블랙홀 경로**

prod , finance 및 hr 세그먼트 에 대한 블랙홀 경로를 생성합니다 . 이렇게 하면 특정 경로가 경로 테이블에 전파되지 않는 한 세그먼트 간의 의도하지 않은 트래픽 경로가 방지됩니다.

* **세그먼트 작업** 탭 을 클릭합니다.
* **경로** 섹션에서 **만들기** 를 클릭 합니다 .
* 드롭다운 에서 **제품** 세그먼트 선택
* 대상 CIDR 블록에 경로 _**10.0.0.0/8 추가**_
* **블랙홀** 확인란 을 선택합니다.
*   **구간 경로 생성을** 클릭 합니다.



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/60.png" alt=""><figcaption></figcaption></figure>
* **finance** 및 hr 부문 에 대해 반복합니다.
*   최종 결과는 다음과 같아야 합니다.



    **정책 생성**

    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/58.png" alt=""><figcaption></figcaption></figure>
* 모든 구성이 올바른지 확인
* **정책 만들기를** 클릭 합니다.
*   새로운 정책 버전은 **Pending generation 의 변경 세트 상태와 함께 표시되어야 합니다.**



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/13.png" alt=""><figcaption></figcaption></figure>
* 정책이 생성된 후 상태는 **실행 준비 완료로 표시되어야 합니다.**
*   새 정책 버전을 선택하고 **변경 세트 보기 또는 적용 을 클릭합니다.**



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/14.png" alt=""><figcaption></figcaption></figure>
*   변경 사항을 검토하고 **변경 세트 적용** 을 클릭하여 변경 사항을 실행할 수 있습니다.



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/15.png" alt=""><figcaption></figcaption></figure>
*   완료되면 **변경 세트 상태 가 실행 성공** 으로 표시되어야 합니다 .



    <figure><img src="https://static.us-east-1.prod.workshops.aws/public/bbadbd28-7122-44c1-b93d-2e5167ff0867/static/16.png" alt=""><figcaption></figcaption></figure>

이 섹션에서는 Cloud WAN 핵심 네트워크 라우팅 구성 및 정책을 성공적으로 배포했습니다.





### Lab 7 체크리스트

* #### **a. 공유 인터넷 egress 위한 경로 설정 후 us-east-1 엣지에서 prod 세그먼트로 공유된 라우팅 정보를 확인하고 다음에 답하세요.**&#x20;
* [ ] &#x20;**prod에서 finance 인스턴스로 통신이 가능한가요? 가능하다면 어떻게 가능한지 설명해주세요.**
* #### **b.  블랙홀 경로 설정후 us-east-1 엣지에서 prod 세그먼트로 공유된 라우팅 정보를 확인하고 다음에 답하세요.**&#x20;
* [ ] &#x20;**prod에서 finance 인스턴스로 통신이 가능한가요? 가능하다면 어떻게 가능한지 설명해주세요.**

