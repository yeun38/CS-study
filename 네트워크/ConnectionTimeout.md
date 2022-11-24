## Connection Timeout과 Read Timeout의 의미
### Connection TimeOut
- Connection Timeout은 종단 간 연결하는데 소요되는 최대 시간을 의미한다. 이 시간을 넘기게 되면 연결 할 수 없는 것으로 판단하고 에러가 발생 한다.
- Connection 이라는 단어가 의미하는 것처럼 종단 간 연결에 사용되는 타임아웃이다. 그리고 이 때의 연결이란 우리가 잘 알고 있는 TCP 3 way handshake를 통해 TCP 연결이 생성되는 것을 의미한다.

### Connection TimeOut
- Read Timeout은 연결된 종단 간에 데이터를 주고 받을 때 소요되는 최대 시간을 의미한다. 이 시간을 넘기게 되면 데이터를 받을 수 없는 것으로 판단하고 에러가 발생한다.
- Read 라는 단어가 의미하는 것처럼 연결되어 있는 종단 간 데이터를 주고 받을 때 사용되는 타임아웃이다.
