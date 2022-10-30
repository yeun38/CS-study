### HTTP의 특징
1. HTTP(HyperText Transfer Protocol)란?
- 데이터를 주고 받기 위한 프로토콜

2. Stateless, connectless의 장점
- 하나의 요청에 응답하므로 독립적이다.
- 서버 디자인이 간단하다.

3. Stateless, connectless의 단점
- 이전의 정보를 저장하지 않기 때문에 매번 인증해야한다.
> 이를 해결하기 위해 **쿠키** 혹은 **세션**을 사용한다.

4. HTTPS의 특징
- HTTP는 평문 데이터를 전송하는 프로토콜이기 때문에, 제 3자에 의해 조회가능하다.
> 이를 보완하기 위해 HTTPS가 생겼다.
- HTTP는 TCP와 직접 통신을 한다.
- 그러나 HTTPS는 중간에 SSL를 한 번 거쳐서 TCP와 통신한다.
- **SSL(Secure Socket Layer)**은 인터넷을 통해 전달되는 정보를 보호하기 위해 개발한 통신 규약이다.
- SSL를 통해 안전성 확보가 가능하고, 암호화가 가능하다.