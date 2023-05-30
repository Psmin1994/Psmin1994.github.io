---
title: Github 블로그 만들기 By Chirpy, VScode - 3
author: Psmin
data: 2022-12-14 10:30:21 +0900
categories: [Project, Github Page]
tags: [Markdown, Syntax]
---

# 첫 포스팅 작성해보기

---

## Markdown

---

## Markdown이란?

> 일반 텍스트 기반의 경량 마크업 언어다. 일반 텍스트로 서식이 있는 문서를 작성하는 데 사용되며, 일반 마크업 언어에 비해 문법이 쉽고 간단한 것이 특징이다. HTML과 리치 텍스트(RTF) 등 서식 문서로 쉽게 변환되기 때문에 응용 소프트웨어와 함께 배포되는 README 파일이나 온라인 게시물 등에 많이 사용된다. <출처 - 위키백과>

---

## 마크다운 문법 (syntax)

---

### 제목 header

```md
# 제목 1

## 제목 2

### 제목 3

#### 제목 4

##### 제목 5

###### 제목 6
```

<h1 data-toc-skip>H1 - heading</h1>
<h2 data-toc-skip>H2 - heading</h2>
<h3 data-toc-skip>H3 - heading</h3>
<h4 data-toc-skip>H4 - heading</h4>
<br/>

---

### 줄바꿈 Line Breaks

```md
<br/> 또는 띄어쓰기 2번
```

---

### 인용구 BlockQuote

<kbd>></kbd> 블럭인용문자를 사용한다.

```md
> This is a First blockquote
>
> > This is a Second blockquote
> >
> > > This is a Third blockquote
```

> This is a First blockquote
>
> > This is a Second blockquote
> >
> > > This is a Third blockquote

---

### 목록 List

- 순서있는 목록 Ordered list

```md
1.  Firstly
2.  Secondly
3.  Thirdly
```

1.  Firstly
2.  Secondly
3.  Thirdly

- 순서없는 목록 Unordered list

```md
- Chapter
  - Section
    - Paragraph
```

- Chapter
  - Section \* Paragraph  
    <br/>
- 체크리스트 Task list

```md
- [ ] Job
  - [x] Step 1
  - [x] Step 2
  - [ ] Step 3
```

- [ ] Job
  - [x] Step 1
  - [x] Step 2
  - [ ] Step 3

---

### 설명문 description

```md
chirpy
: 쾌활한
```

chirpy
: 쾌활한

---

### 프롬프트 창 Prompts

```md
{: .prompt-tip }

> An example showing the `tip` type prompt.

{: .prompt-info }

> An example showing the `info` type prompt.

{: .prompt-warning }

> An example showing the `warning` type prompt.

{: .prompt-danger }

> An example showing the `danger` type prompt.
```

{: .prompt-tip }

> An example showing the `tip` type prompt.

{: .prompt-info }

> An example showing the `info` type prompt.

{: .prompt-warning }

> An example showing the `warning` type prompt.

{: .prompt-danger }

> An example showing the `danger` type prompt.

---

### 테이블 Tables

```md
| Company                      | Contact          | Country |
| :--------------------------- | :--------------- | ------: |
| Alfreds Futterkiste          | Maria Anders     | Germany |
| Island Trading               | Helen Bennett    |      UK |
| Magazzini Alimentari Riuniti | Giovanni Rovelli |   Italy |
```

| Company                      | Contact          | Country |
| :--------------------------- | :--------------- | ------: |
| Alfreds Futterkiste          | Maria Anders     | Germany |
| Island Trading               | Helen Bennett    |      UK |
| Magazzini Alimentari Riuniti | Giovanni Rovelli |   Italy |

---

### 링크 Links

```md
<http://127.0.0.1:4000>
```

<http://127.0.0.1:4000>

---

### 각주 Footnote

```md
Click the hook will locate the footnote[^footnote], and here is another footnote[^fn-nth-2].
```

Click the hook will locate the footnote[^footnote], and here is another footnote[^fn-nth-2].

---

### 이미지 Images

- Default (with caption)

