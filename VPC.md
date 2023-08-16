# Internet G/W, Egress-only internet G/W, NAT G/W
### 이 세 가지 구성요소는 AWS에서 VPC환경에서의 인터넷 연결을 관리하기 위해 사용된다.

## Internet Gateway
# <h1 align="center"><img src="https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/images/internet-gateway-basics.png" width="480"><h1>
VPC와 인터넷 간의 브리지 역할을 한다. 이를 통해 VPC 내부의 인스턴스가 인터넷에 직접 연결될 수 있다.
공개 서브넷 내의 인스턴스가 인터넷에 직접 연결되어야 하는 경우에 사용된다.

## Egress-only Internet Gateway
# <h1 align="center"><img src="https://docs.aws.amazon.com/images/vpc/latest/userguide/images/egress-only-igw.png"><h1>
IPv6 활성화된 VPC에서 프라이빗 서브넷에 있는 인스턴스가 인터넷에 나가기 위한 경로를 제공한다. 주로 IPv6 주소를 사용하는 VPC에서 사용된다
IPv6를 사용하는 프라이빗 서브넷의 인스턴스가 인터넷 업데이트나 패치를 받아야 할 때 사용된다.

## NAT Gateway
# <h1 align="center"><img src="https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/images/public-nat-gateway-diagram.png"><h1>
프라이빗 서브넷 내의 인스턴스가 인터넷으로 아웃바운드 연결을 할 수 있게 하지만, 인터넷에서 직접적인 인바운드 트래픽은 허용하지 않는다.
공개 서브넷 내에 위치하여 프라이빗 서브넷의 인스턴스들에게 인터넷 엑세스를 허용 한다.
