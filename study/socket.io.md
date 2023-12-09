# how it works
## Scoket.IO
### 동작 원리
- Socket.IO는 Engine.IO을 기반으로 동작한다.
  - Engine.IO는 WebSocket/HTTP Long Polling을 이용해 실시간 양방향 통신을 구현하는 라이브러리이다.
- Socket.IO protocol은 multiplexing을 추가해 지원한다. (이는 namespace로 불린다.)

### Exchange protocol
- Socket.IO packet은 아래 fields를 포함한다.
    - packet type: integer
    - a namespace: string
    - payload: Object | Array
    - acknowledgement id: integer

### 추가된 기능
- 자동 연결
- packet buffering
    - reconnection전에는 buffer에 둠
- acknoledgement
- broadcasting (to all clients or to a subset of clients)
- multiplexing

## Engine.IO
- low-level connection을 담당한다.
    - transports and upgrade mechanism을 담당
    - disconnection detection 담당
- 
### Transport
- 종류
    - HTTP long-polling
        - GET: long running receiving
        - POST: short running sending
    - WebSocket
        - 빠른데
- Handshake
    - connection 시작에 server가 보내는 데이터
        ```json
        {
            "sid": "session id",
            "upgrades": ["websocket"],
            "pingInterval": 25000,
            "pingTimeout": 20000
        }
        ``` 
    - sid는 이후 모든 요청에 query parameter로 포함되어야 한다.
    - upgrades 는 적절한 transport를 선택하기 위해 사용된다.
    - pingInterval, pingTimeout은 heartbeat을 위해 사용된다.
### Upgrade mechanism
- 최초엔 long-polling, 되면 websocket 
    - UX + reliability first
- 방법
    - 다른 걸로 연결 시도
        - client
            - long-polling 멈춤 
            - sid로 websocket connection 열기
            - ping에 probe(string) payload 보내기
        - server
            - 기다리는 HTTP long-polling GET request에 대해서 noop packet 보내기
            - pong에 probe(string) payload 보내기
    - 진행 되면 바꾸고 첫번째거 끄기

- 과정
    - handshake
    - send data (HTTP long-polling) 
    - receive data (HTTP long-polling)
    - upgrade (WebSocket)
    - receive data

### Disconnection detection
- Engine.IO 는 아래 조건에서 connection이 끊어 졌다고 생각함.
    - one HTTP request (either GET or POST) failed
    - websocket connection closed
    - socket.disconnect() called on server/client side
- heartbeat mechanism
    - server는 pingInterval에 ping을 client에 보냄 
        - client가 pingTimeout 안에 pong을 안보내면 끝났다고 봄
    - client는 pingTimeout 안에 pong을 보내야 함
        - server가 pingInterval + pingTimeout 안에 pong을 안보내면 끝났다고 봄