```md
![Desktop View](/assets/img/img-sample.png){: width="972" height="589" }
```

![Desktop View](/assets/img/img-sample.png){: width="972" height="589" }
_Full screen width and center alignment_

- Shadow

```md
![Window shadow](/assets/img/img-sample.png){: .shadow width="1548" height="864" .w-75 }
```

![Window shadow](/assets/img/img-sample.png){: .shadow width="1548" height="864" .w-75 }
_shadow effect (visible in light mode)_

- Left aligned

```md
![Desktop View](/assets/img/img-sample.png){: .w-75 .normal}
```

![Desktop View](/assets/img/img-sample.png){: .w-75 .normal}

- Float to left

```md
![Desktop View](/assets/img/img-sample.png){: width="972" height="589" .w-50 .left}
```

![Desktop View](/assets/img/img-sample.png){: width="972" height="589" .w-50 .left}
Praesent maximus aliquam sapien. Sed vel neque in dolor pulvinar auctor. Maecenas pharetra, sem sit amet interdum posuere, tellus lacus eleifend magna, ac lobortis felis ipsum id sapien. Proin ornare rutrum metus, ac convallis diam volutpat sit amet. Phasellus volutpat, elit sit amet tincidunt mollis, felis mi scelerisque mauris, ut facilisis leo magna accumsan sapien. In rutrum vehicula nisl eget tempor. Nullam maximus ullamcorper libero non maximus. Integer ultricies velit id convallis varius. Praesent eu nisl eu urna finibus ultrices id nec ex. Mauris ac mattis quam. Fusce aliquam est nec sapien bibendum, vitae malesuada ligula condimentum. Phasellus a tortor aliquam, tristique felis sit amet, elementum enim. Integer vestibulum vitae nulla nec pretium.

<br/>

- Float to right

```md
![Desktop View](/assets/img/img-sample.png){: width="972" height="589" .w-50 .right}
```

![Desktop View](/assets/img/img-sample.png){: width="972" height="589" .w-50 .right}
Praesent maximus aliquam sapien. Sed vel neque in dolor pulvinar auctor. Maecenas pharetra, sem sit amet interdum posuere, tellus lacus eleifend magna, ac lobortis felis ipsum id sapien. Proin ornare rutrum metus, ac convallis diam volutpat sit amet. Phasellus volutpat, elit sit amet tincidunt mollis, felis mi scelerisque mauris, ut facilisis leo magna accumsan sapien. In rutrum vehicula nisl eget tempor. Nullam maximus ullamcorper libero non maximus. Integer ultricies velit id convallis varius. Praesent eu nisl eu urna finibus ultrices id nec ex. Mauris ac mattis quam. Fusce aliquam est nec sapien bibendum, vitae malesuada ligula condimentum. Phasellus a tortor aliquam, tristique felis sit amet, elementum enim. Integer vestibulum vitae nulla nec pretium.

---

### 강조 Emphasis

```md
_기울어진 글씨_
**굵은 글씨**
**_굵은 기울어진 글씨_**
~~취소선 글씨~~
```

_기울어진 글씨_  
 **굵은 글씨**  
 **_굵은 기울어진 글씨_**  
 ~~취소선 글씨~~

---

### 인라인 코드 Inline code

This is an example of `Inline Code`.

---

### 파일경로 Filepath

```md
Here is the `/path/to/the/file.extend`{: .filepath}.
```

Here is the `/path/to/the/file.extend`{: .filepath}.

---

### 코드 블록 Code block

- Common

```md
This is a common code snippet, without syntax highlight and line number.
```

---

- Console

```console
 $ env |grep SHELL
 SHELL=/usr/local/bin/bash
 PYENV_SHELL=bash
```

---

- Shell

```bash
   if [ $? -ne 0 ]; then
       echo "The command was not successful.";
       #do the needful / exit
   fi;
```

---

- Specific filename

```sass
@import
  "colors/light-typography",
  "colors/dark-typography"
```

{: file='\_sass/jekyll-theme-chirpy.scss'}

---

### 각주 돌아가기 기능 Reverse Footnote

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
