## Connection Timeout과 Read Timeout의 의미
### Connection TimeOut
- Connection Timeout은 종단 간 연결하는데 소요되는 최대 시간을 의미한다. 이 시간을 넘기게 되면 연결 할 수 없는 것으로 판단하고 에러가 발생 한다.
- Connection 이라는 단어가 의미하는 것처럼 종단 간 연결에 사용되는 타임아웃이다. 그리고 이 때의 연결이란 우리가 잘 알고 있는 TCP 3 way handshake를 통해 TCP 연결이 생성되는 것을 의미한다.

### Read TimeOut
- Read Timeout은 연결된 종단 간에 데이터를 주고 받을 때 소요되는 최대 시간을 의미한다. 이 시간을 넘기게 되면 데이터를 받을 수 없는 것으로 판단하고 에러가 발생한다.
- Read 라는 단어가 의미하는 것처럼 연결되어 있는 종단 간 데이터를 주고 받을 때 사용되는 타임아웃이다.

![](https://velog.velcdn.com/images/cil05265/post/15e730f4-cfc6-403b-a294-2c11a164865a/image.png)

### 타임아웃 설정을 위한 기준 정하기
- 타임아웃 값을 설정하기에 앞서 기준을 정해야한다. 그리고 그 기준은 아래 두 가지 조건을 만족 시키도록 설정 될 수 있다.
1. 네트워크 상에서 패킷 유실은 꼭 장애 상황이 아니라도 언제든 발생할 수 있다.
2. 네트워크 상에서 문제가 발생했다면 가능한 빨리 인지해야 한다.

> 패킷 유실은 꼭 장애 상황이 아니라도 발생할 수 있다. 요즘에는 기술이 좋아져서 비교적 덜 하지만, 패킷 유실이라는 것은 언제든 발생할 수 있다. 실제로 네트워크에 장애가 발생했다면 가급적 빨리 발견해서 필요한 조치를 할 수 있어야 한다. 만약 타임아웃 값을 너무 짧게 둔다면 간헐적으로 발생할 수 있는 패킷 유실에 대해 너무 민감하게 반응하게 되고, 타임아웃 값을 너무 길게 준다면 네트워크 장애를 발견하는 데 긴 시간이 소요되게 된다.

> 한 번의 패킷 유실 정도는 재전송을 통해 해결 할 수 있는 수준의 타임아웃이다. 이에 대해서는 사람들마다 기준이 다르겠지만 어쨋든 중요한 건 기준을 세우고 그에 맞는 타임아웃 값을 설정하는 것이 중요하다는 것이다.

### Connection Timeout 값 설정하기
- Connection Timeout은 TCP 3 way handshake시 발생할 수 있는 연결 지연이기 때문에 연결을 맺는 과정에서 발생할 수 있는 패킷 유실에 대해 생각해 봐야한다.
- 고려해 볼 수 있는 경우의 수는 세 가지이다.
1. SYN 패킷이 유실 되었을 때
2. SYN + ACK 패킷이 유실 되었을 때
3. 마지막 ACK 패킷이 유실 되었을 때 

#### SYN 패킷이 유실 되었을 때
![](https://velog.velcdn.com/images/cil05265/post/cac9f6c4-e501-474d-9dc2-3de21f19e048/image.png)

- 사람들 마다 의견은 다르지만 보통 이상적인 값은 3초이다. 왜냐하면 세 경우에서 발생하는 패킷 유실이 경우 최대 1초 정도의 시간이 발생하기 때문이다. 한번의 패킷 유실로 인해 발생할 수 있는 지연 시간은 1초 이상이기 때문에 최소 1초 보다는 커야 한 번의 패킷 유실 정도는 무시하고 넘어가게 된다.
- 두 번 이상 패킷 유실이 발생한다면, 이 때는 네트워크 상에 이슈가 잇을지도 모른다는 생각을 해봐야한다. 연속해서 두 번 이상 패킷 유실이 발생한다면 확시히 이상이 있을 수 있기 때문이다. 특히 lnitRTO의 경우 2의 제곱값으로 갑싱 커지기 때문에 최초의 유실에 대해서는 1초를 기다리지만 두 번재 유실은 2초를 기다리게 된다.
- 두 번 이상의 패킷 유실이 발생한다면 3초 이상의 지연 시간이 발생하기 때문에 Connection Timeout이 3초라면 최소한 패킷 유실이 두 번 이상 발생했을 수 있다는 얘기가 된다. 이 경우 두 번 이상의 패킷 유실이 발생했다면 네트워크에 이슈가 있지 않은지 살펴 봐야할 필요가 있다.