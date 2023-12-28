# Desgin

## 기능
### 회원
1. 회원 가입
    - nickname
    - email
    - password
1. 로그인
    - email
    - password
- 로그인 방식
    - 쿠키/세션

### 서버
1. 생성 기능
    - 이름: unique
    - password

### 채널 
1. 생성 가능
    - 서버내 이름이 unique

### 메세지
1. 메세지 작성
    - 컨텐츠
    - 작성자 정보
        - id
        - nickname
    - 작성시간
1. 메세지 수신
    - 컨텐츠
    - 작성자 정보
        - id
        - nickname
        - server 내 구분 색깔
    - 작성 시간
1. 메세지 보존
    - 컨텐츠
    - 작성자 정보
        - id
        - nickname
    - 작성시간

## 구체적인 스펙
### IDE
- neo vim
    - 생산성을 높이는데 장점이 있다고 생각함.

### Server
- nest js
- docker compose
    - PostgreSQL
    - redis
- typescript

### front
- next js 14
    - 흥미

## 서버 스펙
### 회원
- 회원 가입
    - POST /auth/signUp
    - body
        - ```typescript
          interface ISignUpBody {
              email: string;
              nickname: string;
              password: string;
          }
          ``` 
- 로그인
    - POST /auth/signIn
    - body
        - ```typescript
          interface ISignInBody {
              email: string;
              nickname: string;
              password: string;
          }
          ```
- 업데이트
    - PUT /auth/update
    - body
         - ```typescript
           interface IAuthUpdateBody {
               nickname: string;
           }
           ```
### server
- 서버 생성
    - POST /server
    - body
        - ```typescript
          interface IServerBody {
              name: string;
              password: string;
          }
          ``` 
- 서버 조인
    - POST /server/join
    - body
        - ```typescript
          interface IServerJoinBody {
              name: string;
              password: string;
          }
          ```
### 채널
- 채널 생성
    - POST /channel
    - body
        - ```typescript
          interface IChannelBody {
              name: string;
          }
          ```
- 채널 join
    - GET /channel
    - query
        - ```typescript
          interface IChannelQuery {
              name: string;
          }
          ```
    - websocket으로 연결

### 메세지
- receive_channel
- send_channel

## 프론트 페이지
### 회원
- 로그인 페이지
    - email, password
- 회원가입 페이지
    - email, nickname, password
- 프로필 페이지
    - email, nickname, password

### 서버
- 서버 참여페이지
    - name, password

### 채널/채팅 페이지
- 채널 목록 조회
- 채널 참여

