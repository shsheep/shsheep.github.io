---
layout  : wiki
title   : 
summary : 
date    : 2023-12-29 17:47:12 +0900
updated : 2023-12-29 20:42:30 +0900
tag     : 
toc     : true
public  : true
parent  : 
latex   : false
resource: 58BAA7B2-14DD-4022-92D3-16707A695042
---
* TOC
{:toc}

# Document AI
Upstage Document AI(docai) docs

## Serving
### 구조 분석
- Controller와 Agent 간 통신
    - 컨트롤러로 전달한 요청이 에이전트의 동작까지 이어지는 흐름 파악
        - `POST /api/agents`의 경우,
            1. 컨트롤러 기동. 기동하면서 API URL listen 시작
            2. 사용자가 에이전트와 관련된 요청 송신. 컨트롤러는 해당 요청 처리 시작.
            3. 2번 전에 에이전트가 미리 기동되어 있어야하는 것 같다. 기동되어있는 에이전트의 IP 주소와 포트를 2번 POST API의 body로 넣어주어야 한다(ex. `$ curl -X POST http://localhost:8080/api/agents -d '{"address": "1.2.3.4", "port": 12345}'`). 컨트롤러는 해당 주소와 포트로 에이전트에 그냥 GET을 날려 status 200을 확인하고 확인되었으면 캐쉬 DB에 Set한 뒤 Set한 것과 동일한 데이터를 응답으로 넘겨준다.
        - `POST /api/isvcs`를 통해 에이전트가 추론 모델을 기동하게 된다.
            1. 컨트롤러 기동. 기동하면서 API URL listen 시작. 추론 모델을 기동할 에이전트의 ID와 기동할 모델 ID를 사전에 미리 획득해놓는다.
            2. 에이전트 주소와 포트로 `POST /isvcs` 날린다.  이 떄 요청 데이터로 1의 에이전트 ID, 모델 ID 등을 포함하는 `ISVCCreateRequest`를 사용한다.
            3. 성공 시 응답으로 받은 isvc ID와 isvc 주소(?)를 응답으로 넘겨준다.
            4. 컨트롤러로부터 `POST /isvcs` 요청을 받은 에이전트는 먼저 30,000 ~ 35,000 사이의 사용가능한 임의의 포트를 찾음.
            5. 그리고 요청값에서 사용할 GPU 번호를 파싱하는데 "auto"로도 지정할 수 있다.
            6. 포트와 GPU 번호가 알맞게 정해졌다면 랜덤으로 생성된 isvc ID와 해당 isvc의 주소와 포트를 응답 데이터로 전송한다. 주소의 경우 에이전트 기동시 디폴트 12.0.0.1로 되어있는데(`cmd/agent/cmd.go:75`) 이 주소가 응답으로 오는건지 확인해봐야 한다. 먼저 응답을 하고 실제 isvc 컨테이너 생성을 고루틴으로 비동기적으로 생성한다.
            7. 
### etc
    - 컨트롤러 기동시 세션에 나오는 로그는 stderr이다. stdout은 나오지 않아서 보고싶다면 따로 리디렉션 해야한다.
