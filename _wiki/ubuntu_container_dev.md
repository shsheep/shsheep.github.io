---
layout  : wiki
title   : Ubuntu 컨테이너에 개발환경 갖추기
summary : maximal 컨테이너는 없을까
date    : 2023-08-27 13:34:05 +0900
updated : 2024-01-23 17:14:30 +0900
tag     : ubuntu container devenv
toc     : true
public  : true
parent  : 
latex   : false
resource: B079218B-68B4-4E86-A878-B34F1A72DBB5
---
* TOC
{:toc}

# Ubuntu 컨테이너에 개발환경 갖추기
## 배경
Mac(Intel, ARM 모두)에서는 GNU C/C++ 개발하기 불편하다.
기본으로 설치되거나 지원되는 C/C++가 LLVM 기반(clang,clang++)이라 회사에서 사용하는 RHEL 상의 GNU C/C++와 비슷한 기능을 공부하거나 실험해 볼 수가 없다.

clang을 깊게 사용해보지는 않았지만, LLVM 기반 `lldb`를 대충 써봤을 때 `gdb`보다 좀 더 사용자 친화적인 것 같아 마음이 가기도 했다.
하지만 C/C++ 프로그램을 개발해서 프로덕션에서 운영하게 되는 일은 왠지 대부분 Mac 같은 BSD 기반 리눅스에서가 아니라, 레드햇이나 데비안 계열의 리눅스에서 GNU C/C++ 기반에서 훨씬 더 자주 일어날 것 같다(clang 기반의 C/C++ 프로그램을 운용하는 곳이 있을지 궁금하다)(clang이냐 gcc이냐를 따지기 전에 C/C++ 자체가 도메인을 강하게 타는 언어라 이런 물음에 대한 답은 정해져 있을지 모른다).

때문에 GNU C/C++ 개발을 하고 테스트를 해볼 수 있는 환경을 로컬 Mac 상에서 컨테이너로 구축하려고 한다. VM을 사용하면 더 쉽고 빠르게 구축할 수도 있겠지만, Mac에서 사용할 수 있는 VM이 뭐가 있는지는 잘 모르겠다. Paralles가 쓸만해 보이던데 유료였다. 컨테이너 사용에 익숙해지도록 도커로 구축해보려 한다.

## Draft
실제 코드 작성과 프로그램 테스트를 진행하기 전까지 했던 모든 활동을 우선 이곳에 순차적으로 기록한다.
그리고 갈무리해서 GNU C/C++ 개발 도커 이미지를 만들어낸다!
1. 우분투 LTS(22.04 버전) 도커 컨테이너 이미지 다운로드 및 기동
   ```
   docker run -it ubuntu
   ```
   이 우분투 이미지는 클라우드 환경에서 빠르고 가볍게 돌아가기 위해 자체 크기를 경량화한 minimal 버전이다.
   때문에 일반적인 수준의 기능이나 프로그램이 제공되지 않기 때문에 나처럼 개발 환경으로서 사용하려면 필요한 기능들을 설치하고 구성하는 과정이 필요하다.
2. 필요한 패키지를 빠르게 다운받기 위해 `apt` 저장소의 미러를 국내 미러 서버로 바꿔준다. 이 단계에서는 `vi`. `vim`이나 `nano` 같은 텍스트 에디터도 깔려있지 않은 상태라 `sed`로 저장소 미러 주소를 바꿔준다.
   ```
   sed -i -e 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list'
   sudo apt update
   ```
3. 그리고 현재 우분투의 minimal 설정을 해제. 이 과정에서 약간의 패키지들이 설치되는 것 같다.
   ```
   unminimize
   ```
4. 그리고 나서 필요한 패키지들을 깔아준다. 내가 개인적으로 필요해서 설치한 프로그램은 아래와 같다(나열 순서가 중요할까?)
   ```
   apt install build-essential manpages-dev coreutils sudo man-db iproute2 htop sqlite sqlite3 wget curl zip \
       vim python3 python3-dev gdb cscope git \
       tree \
       delta bat rg
   ```
   - 프로그램별 deprecated 여부와 확장성 있는 최신 프로그램 추적이 진행되면 좋을 것 같다(ex. `netstat`, `ifconfig` 대신 `ss`, `ip` 명령어 설치 및 사용)(https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/)
5. 실제 사용할 사용자 계정 생성 및 sudo 권한 부여, 비밀번호 설정
   ```
   useradd -s /usr/bin/bash -d /home/shsheep/ -m -G sudo shsheep
   passwd shsheep
   ```
6. `bash` 및 `vim` 개인환경 설정
   ```
   # vim-plug 설치
   curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   # dotfiles 구성
   mkdir ~/Workspace
   cd ~/Workspace && git clone https://github.com/shsheep/dotfiles.git
   cd ~ && ln -s ~/Workspace/dotfiles/.bash_profile .bash_profile && ln -s ~/Workspace/dotfiles/my.vimrc .vimrc
   ```
7. dependency 프로그램, 라이브러리 설치
   ```
   # nodejs, npm 설치
   mkdir -p ~/.local/bin
   # Oracle DB dependency 설치
   sudo apt install libaio1
   ```
   
## Dockerfile
위 내용들로 Dockerfile을 작성하고 이미지를 빌드하자.
> 도커파일 작성시 유의점
> https://docs.docker.com/engine/reference/run/#env-environment-variables
> 도커는 $HOME, $HOSTNAME, $PATH, $TERM 네 개의 환경변수만 디폴트로 지정해준다. $USER 같은 값은 직접 `ENV`나 `-e` 등으로 지정해주어야한다.

```
* 커맨드 수정
    * (예시) `$ md5sum *.jg` 입력했을 때(jpg를 jg로 잘못 입력) `No such file or directory` 오류 발생
    * 다음 명령어로 `$ ^jg^jpg`로 입력하면 `$ md5sum *.jpg`로 실행됨
    * `s/(source)/(target)`을 활용할 수도 있음 `$ !!:s/jg/jpg/` 또는 `$ !md5sum:s/jg/jpg/`
```
