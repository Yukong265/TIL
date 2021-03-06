<h1>JWT</h1>


<h3>1. JWT는 JSON Web Token의 약자로 전자서명된 URL-safe의 JSON이다.</h3>

<h3>2. JWT와 관련된 JWE는 JSON을 암호화 하여 URL-safe 문자열로 표현 한 것이다.</h3>

<h3>3. JWT는 세파트로 나누어지며, 각 파트는 점으로 구분하여 xxx.yyy.zzz 이런식으로 표현 한다.
    순서대로 헤더(HEADER), 페이로드(PAYLOAD), 서명(SINATURE)로 구성한다.
</h3>
1. Header는 토큰의 타입과 해시 암호화 알고리즘으로 구성
2. Payload는 토큰에 담을 클레임 정보를 포함
3. Signature는 secret key를 포함하여 암호화.

- **Microservice with OAuth** : 기존의 토큰방식 인증은 다이어그램에 표시된 것처럼 토큰은 이후의 모든 서비스 호출에 사용됩니다.서비스를 받기 위해서는 토큰의 유효성을 확인하여 세부 정보를 쿼리해야합니다.
  참조에 의한 호출(By Reference) 형태로 모든 서비스는 항상 상호 작용할 때 다시 접속해야합니다.

- **Microservice with JWT** : JWT 와 같이 값에 의한 호출이 가능한 토큰이 필요합니다.
  토큰이 필요한 모든 정보를 포함하고 있어 참조(적어도 인증 및 권한 부여를 위해)가 필요없기 때문에 마이크로서비스 자체에서 유효성을 검증 합니다.
  이것이 JWT ( JSON Web Token )의 목적이다.

<h2>JWT Process</h2>

1. 사용자가 id와 password를 입력하여 로그인을 시도합니다.
2. 서버는 요청을 확인하고 secret key를 통해 Access token을 발급합니다.
3. JWT토큰을 클라이언트에 전달 합니다.
4. 클라이언트에서 API를 요청 할 때 클라언트가 Authorization header에 Access token을 담아서 보냅니다.
5. 서버는 JWT Signature를 체크하고 Payload로부터 사용자 정보를 확인해 데이터를 반환합니다.
6. 클라이언트의 로그인 정보를 서버 메모리에 저장하지 않기 때문에 토큰 기반 인증 메커니즘을 제공 합니다.

- 인증이 필요한 경로에 접근 할 때 서버측은 Authorization 헤더에 유효한 JWT 또는 존재하는지 확인한다.
- JWT에는 필요한 모든 정보를 토큰에 포함하기 때문에 데이터베이스와 같은 서버와의 커뮤니케이션 오버 헤드를 최소화 할 수 있습니다.
- Cross-Origin Resource Sharing (CORS)는 쿠키를 사용하지 않기 때문에 JWT를 채용 한 인증 메커니즘을 두 도메인에서 API를 제공하더라도 문제가 발생하지 않습니다.
- 일반적으로 JWT 토큰 기반의 인증 시스템은 위와 같은 프로세스로 이루어지며, 처음 사용자를 등록 할 때 Access token과 Refresh token이 모두 발급 되어야 합니다.

<h2>JWT의 장점과 단점</h2>

JWT의 주요한 이점은 사용자 인증에 필요한 모든 정보는 토큰 자체에 포함하기 때문에 별도의 인증 저장소가 필요 없다는 것입니다.

**JSON Web token**의 사용을 권장하는 몇가지 이유는 다음과 같다.

- URL 파라미터와 헤더로 사용
- 수평 스케일이 용이
- 디버깅 및 관리가 용이
- 트래픽에 대한 부담이 낮음
- REST 서비스로 제공 가능
- 내장된 만료
- 독립적인 JWT

**단점**

- 토큰은 클라이언트에 저장되어 데이터 베이스에서 사용자 정보를 조작하더라도 토큰에 직접 적용 할 수 없다.
- 더 많은 필드가 추가되면 토큰이 커질 수 있다.
- 비상태 애플리케이션에서 토큰은 거의 모든 요청에 대해 전송되므로 데이터 트래픽 크기에 영향을 미칠 수 있다.

<h2>JWT의 사용법</h2>

<h3>회원인증</h3>

* JWT를 사용하는 가장 흔한 사용법이다. 사용자가 로그인을 하면 서버는 사용자의 정보를 기반으로 한 토큰을 발급한다. 그 후 사용자가 서버에 요청을 할 때 마다 JWT를 포함하여 전달한다.
* 서버는 클라이언트에서 요청을 받을때 마다, 해당 토큰이 유효하고 인증됐는지 검증을 하고, 사용자가 요청한 작업에 권한이 있는지 확인하여 작업을 처리합니다.
* 서버에서는 사용자에 대한 세션을 유지 할 필요가 없습니다. 
  즉 사용자가 로그인되어있는지 안되어있는지 신경 쓸 필요가 없고, 사용자가 요청을 했을때 토큰만 확인하면 되므로 세션 관리가 필요 없어서 서버 자원과 비용을 절감할 수 있습니다.

<h3>정보교류</h3>

- JWT는 두 개체 사이에서 안정성 있게 정보를 교환하기에 좋은 방법입니다. 그 이유는, 정보가 서명이 되어있기 때문에 정보를 보낸이가 바뀌진 않았는지, 또 정보가 도중에 조작되지는 않았는지 검증 할 수 있습니다.

  ​	~			~
