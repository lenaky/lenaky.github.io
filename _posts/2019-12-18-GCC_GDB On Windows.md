---
category: post
tags: gcc,vscode
comments: true
---
## 들어가며
본 문서는 최신의 C++ Standard 를 지원하는 빌드환경 구성이 목적이고 Windows 10 에서 WSL + Ubuntu + GCC9.x + Visual Studio Code 를 이용하여 환경을 구성합니다.

## 환경
에디트 툴 : Visual Studio Code - v1.39.0 [(다운로드)](https://code.visualstudio.com/download)<br>

## 기본 환경 구축

### 1. WSL 사용

프로그램 및 기능 -> Windows 기능 켜기/끄기 -> Linux 용 Windows 하위 시스템 활성화
# ![WSL]](/assets/img/program_function_01.png)

### 2. Ubuntu 설치

Microsoft Store 앱에서 Ubuntu 찾아 설치. (다른 것 사용해도 되겠지만 테스트해보지 않았음)

설치가 되면 Ubuntu에 접속할 수 있는 터미널이 생기게 됩니다. 아래의 과정을 수행하면 됩니다.

# ![ubuntu]](/assets/img/microsoft_store.png)

```bash
$ sudo apt update
$ sudo apt install build-essential
$ sudo apt install gcc-9 g++-9
$ sudo apt install gdb
```

여기까지 하면 gcc 9 버전과 gdb 까지 설치가 됩니다.

gcc 를 선택하여 버전을 사용하고 싶다면 update-alternatives 설정을 추가하여 선택하도록 합니다.

```bash
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 70 --slave /usr/bin/g++ g++ /usr/bin/g++-7 --slave /usr/bin/gcov gcov /usr/bin/gcov-7
```
참고로, 기본으로 gcc 7 버전이 설치되어있어 위와 같이 세팅한 뒤
```bash
$ sudo update-alternatives --config gcc
There are 2 choices for the alternative gcc (providing /usr/bin/gcc).

  Selection    Path            Priority   Status
------------------------------------------------------------
* 0            /usr/bin/gcc-9   90        auto mode
  1            /usr/bin/gcc-7   70        manual mode
  2            /usr/bin/gcc-9   90        manual mode

Press <enter> to keep the current choice[*], or type selection number:
```
위에서 세팅하도록 합니다. update-alternatives 설치 시 우선순위가 가장 높은것이 자동으로 최우선 명령어에 맵핑되므로 굳이 안바꿔줘도 상관은 없습니다.

### 3. Visual Studio Code 설치
[(다운로드)](https://code.visualstudio.com/download) 페이지에 접속하여 최신 버전의 Visual Studio Code를 설치합니다.

### 4. C/C++ 개발을 위한 Visual Studio Code 플러그인 설치
Visual Studio Code 를 설치하였으면 아래의 플러그인을 설치합니다. (걍 install 누르시면 됩니다.)
CTRL + SHIFT + X 를 눌러 마켓플레이스 검색 창을 연 뒤, "C/C++" 을 검색하면 나옵니다.
# ![plugin](/assets/img/vscode_plugin.png)

### 4. WSL 연결을 위한 플러그인 설치
WSL 연결을 위한 아래의 플러그인을 설치합니다.
CTRL + SHIFT + X 를 눌러 마켓플레이스 검색 창을 연 뒤, "WSL" 을 검색하면 나옵니다.
# ![plugin2](/assets/img/WSL_Plugin.png)

## 실제 프로젝트 생성

### 1. 디렉토리 만들고 Visual Studio Code 로 폴더 열기

Visual Studio Code는 Folder 를 하나의 프로젝트 단위처럼 인식합니다. (.vscode 폴더 생성)
따라서 Ubuntu 터미널에 들어가서 작업할 디렉토리를 하나 만들고 거기서 Visual Studio Code 를 실행합니다.
# ![vscode](/assets/img/open_vscode.png)

### 3. C/C++ Tasks 설정하기 (빌드 설정)
Visual Studio Code 에서 CTRL+SHIFT+P 를 눌러 팔레트를 엽니다.<br>
팔레트에 "tasks" 를 검색하여 Configure Default Build Task 를 실행합니다.
# ![task1](/assets/img/task_create.png)
다시 선택 메뉴가 뜨면 g++ build active file을 설정해줍니다.<br>
그러면 아래처럼 .vs_code 디렉토리에 자동으로 tasks.json 파일이 생성됩니다.
# ![task1](/assets/img/task_create2.png)

### 4. 빌드하기
CTRL + SHIFT + B 버튼을 눌러 빌드하면 됩니다. 디폴트 상태의 tasks.json 으로 빌드하는 경우 바이너리는 file 명으로 생성됩니다.<br>
조금 복잡한 구조의 빌드를 하고 싶다면 tasks.json 을 좀 파보시면 될듯합니다. (저도 안해봐서 모릅니다.)

### 5. 디버깅 하기.
Visual Studio 와 같습니다. 일단 빌드하려는 소스파일에 브레이크포인트를 걸고 F5 를 눌러줍니다.
# ![debug1](/assets/img/debug1.png)
환경 선택을 위의 사진처럼 해주고 실행하면 아래처럼 디버깅을 할 수 있습니다.
# ![debug2](/assets/img/debug2.png)
# ![debug3](/assets/img/debug3.png)

기본적인 단축키는 Visual Studio 와 동일하고, Call Stack, 조사식 등의 대부분의 기능을 이용할 수 있습니다.

아마 제한사항이 있을 것 같은데 관련 내용은 알게되는 대로 업데이트 하도록 하겠습니다.

---

참고 사이트

https://code.visualstudio.com/docs/cpp/launch-json-reference

https://code.visualstudio.com/docs/cpp/cpp-debug