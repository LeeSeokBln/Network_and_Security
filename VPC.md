# Internet G/W, Egress-only internet G/W, NAT G/W
### 이 세 가지 구성요소는 AWS에서 VPC환경에서의 인터넷 연결을 관리하기 위해 사용된다.

Internet Gateway
VPC와 인터넷 간의 브리지 역할을 한다. 이를 통해 VPC 내부의 인스턴스가 인터넷에 직접 연결될 수 있다.
공개 서브넷 내의 인스턴스가 인터넷에 직접 연결되어야 하는 경우에 사용된다.

Egress-only Internet Gateway
IPv6 활성화된 VPC에서 프라이빗 서브넷에 있는 인스턴스가 인터넷에 나가기 위한 경로를 제공한다. 주로 IPv6 주소를 사용하는 VPC에서 사용된다
IPv6를 사용하는 프라이빗 서브넷의 인스턴스가 인터넷 업데이트나 패치를 받아야 할 때 사용된다.

NAT Gateway
프라이빗 서브넷 내의 인스턴스가 인터넷으로 아웃바운드 연결을 할 수 있게 하지만, 인터넷에서 직접적인 인바운드 트래픽은 허용하지 않는다.
공개 서브넷 내에 위치하여 프라이빗 서브넷의 인스턴스들에게 인터넷 엑세스를 허용 한다.

VPC peering
AWS의 가상 사설 네트워크를 안전하게 연결하는 기능으로, VPC 간 사설 통신을 제공헌다. IP 주소 충돌을 피하며 라우팅 설정을 통해 구성되며, 변경 사항 자동 업데이트와 다른 계정 간 연결도 가능하다. 다만 트랜지티브 연결은 불가능하다. 이로써 VPC 간 안전하고 효율적인 네트워크 통신을 가능하게 하다.

VPC EndPoint, Private Link
### VPC Endpoint와 PrivateLink는 VPC 내의 리소스가 AWS 서비스 및 VPC 엔드포인트 서비스로 프라이빗 연결을 생성하는 데 사용되는 기능이다.

VPC Endpoint
AWS VPC 내에서 실행되는 인스턴스와 다른 AWS 서비스 간의 트래픽이 인터넷을 통하지 않고 AWS의 프라이빗 네트워크를 통해 직접 연결되도록 허용한다. 이는 보안 및 성능을 향상시키는 데 도움이 된다.

VPC Endpoint에는 두가지 유형이 있다.
### Interface Endpoints
특정 AWS 서비스 또는 VPC 엔드포인트 서비스에 대한 엔드포인트를 생성한다.
ENI를 VPC 내에 프로비저닝하여 작동한다.
### Gateway Endpoints
Amazon S3 및 DynamoDB와 같은 특정 AWS 서비스에 대해 설계되었다.
라우팅 테이블을 활용하여 특정 대상에 대한 트래픽을 엔드포인트로 리디렉션한다.

## PrivateLink
VPC와 VPC 엔드포인트 서비스 간의 프라이빗 연결을 활성화하는 기술이다. Interface Endpoints의 주요 기술로 사용되며, 인터넷이나 VPN, Direct Connect를 통하지 않고 AWS 서비스에 프라이빗으로 액세스할 수 있게 한다.
