---
layout  : wiki
title   : Makefile 잘 쓰기
summary : GNU Makefile
date    : 2023-10-04 14:21:29 +0900
updated : 2023-10-04 14:48:38 +0900
tag     : 
toc     : true
public  : true
parent  : 
latex   : false
resource: ECA944E2-0437-4F20-997F-0C86AB76B486
---
* TOC
{:toc}

# Makefile 잘 쓰기
C/C++ 프로젝트 구성에 반드시 필요한 Makefile을 정리해서 필요한 내용만 필요할 때 골라서 볼 수 있도록 하자. GNU C/C\+\+말고도 MSVC를 사용할 때도 Visual Studio에서 사용하는 것 같다. 뿐만 아니라 Go나 다른 프로그래밍 언어의 빌드 구축에도 활용할 수 있<del>기는 하</del>다.

## 배경


## Default Macros
Makefile은 단계별 빌드 명령에서 수행할 절차를 작성할 수 있는데 이 과정에서 사용할 매크로(일종의 변수)를 직접 정의할 수도 있고, 혹은 기본적으로 정의되어 있는 매크로를 사용할 수도 있다. 기본적으로 정의되어 있는 매크로들이 굉장히 많아서 GNU C/C\+\+ 개발에 활용할 수 있는 매크로만 추려보았다(`make -p`로 환경변수를 포함한 전체 Makefile 매크로 구성을 확인할 수 있다.)
```Makefile

```

- **이런 디폴트 매크로가 신경쓰인다면 `-R` 혹은 `--no-builtin-varialbes` 명령줄 아규먼트로 디폴트 매크로를 해제할 수 있다.**
