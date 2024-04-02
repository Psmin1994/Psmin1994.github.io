---
title: 테이블을 ERD로 변환
author: Psmin
data: 2023-05-11 19:01:23 +0900
categories: [Knowledge, CS]
tags: [Mysql, Workbench]
---

MySQL Workbench에서는 기존의 DB 스키마에서 ER 다이어그램을 추출하는 **Reverse Engineer**를 제공합니다.

## ER 다이어그램 생성방법

1. 상단 탭의 <kbd>Database</kbd>에서 <kbd>Reverse Engineer...</kbd> 클릭

   ![workbench-erd-01](/assets/img/workbench-erd-01.png){: }

2. <kbd>Next</kbd> 클릭

   ![workbench-erd-02](/assets/img/workbench-erd-02.png){: }

   ![workbench-erd-03](/assets/img/workbench-erd-03.png){: }

3. 사용할 SCHEMA 선택 후 <kbd>Next</kbd> 클릭  
   (예시로 test 스키마를 사용해보겠습니다.)

   ![workbench-erd-04](/assets/img/workbench-erd-04.png){: }

   ![workbench-erd-05](/assets/img/workbench-erd-05.png){: }

4. <kbd>Execute</kbd> 클릭  
   (스키마에 테이블이 존재할 경우 가져올 테이블을 선택할 수 있습니다.)

   ![workbench-erd-06](/assets/img/workbench-erd-06.png){: }

5. <kbd>Next</kbd> 클릭

   ![workbench-erd-07](/assets/img/workbench-erd-07.png){: }

6. <kbd>Finish</kbd> 클릭

   ![workbench-erd-08](/assets/img/workbench-erd-08.png){: }

7. 사진과 같이 빈 다이어그램 창이 뜨면 성공입니다.

   ![workbench-erd-09](/assets/img/workbench-erd-09.png){: }

8. 좌측 도구를 선택해 테이블을 생성하거나 관계를 설정할 수 있습니다.

   ![workbench-erd-10](/assets/img/workbench-erd-10.png){: }

9. 테이블을 생성할 때는 상단 탭에서 원하는 스키마를 지정한 후 생성합니다.

   ![workbench-erd-11](/assets/img/workbench-erd-11.png){: }
