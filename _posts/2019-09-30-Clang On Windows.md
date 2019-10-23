---
category: post
tags: clang,vscode
comments: true
---
## 들어가며
본 문서는 C++2A 이상을 지원하는 빌드환경 구성이 목적이며 그 중 Visual Studio Code 와 Clang 9.0 을 이용하는 방법을 소개합니다.


## 환경
에디트 툴 : Visual Studio Code - v1.39.0 [(다운로드)](https://code.visualstudio.com/download)<br>
컴파일러 / 디버거 : LLVM 9.0 [(다운로드)](https://releases.llvm.org/download.html)

## 기본 환경 구축

### 1. Visual Studio Code 설치
[(다운로드)](https://code.visualstudio.com/download) 페이지에 접속하여 최신 버전의 Visual Studio Code를 설치합니다.

### 2. LLVM 9.0 설치
[(다운로드)](https://releases.llvm.org/download.html) 페이지에 접속하여 LLVM 9.0(이상) 을 설치합니다.

### 3. C/C++ 개발을 위한 Visual Studio Code 플러그인 설치
Visual Studio Code 를 설치하였으면 아래의 플러그인을 설치합니다. (걍 install 누르시면 됩니다.)
CTRL + SHIFT + X 를 눌러 마켓플레이스 검색 창을 연 뒤, "C/C++" 을 검색하면 나옵니다.
# ![plugin](/assets/img/vscode_plugin.png)

## 실제 프로젝트 생성

### 1. 디렉토리 만들고 Visual Studio Code 로 폴더 열기
# ![open_folder](/assets/img/open_folder.png)
적당한 곳에 프로젝트를 위한 폴더를 하나 만들고,  Visual Studio Code의 File→Open Folder 로 엽니다. 

Visual Studio Code는 Folder 를 하나의 프로젝트 단위처럼 인식하기 때문에 Open Folder로 Working Directory 를 지정하지 않으면 이후 작업에 영향을 미치니 폴더 열기를 해주세요.

### 2. C/C++ Configurations 설정하기 (IntelliSense 구성 설정)
Visual Studio Code 에서 CTRL+SHIFT+P 를 눌러 팔레트를 엽니다.

팔레트에 "C/C++" 로 검색하여 C/C++:Edit Configurations (JSON) 을 열어줍니다.
# ![select_conf](/assets/img/select_conf.png)
여는 즉시 워크스페이스의 .vs_code 폴더 안에 c_cpp_properties.json 이라는 파일이 생성되며 대충 기본적인 데이터가 들어있게 됩니다.
해당 내용을 수정하여 아래처럼 수정합니다.

```json
c_cpp_properties.json

{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "C:/Program Files/LLVM/include/llvm-c",
                "C:/Program Files/LLVM/include/clang-c"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "8.1",
            "compilerPath": "\"C:/Program Files/LLVM/bin/clang-cl.exe\"",
            "cStandard": "c11",
            "cppStandard": "c++20",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}

```

### 3. C/C++ Tasks 설정하기 (빌드 설정)
Visual Studio Code 에서 CTRL+SHIFT+P 를 눌러 팔레트를 엽니다.<br>
팔레트에 "tasks" 를 검색하여 Configure Default Build Task 를 실행합니다.
# ![tasks](/assets/img/tasks.png)
이후에 또다른 선택 창이 나올 것인데 우리는 clang-cl 을 사용하고 있으므로 아래와 같이 clang-cl.exe build active file을 선택해줍니다. 
# ![build](/assets/img/build.png)
여는 즉시 워크스페이스의 .vs_code 폴더 안에 tasks.json 이라는 파일이 생성되며 기본적으로 뭔가가 들어가있습니다. 쓸모 없으니 아래처럼 바꿔주도록 합니다.

``` json
tasks.json

{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "clang-cl.exe build active file",
            "command": "C:/Program Files/LLVM/bin/clang-cl.exe",
            "args": [
                "example_1.cpp",
                "/Z7",
                "/MDd",
                "/W4",
                "/EHsc",
                "/std:c++2a",
                "/Od",
                "/o",
                "example_1.exe",
                "/link",
                "User32.lib"
            ],
            "group": "build"
        }
    ]
}

```

참고로 args 에는 빌드에 들어가는 파라미터 값들입니다. 위의 내용은 Debug 모드이며 Release 설정을 원하시면 참고사이트의 www.40tube.fr 블로그를 참고하시기 바랍니다.

또 입력 args 의 0번째는 소스파일이 들어가게 되므로, 소스파일이 아직 없다면 나중에 채워넣으시면 됩니다. (당연하지만)

### 4. 소스파일 생성
아무거나 일단 소스파일 만들어주세요. (디버깅 설명해야하니까..)

### 5. 빌드하기
3번 Task 설정에서 tasks.json 의 args 에 소스파일 이름을 입력해줍니다. 이후 CTRL+SHIFT+B 를 입력하여 빌드할 수 있습니다.
# ![build_console](/assets/img/build_console.png)

### 6. 디버깅 설정하기
빌드까지 완전하게 되었다면, 디버깅을 설정할 수 있습니다. 디버깅 설정화면으로 들어가기 위해서는 F5 키를 누르면 되고 확인을 누르는 순간 launch.json 이라는 파일이 워크스페이스의 .vs_code 아래 생성되게 됩니다.

기본적으로 많은 내용이 비어있을텐데 아래처럼 입력해 줍니다.

``` json
{
    launch.json

    "version": "0.2.0",
    "configurations": [
      {
        "name": "(Windows) Launch",
        "type": "cppvsdbg",
        "request": "launch",
        "program": "${workspaceFolder}/example_1.exe", 
        "args": [],
        "stopAtEntry": false,
        "cwd": "${workspaceFolder}",
        "environment": [],
        "externalConsole": true
      }
    ]
  }
```

### 7. 디버깅하기
6의 디버깅 설정이 끝났으면 소스 내에 Break Point 를 설정하고 다시 F5 를 눌러봅니다.<br>
아래와 같이 디버깅이 잘 먹히는 것을 볼 수 있습니다.
# ![debug](/assets/img/debug.png)
기본적인 단축키는 Visual Studio 와 동일하고, Call Stack, 조사식 등의 대부분의 기능을 이용할 수 있습니다.

아마 제한사항이 있을 것 같은데 관련 내용은 알게되는 대로 업데이트 하도록 하겠습니다.

---

참고 사이트

https://code.visualstudio.com/docs/cpp/launch-json-reference

https://code.visualstudio.com/docs/cpp/cpp-debug

https://www.40tude.fr/blog/compile-cpp-code-with-vscode-clang/ ← 실은 여기 다있음
