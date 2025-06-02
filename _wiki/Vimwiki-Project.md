---
layout  : wiki
title   : Vimwiki-Project
summary : fork한 Vimwiki를 커스터마이징해 보자. 
date    : 2022-12-12 08:23:58 +0900
updated : 2023-09-06 16:20:55 +0900
tag     : vimwiki project
resource: 3b/b4eb4a-08a4-4fa8-a4be-3554856f4561
toc     : false
public  : true
parent  : [[Project]]
latex   : false
---
* TOC
{:toc}

# 개요
이 위키는 John Grib님의 [Vimwiki skeleton repository](https://github.com/johngrib/johngrib-jekyll-skeleton){:target="_blank"} 를 fork하여 만들었다. 이 위키를 커스터마징해보자.

# 목표
* 현재 John Grib님이 운영하고 있는 위키에 추가된 기능을 적용해 본다.
* 나만의 스타일(디자인?)도 조금씩 적용해 본다.

# TIL
* rbenv를 이용하여 Ruby를 설치하는 경우
    * Ruby 버전 설치 `rbenv install 2.7.4`
    * Ruby 설치 후 재실행 `rbenv rehash`
    * Ruby 버전 변경
        * 시스템 전체 지정한 버전 사용 `rbenv global 2.7.4`
        * 현재 디렉토리만 사용 `rbenv local 2.7.4`
    * Ruby 버전확인 `rbenv versions`
    * Ruby 버전삭제 `rbenv unistall 2.7.4`

# To-do
* [O] 현재 운영중인 위키에 추가된 기능 중 적용해 볼 것들
    * [X] (2022-12-12) 외부 링크를 표시하는 방법
    * [X] (2012-12-12) 주석을 다는 부분
    * [X] (2022-12-21) 2022-12-07에 업데이트된 skeleton 내용 확인 후 적용
    * [ ] resource 경로에서 엔터를 칠 경우 해당 경로에서 Finder(윈도우의 경우 탐색기) 열기
    * [X] (2023-01-05) 댓글 기능 utterances에서 giscus로 교체
    * [X] (2023-01-07) 문서 내 랜덤링크 열어주는 기능

* [X] 나만의 스타일로 구현할 부분
    * [X] (2012-12-12) tag에 배경색을 입혀서 텍스트와 구분해 보면 어떨까.
    * [X] (2012-12-14) responsive top navigation 적용
        * w3schools.com에 있는 예제를 참고했는데 a 태그를 이용했더니 (href="javascript:void(0)") 외부링크로 인식하여 index화면에서 external-link.svg가 보여지는 현상 발생함
        * 각 페이지 링크마다 <a href="javascript:void(0)..."가 생성되 지저분해짐
        * a 태그를 버리고 button 태그로 다시 구현했더니 모든 증상 해결됨   
    * [X] (2022-12-17) 검색창 상단 메뉴바 안으로 이동해보면 어떨까.
        * bootstrap의 도움을 받아 성공
    * [X] (2022-12-17) 메인 화면을 좀더 심플하게 바꿔보자.
        * 아이콘 변경, 색상변경, 여백 조정 등 기본 세팅 마무리

# 자료

## Web

* John Grib, [리스트를 테이블로 변환하는 기능](https://johngrib.github.io/wiki/blog/this/table-generate/) 
