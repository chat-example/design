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
- 일단 없이 ㄱㄱ