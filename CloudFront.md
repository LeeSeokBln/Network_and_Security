# CDN
**전 세계 여러 위치에 분산된 서버 네트워크로 구성된 시스템을 말한다. 이 서버 네트워크는 웹 콘텐츠 및 애플리케이션을 사용자에게 빠르고 효율적으로 전달하는 데 중점을 돈다. CDN의 주요 목적은 사용자의 지리적 위치와 관계없이 콘텐츠의 로딩 시간을 최소화하는 것이다.**

## CDN의 핵심 특징 및 이점:

**```성능 향상:```** 사용자가 요청한 콘텐츠는 가장 가까운 엣지 서버에서 제공되므로 데이터 전송 시간이 단축되고 웹사이트의 로딩 시간이 빨라진다.

**```신뢰성 및 가용성:```** CDN은 여러 서버에 콘텐츠 복사본을 저장하기 때문에 한 서버에 문제가 발생하더라도 다른 서버에서 콘텐츠를 제공할 수 있다. 이로 인해 높은 가용성과 장애 복구 능력을 제공한다.

**```트래픽 피크 처리:```** 대규모 트래픽 또는 트래픽의 급증이 있을 때, CDN은 요청을 분산하여 원래의 서버에 대한 부하를 줄여준다.

**```보안:```** DDoS 공격과 같은 일부 보안 위협에 대해 추가적인 보호층을 제공할 수 있다.

**```비용 절감:```** 원본 서버의 부하가 감소함으로써, 서버 유지 및 대역폭 비용을 줄일 수 있다.

## 작동 원리:
사용자가 웹사이트에 액세스하려 할 때 CDN으로 리다이렉트 된다.
CDN은 사용자의 위치를 파악하고 가장 가까운 엣지 서버를 결정한다.
해당 엣지 서버에 요청된 콘텐츠의 캐시된 복사본이 있는 경우, 해당 복사본이 사용자에게 전송된다.
만약 엣지 서버에 캐시된 복사본이 없거나 캐시된 내용이 오래된 경우, 원본 서버에서 콘텐츠를 가져와 사용자에게 전달하고, 해당 콘텐츠를 엣지 서버에 캐시한다.

# loudFront가 S3에 있는 static asset을 캐싱 하고 있고 방금 S3의 파일들이 배포가 일어났을 때, 각 edge로 즉시 캐시 파일을 업데이트할 수 있는 방법
**Amazon S3와 연동하여 웹사이트의 static asset들을 전 세계에 분산된 edge location에 캐시합니다. 새로운 파일이 S3에 배포되거나 기존 파일이 변경될 경우, CloudFront의 캐시된 콘텐츠는 업데이트되지 않는다. 이러한 상황에서 CloudFront 캐시를 즉시 업데이트하려면**
**```CloudFront 캐시 무효화:```**
CloudFront 콘솔, AWS CLI, SDK를 통해 캐시 무효화를 생성할 수 있다.
무효화를 생성하면 CloudFront는 해당 콘텐츠의 캐시된 버전을 삭제한다.
사용자의 요청이 다음에 들어오면 CloudFront는 원본 위치 (예: Amazon S3)에서 새로운 버전의 콘텐츠를 가져와 캐시하게 된다.

**```TTL 설정 변경:```**
CloudFront에서는 각 콘텐츠 유형에 대해 TTL 값을 설정할 수 있다. TTL은 캐시된 콘텐츠가 얼마 동안 유효한지를 나타낸다.
배포가 자주 이루어지는 경우, TTL 값을 낮게 설정하여 캐시된 콘텐츠가 빠르게 만료되도록 할 수 있다. 그러나 이 방법은 서버에 부하를 주는 요인이 될 수 있으므로 신중하게 설정해야 한다.

**```버전화 또는 지문:```**
파일 이름에 버전 번호나 해시 값을 포함시켜 새로운 파일 배포 시 새로운 URL을 생성하게 할 수 있다.
예: styles-v2.css 또는 styles-abcdef123456.css
이 방법으로, 캐시 무효화를 수행할 필요 없이 자동으로 최신 버전의 파일을 가져올 수 있다.

# Client -> CloudFront -> EC2(Backend)로 request가 들어올 때 EC2에서 Client의 IP를 알 수 있는 방법
**Client의 요청이 CloudFront를 통해 EC2로 전달될 때, EC2에서 Client의 IP 주소를 확인하려면 CloudFront에서 원래의 클라이언트 IP 정보를 포함하는 헤더를 추가로 전달해야 한다**
**```X-Forwarded-For 헤더```**
    CloudFront는 클라이언트의 IP 주소를 X-Forwarded-For 헤더에 추가하여 EC2로 전달한다.
    EC2에서는 이 헤더를 읽어 클라이언트의 실제 IP 주소를 알아낼 수 있다.
    일반적으로 이 헤더를 사용하여 리버스 프록시나 로드 밸런서 뒤의 서버에서 원본 클라이언트 IP 주소를 파악한다.

# CORS
**CORS는 웹 페이지의 리소스가 다른 도메인의 리소스에 접근할 때 안전하게 그 접근을 허용하기 위한 표준이다. 기본적으로 웹 브라우저는 보안 상의 이유로 다른 오리진의 리소스에 대한 요청을 제한한다. CORS는 HTTP 헤더를 사용하여 한 출처에서 다른 출처의 리소스에 접근 권한을 부여할 수 있게 한다.**

### CloudFront에서의 CORS 관련 옵션

**```Origin Configuration:```**
        CloudFront 배포의 오리진 설정에서 특정 HTTP 헤더를 포워딩하도록 설정할 수 있다. Access-Control-Allow-Origin와 같은 CORS 관련 헤더를 포워딩하도록 설정하면 오리진 서버의 CORS 설정이 클라이언트에 제대로 전달된다.

**```Cache Based on Selected Headers:```**
        CloudFront에서는 캐시 동작을 조절하기 위해 특정 헤더를 선택하여 캐시 키로 사용할 수 있다. CORS를 올바르게 처리하기 위해 Origin 헤더를 기반으로 캐시 응답을 분리할 수 있다.

**```Behavior Settings:```**
        CloudFront 배포에서 각 URL 패턴에 대한 동작을 정의하는 behavior 설정에서 HTTP 메서드를 허용할 수 있습니다. CORS 프리플라이트 요청에 대응하기 위해 OPTIONS 메서드를 허용하도록 설정해야 한다.

# CloudFront에서 backend에서 준 정보에 추가로 http response header에 정보를 추가하는 방법
**CloudFront에서 백엔드에서 제공하는 정보에 추가로 HTTP 응답 헤더를 추가하려면 주로 Lambda@Edge 함수를 사용한다. Lambda@Edge는 CloudFront의 이벤트에 따라 Lambda 함수를 실행하게 해준다.**
### CloudFront에서 HTTP 응답 헤더 추가 방법
**```Lambda@Edge 사용:```**

Viewer Response 또는 Origin Response 이벤트를 사용하여 Lambda 함수를 트리거한다.
이 함수 내에서 응답 헤더를 수정하거나 추가한다.
```
exports.handler = (event, context, callback) => {
    const response = event.Records[0].cf.response;
    response.headers['strict-transport-security'] = [{
        key: 'Strict-Transport-Security',
        value: 'max-age=63072000; includeSubdomains; preload'
    }];
    callback(null, response);
};
```
