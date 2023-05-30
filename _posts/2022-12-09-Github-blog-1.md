---
title: Github 블로그 만들기 By Chirpy, VScode - 1
author: Psmin
data: 2022-12-09 10:30:21 +0900
categories: [Project, Github]
tags: [Github, Jekyll]
---

<h1> Github Blog 생성 (+ Jekyll)</h1>
***

## Github Blog

### Github Blog란?

**Github Repository** 에 저장된 `html`, `css`, `js` 등 정적 웹 문서들을 **Github**에서 **무료**로 웹에서 볼 수 있도록 **호스팅** 서비스를 제공해주는 것 입니다.

`Jekyll`, `Hugo`등의 정적 웹 사이트 생성기와 결합하여 사용자가 편리하게 웹을 만들 수 있도록 기능을 제공합니다.

즉, **Github Blog**는 **Github**에서 제공하는 웹 호스팅 서비스로 유저가 자유롭게 웹 사이트를 운영 할 수 있는 서비스입니다.

참고로, **Github** 이외에도 여러가지 서비스가 존재합니다. ex) `GitHub`, `GitLab`, `BitBucket`

---

### Github Repository 생성

회원가입은 생략하겠습니다. (회원가입은 구글링하시면 바로 나오니 어렵지 않습니다.😊)

- 가입이 완료된 후 <kbd>New</kbd>버튼을 눌러 `Repository` (저장소)를 생성합니다.
  ![New-repo](/assets/img/github-repo-new.png){: .normal .w-80}

  ![Create-repo](/assets/img/github-repo-create.png){: .normal .w-80}

  **Repository name**에는 저장소의 이름을 지정합니다.  
   (**Owner**는 변경하지 마시고, **Repository name**항목은 **username.github.io**를 입력합니다.)

  **Public**, **Add a README file**만 체크 후 맨 아래의 **Create repository**를 클릭합니다.

- URL에 **username.github.io** 입력 시 아래 사진처럼 나온다면 성공입니다!  
  ![Page-init](/assets/img/github-page-init.png){: .normal .w-80}

