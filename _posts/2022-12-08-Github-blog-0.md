---
title: Github 블로그 만들기 By Chirpy, VScode - 0
author: Psmin
data: 2022-12-08 20:30:31 +0900
categories: [Github, blog]
tags: [Git, VScode]
---

# Git, VScode 설치 (+ Git Bash)

---

## Git 설치

---

### GIt이란?

**형상 관리 도구 (Configuration Management Tool)** 중 하나입니다.  
기본 기능으로는 **현재 작업 내역**, **과거 작업 내역**, **변경점**을 확인할 수있는 것 입니다.  
SW 개발에 필요한 소스 코드를 관리할 수 있게 해주며 **Linux**를 만든 **LinusTorvlads**에 의해 만들어졌다.

---

### Git 설치

- [Git 홈페이지](https://git-scm.com/)로 이동합니다.  
  홈페이지에 접속하면 아래의 사진과 같은 모습이 나옵니다. (2022-12-11 작성)  
  <br/>
  ![git-page](/assets/img/git-page.png){: .normal .w-75}  
  <br/>
  ![git-download-btn](/assets/img/git-page-download.png){: .normal .w-75}  
  <kbd>Download for Windows</kbd> 버튼을 눌러 **Download 페이지**로 넘어갑니다.  
  <br/>
  ![git-download-page](/assets/img/git-page-download-2.png){: .normal .w-75}  
  <kbd>CLick here to download</kbd> 버튼을 눌러 진행합니다.  
  <br/>
- Git 설치 프로그램  
  대부분 Default로 수정없이 진행하시고 아래 항목만 수정해주시면 됩니다.  
  <br/>
  ![git-editor](/assets/img/git-default-editor.png){: .normal .w-75}  
  <br/>
  저는 **VScode**를 주로 사용할 것이므로 **VScode**를 기준으로 설치를 진행한다.  
  (`Next` 클릭)
  <br/>
  ![git-editor](/assets/img/git-branch-name.png){: .normal .w-75}  
  <br/>
  분기 이름을 **master**로 할 것인지 묻습니다.  
  따로 이름을 지정하려면 두번째 항목을 선택하고 입력해줍니다.  
  (초보자분들은 `Let Git decide` 선택하시는 것을 추천드립니다.)  
  <br/>
  ![git-editor](/assets/img/git-path-enviroment.png){: .normal .w-75}  
  <br/>
  **Bash환경**에서만 사용할 지, **Unix환경**에서 Command Prompt를 사용할 지 묻는 항목입니다.  
  **Git 환경변수**를 이용할 것이기 때문에 2번째 항목을 선택합니다.  
  (**Window**환경에서는 2번이 추천됩니다.)  
  <br/>
  **cmd**창에 `git --version` 입력 시 버전이 잘 나온다면 성공입니다.
  ![git-check](/assets/img/git-check.png){: .normal .w-75}  
  <br/>

---

## VScode 설치

---

### VScode란?

**마이크로소프트(Microsoft)**에서 만든 소스 코드 에디터입니다.  
소스 코드 에디터란 컴퓨터 프로그램의 소스 코드를 편집하기 위해 설계된 프로그램을 말합니다.  
대표적으로 **Visual Studio Code**, **IntelliJ**, **Brackets**, **Eclipse** 등이 있습니다.

**VScode (Visual Studio Code)** 는 대부분의 운영체제를 모두 지원하며, **무료**로 사용할 수 있기 때문에  
실무에서 많이 사용되고 있다고 합니다.

---

### VScode 설치

[VScode 홈페이지](https://visualstudio.microsoft.com/ko/)로 이동합니다.  
![vscode-page](/assets/img/vscode-page.png){: .normal .w-75}  
<br/>
아래로 스크롤을 내려서 Visual Studio Code를 찾습니다.
![vscode-download](/assets/img/vscode-page-download.png){: .normal .w-75}  
<br/>
<kbd>Visual Studio 코드 다운로드</kbd> 버튼을 눌러 운영체제에 맞는 설치 파일을 다운로드합니다.  
![vscode-download-page](/assets/img/vscode-page-download-2.png){: .normal .w-75}  
화면이 넘어가고 다운로드된 VScode 설치 파일을 진행해줍니다.  
<br/>
대부분 수정없이 진행해주시고 중간에 `PATH에 추가 항목`을 활성화해줍니다.  
![vscode-setup](/assets/img/vscode-setup-path.png){: .normal .w-75}  
<br/>

---

## 추가 작업

Git, VScode에 작업 환경을 만들어보겠습니다.  
(생략해도 괜찮습니다.😊)

---

### Git 사용자 등록

```console
   git config --global user.name "사용자이름"
   git config --global user.email "사용자이메일"
```

다음 명령어로 사용자의 이름과 이메일을 등록합니다.  
<br/>

```console
   git config --list
```

다음 명령어로 확인합니다.  
![git-list](/assets/img/git-list.png){: .normal .w-75}

---

### Git Bash 사용하기

**Git bash**란 **window**의 **cmd**, **Linux와 Mac**의 **Terminal**과 같은 역할을 하는데, 운영체제마다 사용하는 명령어가 달라서 생기는 문제를 극복하게해준다.  
즉, **Git Bash**를 이용하면 **window**에서도 **Linux**에서 사용하는 **명령어**를 사용할 수 있다.  
(Github 작업을 할 예정이기 때문에 `Github Desktop` 등의 GUI 클라이언트를 사용해도 무관하다)  
<br/>
VScode의 터미널에 **Git Bash**를 설정해서 사용하겠습니다.

1. VScode를 실행합니다.
2. <kbd>F1</kbd> 입력 -> 'settings' 입력 -> <kbd>Preferences : Open Settings (JSON)</kbd> 클릭
   ![vscode-settings](/assets/img/vscode-settings.png){: .normal .w-75}
3. `"terminal.integrated.profiles.windows"` 항목에 **Git bash**를 추가해줍니다.
   ```
   "terminal.integrated.profiles.windows": {
      "Git-bash": {
            "path": "C:\\Git\\bin\\bash.exe"
      }
   }
   ```
   **path** 에는 설치된 <kbd>Git</kbd>폴더 안의 <kbd>bin</kbd>폴더 안의 <kbd>bash.exe</kbd>파일의 경로를 입력해줍니다.  
   (Git 설치 과정에서 설치될 경로를 지정할 수 있습니다.)
4. **Git bash**를 VScode의 **기본 터미널**로 설정 가능합니다.

   ```
   "terminal.integrated.defaultProfile.windows": "Git-bash"
   ```

   터미널 변경 시 입력값은 `"terminal.integrated.profiles.windows"`항목의 입력값과 같아야합니다.  
   ![vscode-bash](/assets/img/vscode-bash-default.png){: .normal .w-75}

   <kbd>Ctrl</kbd> + <kbd>`</kbd> 단축키로 터미널을 켰을 때 bash가 뜨면 성공입니다.  
   ![vscode-bash](/assets/img/vscode-bash.png){: .normal .w-75}

---

### VScode 한국어 설정

<kbd>F1</kbd> 버튼 -> `display` 검색 -> <kbd>Configure Display Language</kbd> 클릭 -> 한국어로 바꿔줍니다.
