---
layout  : wiki
title   : Golang HTTP 서버 + DB 사용해보기
summary : 
date    : 2025-02-25 15:18:30 +0900
updated : 2025-03-13 20:37:03 +0900
tag     : 
toc     : true
public  : true
parent  : 
latex   : false
resource: F67D01DC-50BB-414C-9D49-972D0918D6A1
---
* TOC
{:toc}

# 베슬 AI 과제 면접 준비
## 배경
베슬 AI 코테는 만점으로 통과하고(쉬웠음) 이제 면접 전형에 들어가는데 면접 내용이 개인 개발환경 준비해와서 요구사항에 맞는 HTTP API 서버 구현하는 것이라고 함. 그래서 개발환경 미리 설정해놓고 면접 준비할 겸 대략적인 HTTP 서버 시나리오 미리 구성해보고 구현까지 한번 해보려 한다.

## 개발환경
1. Vim - vim-go, vimspector, dadbod
2. Go 스탠다드 net/http 라이브러리
3. Gorilla Mux
4. Go Redis client
5. Gorm
6. Redis
7. PostgreSQL
8. Docker

## 시나리오
1. [X] 로깅은 stdout(log)로 사용
2. [X] 데이터 구조는 Person{ID:int, Name:string}
2. [X] 라우터 미들웨어에 HTTP 요청 타임아웃 반영
3. [X] 라우터 미들웨어에 requestId 생성해서 context로 넘기는 로직 반영
4. [X] POST 요청 받아서 redis와 postgresql에 write
5. [X] GET 요청 시 해당하는 정보를 redis와 postgresql에서 찾아서 반환
6. [ ] RDB I/O 타임아웃 반영
7. [ ] json 파일 파싱해서 POST insert하기
8. [ ] 에러 정의
9. [ ] http 요청하는 클라이언트