- 안될 때 시도 해볼 수 있는 방법입니다.
  ![Page-setting](/assets/img/github-page-setting.png){: .normal .w-80}

  ![Page-branch](/assets/img//github-page-branch.png){: .normal .w-80}

  **Page** 카테고리에서 **Branch** 설정을 확인하시고 사진과 같이 설정 후 `SAVE` 해줍니다.  
  **Your site in live at ...**이 나타나면 페이지에 들어가셔서 확인해봅니다.  
  (오래 걸릴 수 있으니 5~10분 정도 기다렸다가 확인해보세요!)

---

## Git Clone

---

### Local repository 생성

페이지 작업을 하기 위해 **Git Repository**와 **VScode**를 연동해보겠습니다.  
 (연동없이 Github Desktop 등을 이용하셔도 됩니다.👌)  
 <br/>

- **VScode** 실행 후 <kbd>F1</kbd> -> <kbd>git clone</kbd> 클릭
- <kbd>Github에서 복제</kbd> 클릭
- 연동할 Repository의 **Web URL**을 복사해 입력합니다.  
   ![github-https](/assets/img/github-https.png){: .normal .w-80}

  **Owner/repository**로도 입력 가능합니다.
  ![github-clone](/assets/img/vscode-clone.png){: .normal .w-80}

- 연동할 Local Repository를 선택합니다.  
  ![local-repo](/assets/img/local-repo.png){: .normal .w-80}  
  선택한 폴더에서 **Git repository**와 같은 이름의 폴더가 생성되고 폴더 안에 <kbd>.git</kbd> 폴더가 생성되며 연동됩니다.

---

### index.html 만들기

- 연동된 Local repository가 VScode에 실행됩니다.  
  index.html을 만들어봅니다.

  ```html
  <html>
    <body>
      Hello, World!
    </body>
  </html>
  ```

---

### add, commit, push

<kbd>CLI환경</kbd>에서 **명령어**로 이용하는 것보다 더 간편하게 <kbd>VScode</kbd>에서 `add`, `commit`, `push`를 실행할 수 있습니다.  
 <br/>

- VScode의 왼쪽 메뉴에서 **소스 제어** 항목으로 이동합니다.  
  ![local-repo](/assets/img/vscode-source-control.png){: .normal .w-80}

- <kbd>+</kbd> 버튼으로 간단하게 변경 사항을 **스테이징** 할 수 있습니다. (`git add`와 같습니다.)  
  ![local-repo](/assets/img/vscode-git-add.png){: .normal .w-80}
- 간단한 메세지를 적고 <kbd>commit</kbd> 버튼을 클릭합니다. (`git commit`과 같습니다.)  
  ![local-repo](/assets/img/vscode-git-commit.png){: .normal .w-80}
- **commit** 후 <kbd>변경 내용 동기화</kbd> 버튼을 클릭합니다. (`git push`와 같습니다.)
  ![local-repo](/assets/img/vscode-git-push.png){: .normal .w-80}
- 첫 push 시 알림 팝업이 뜨고 <kbd>확인</kbd> 버튼을 클릭 시 push 권한 여부 확인을 위한 절차가 진행됩니다. (push 이후 페이지에 반영되기까지 시간이 걸릴 수 있습니다.)  
  ![local-repo](/assets/img/vscode-git-push-popup.png){: .normal .w-80}
- 절차 완료 후 **Web URL**에 `username.github.io` 입력 시 **Hello, World!!**가 나타난다면 성공입니다.  
  ![local-repo](/assets/img/test-page.png){: .normal .w-80}

---

## Jekyll 설치

---

### Jekyll이란?

**Jekyll**은 Github의 설립자 중 한 명인 Tom Preston-Werner가 `Ruby`언어로 개발한 프레임워크입니다.  
 템플릿, 인라인 코드, MarkDown과 같은 **동적인 구성요소**를 **정적인 웹 페이지**로 만들어주는 파싱 엔진입니다.  
 **Jekyll**에 대한 자세한 이야기는 다음에 하고 지금은 가볍게 **마크다운**을 통한 웹 사이트 생성기로 생각하면 편합니다.😃  
 <br/>

---

### Rudy 설치 - Window

`Jekyll`을 **설치**하기 위해서는 먼저 `Ruby`를 설치해야합니다.

- 먼저 [Ruby 다운로드 사이트](https://rubyinstaller.org/downloads/)에서 `Ruby+Devkit 3.X.X (x64)` 를 다운로드합니다.  
  ![Ruby-check](/assets/img/ruby-install.png){: .normal .w-75}  
  설치는 수정 없이 **Next**만 누르시면 됩니다.

- Ruby가 잘 설치됐는지 확인해봅니다.  
  ![Ruby-check](/assets/img/ruby-check.png){: .normal .w-75}

---

### Jekyll 생성 & 서버 열기

- **cmd창**에서 본인이 원하는 **로컬 저장소 디렉토리**로 이동합니다.  
  위에서 git clone경로로 진행하겠습니다.  
  (저의 경우에는 **C:\workspace\project\Psmin-be.github.io**입니다.)

  ```console
    cd C:\workspace\project\Psmin-be.github.io
  ```

  이동한 후 경로에 한글이 깨질 수 있으니 UTF-8 인코딩합니다.  
  ![cmd-chcp](/assets/img/cmd-chcp.png){: .normal .w-75}  
  `Active code page: 65001`가 보이면 정상입니다.

- 이동한 경로에서 **Jekyll, bundle** 설치합니다. 이후 **webrick**까지 설치합니다.

  ```console
    gem install jekyll bundler
    gem install webrick
  ```

  Jekyll 생성  
  생성하기 전 폴더 안의 파일들을 삭제해주세요.  
  (README.md, index.html)

  ```console
    jekyll new ./
  ```

  bundle install

  ```console
    bundle install
  ```

  jekyll 서버 열기

  ```console
    bundle exec jekyll s
  ```

  정상적으로 실행된다면 <http://127.0.0.1:4000/> 또는 <http://localhost:4000/>로 접속합니다.  
  ![welcome-jekyll](/assets/img/welcome-jekyll.png){: .normal .w-85}
