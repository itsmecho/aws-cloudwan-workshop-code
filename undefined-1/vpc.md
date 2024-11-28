# 기본 VPC 삭제

계정의 각 리전에 대해 기본 VPC가 자동으로 생성됩니다. 계정의 리전당 최대 VPC 수는 5개(소프트 제한이지만)이고 이번 워크샵에서는 각 리전에서 최대 5개의 VPC가 사용됩니다. 따라서, 기본 VPC를 포함하면 한도가 초과할 수 있기 때문에 기본 VPC를 제거하거나 지원 콘솔을 통해 한도를 늘리도록 요청해야합니다.

참고: 현재 사용중인 계정에서 VPC 사용 수를 증가하려면 아래 링크를 참조하여 한도 증가 요청을 할 수 있습니다.

[https://console.aws.amazon.com/support/cases#/create](https://console.aws.amazon.com/support/cases#/create)

참고: 기본 VPC를 제거하더라도 아래 링크를 참조하여 나중에 다시 콘솔이나 CLI를 통해 다시 생성할 수 있습니다.&#x20;

[https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)



아래의 순서대로 us-east-1, eu-central-1 리전의 기본 VPC를 제거하도록 합니다. (해당 리전은 Lab 5에서 OnPrem VPC가 추가되어야하기 때문에 최대 5개의 VPC가 필요합니다.)

1. AWS Management 콘솔에서 작업할 리전으로 변경합니다. 이 리전 선택은 오른쪽 상단 롭다운 메뉴에 있습니다.
2. AWS Management 콘솔 에서 _**서비스**_ 를 선택한 다음 _**VPC**_ 를 선택 합니다.
3. 왼쪽 메뉴에서 _**VPC**_ 를 선택합니다 .
4. 기본 패널에서 VPC(기본 VPC)만 옆에 있는 확인란이 강조 표시되어야 합니다. 오른쪽으로 스크롤하여 이것이 기본 VPC인지 확인할 수 있습니다. _**기본 VPC**_ 열은 예로 표시됩니다.
5. 기본 VPC가 선택된 상태에서 위의 작업 드롭다운을 선택하고 _**VPC 삭제**_ 를 선택 합니다.
6. _**VPC 삭제**_ 패널 에서 '기본 VPC를 삭제하고 싶습니다.' 확인란을 선택합니다. 오른쪽 하단의 _**VPC 삭제**_ 버튼을 클릭합니다 .
7. 'VPC가 삭제되었습니다'라는 녹색으로 강조 표시된 대화 상자가 표시되고 닫기를 클릭할 수 있습니다.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
삭제가 되지 않고 대화상자가 빨간색으로 표시되는 경우 이미 기본 VPC에 무언가가 배포되었을 가능성이 있습니다. 이럴 경우 에러를 발생시키는 기존 리소스(EC2 인스턴스, NAT 게이트웨이, VPC 엔드포인트 등의 항목으로 VPC가 삭제되지 않도록 하는 모든 항목)를 먼저 제거해야 합니다.&#x20;
{% endhint %}

\
