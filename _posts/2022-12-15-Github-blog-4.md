---
title: Github 블로그 만들기 By Chirpy, VScode - 4
author: Psmin
data: 2022-12-15 23:58:21 +0900
categories: [Project, Github]
tags: [Markdown, Chirpy]
---

# Jekyll Blog 커스터마이징 해보기

---

## 프로필 변경

sidebar 상단의 프로필을 변경해보겠습니다.  
`_config.yml`{: .filepath}파일에는 title, tagline, avatar 항목이 있습니다.

- `title` : 블로그 제목
- `tagline` : 블로그 소개말
- `avatar` : 프로필 사진 (img파일의 경로 입력)  
  해당 항목을 바꾸면 블로그 sidebar의 프로필이 변경됩니다.

---

## favicon 변경

웹 브라우드 상단의 탭 메뉴에서 사이트 제목과 함께 뜨는 아이콘을 변경해보겠습니다.

1. 먼저 원하는 **favicon** 이미지를 선택합니다.  
   (구글에 검색하시면 무료 이미지 사이트들이 많이 있습니다.👍)
2. `assets/img/favicons/`{: .filepath} 경로로 선택한 이미지 파일을 넣어둡니다.
3. `/_includes/favicons.html`{: .filepath}파일을 열어줍니다.
4. 넣어둔 파일명과 사이즈를 확인하시고 `href`를 변경해줍니다.  
   ![favicon-link](/assets/img/favicon-link.png){: .normal .w-60}

---

## sidebar 배경 이미지 변경

**sidebar 배경 이미지**를 변경해보겠습니다.  
변경할 소스 코드 부분을 찾기 위해서 브라우저의 `개발자 모드`를 이용하겠습니다.

1. 먼저 <kbd>F12</kbd>버튼을 눌러 개발자 모드를 엽니다.
2. <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> 버튼을 누르거나 개발자 모드에서 `select element`아이콘을 클릭합니다.  
   ![Devtool-select](/assets/img/devtool-select.png){: .normal .w-50}

3. sidebar 전체를 나타내는 `<div>`태그를 찾을 수 있습니다.  
   `<div id="sidebar" ...>`  
   ![Devtool-sidebar](/assets/img/devtool-sidebar.png){: .normal .w-60}

4. 태그를 클릭하고 우측의 Styles에서 `background: var(--sidebar-bg);`를 확인 할 수 있습니다.  
   ![Devtool-sidebar-bg](/assets/img/devtool-sidebar-bg.png){: .normal .w-60}

5. `background: var(--sidebar-bg);`를 VScode에서 검색합니다.  
   ![Vscode-sidebar-bg-search](/assets/img/vscode-sidebar-bg-search.png){: .normal .w-60}

6. 파일을 열어 원하는 이미지로 변경 하거나 rgb값을 통해 색상을 변경할 수 있습니다.  
   ![Vscode-sidebar-bg](/assets/img/vscode-sidebar-bg.png){: .normal .w-60}
