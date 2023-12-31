# Design

## Functionality

### chat
- 채팅룸 만들기
    - with name
    - with password
- 채팅룸 참여
    - with name
    - with password
- 채팅룸 떠나기
- 채팅
    - Can distinguish the sender and the receiver
    - Can send a message

## reference

### Architecture

- https://ably.com/blog/chat-app-architecture
    - Need to confirm
        - Scalability
        - Fault tolerance
        - Message synchronization and queue
            - enter the room in the middle of conversation
            - reconnecting
    - Common 7 components
        1. Application server
        2. Load balancer
        3. Streaming event manager
        4. User authentication and user manager
        5. Presence
        6. Media store
        7. Database

- https://www.cometchat.com/blog/chat-application-architecture-and-system-design
    - default functionality
        - user registration
        - message
        - push
        - file transfer
        - user presence
    - chat protocol
        - XMPP
        - MQTT
        - webRTC

- https://www.youtube.com/watch?v=ba4T590JPnw
    - TUTORIAL

- https://quickblox.com/blog/beginners-guide-to-chat-app-architecture/
    - XMPP⇒ messeging
    - Web socket ⇒ bidirectional channel, real-time

## Develop

### Bidirectional communication
- socket io
    - pros
        - Multi-language
        - Many features
    - cons
        - Unavailable to communitcate with plain web socket
        - It isn't intended to use for mobile back-end
            - Mobile battery could be drained because socket.io keeps the TCP connection alive.

- deepstream.io
    - pros
        - high code quality on [libhunt](https://nodejs.libhunt.com/socket-io-alternatives)
        - updated in last month
    - cons
        - not support media server

- plain web socket
    - pros
        - HTML5 standard
        - protocol
        - use small size of data on communication
    - cons
        - need to implement functions like reconnecting that socket.io already has

- UDP
    - pros
        - traditional method for communication
        - small and fast
    - cons
        - do things by my bare hand.

## database design
사진 ![image](https://private-user-images.githubusercontent.com/29042329/290530743-33d5ff2f-4585-4bbe-8760-65a0e1d6b7f6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDI1NjA0NzksIm5iZiI6MTcwMjU2MDE3OSwicGF0aCI6Ii8yOTA0MjMyOS8yOTA1MzA3NDMtMzNkNWZmMmYtNDU4NS00YmJlLTg3NjAtNjVhMGUxZDZiN2Y2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzEyMTQlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMjE0VDEzMjI1OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWU0MGY1MzczZTY2YmM4MjM5YTNlNGY1NmM1ODE1ZGQxODRjYjAyZWQ5MDk3N2MzYTNlMTY0ODQ0MmVhZDk5YTEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.j-JOr8WhiHplmLAiojITrd0u2xp1OiyX4YUfSfmiCh0)
### 고려한 점
- 채팅방에 접근할 수 있는 권한을 추후 인증하는 방법으로 점검 예정
- 접근 가능한 채널을 구분하는 걸 server joined user에 column하나 추가해서 idList를 만들어 주기적으로 업데이트 하는 방식으로 하면 될 것 같다는 생각

## API design
### 사용할  api방식
- REST
    - 장
        - 가장 익숙
        - express 기본으로 사용 가능
    - 단
        - socket io와 어떻게 공존 시킬지 모르겠음
- gRPC
    - 장
        - 안써봄
        - 빠르다 함
    - 단
        - 안써 봄
### 결정
- REST
    - 빠름
    - ref
        - 구조: https://github.com/anandundavia/express-api-structure/tree/master
### 스펙
- server
    - list
        - relation 없이 조회
    - create
        - post
    - update
        - put
    - delete
        - delete
    - userList
        - 서버에 조인한 유저 리스트만 적용
    - join
        - user받아서 serverJoinedUser 생성
    - leave
        - userId 받아서 serverJoinedUser 삭제
- user
    - signin
        - post
    - signup
        - post
    - signout
        - post
    - withdrawal
        - post
    - update
        - put
- channelgroup
    - list
        - channel도 함께 조회
    - create
    - update
    - delete
- channel
    - list
    - join
    - leave
    - create
    - delete
    - archive

## 아쉬운 점
### 에러객체를 정의하지 않은 것
- 프론트에서 에러메세지 보여주기 불편
### 프론트에서 호출하기 쉬운 형태로 출력짜기
- 응답이 때에 따라 다르면 파싱하기 까다로움 : user 등
- fetch 실패 시에도 에러메세지를 보여주기 위해 response를 json으로 파싱해야 함

## 배운 점
### credentials
- credential을 담고 싶으면 header에 Access-Control-Allow-Credentials를 true로 설정해야 함

## 비상!
### ws로 connect시 redis pub/sub channel에 연결을 시켜줘야 할텐데 어떻게 하면 좋을지 모르겠음.
1. WS로 CONNECT
1. 이때 websocket publisher와 subscriber를 만들어줌 <- 채널에 대한 정보가 필요
    1. socket.handshake.query.userId; <- 로 가능

## 미비사항
1. 로그인이 분리가 안됨 <- 브라우저 한개로 해서 그런 것 같기도 함. // 세션으로 하면 같은 브라우저도 함께 사용 가능
    1. 다른 브라우저에서 쿠키가 안쌓임. 슬픔
1. 이름이 안보임
1. 채팅방 접근 권한이 주인만 있음
1. websocket 연결이 자동으로 됨
1. 브라우저 연결이 잘 안됨.
