---
title: 물리적 데이터 모델링 (Mysql Workbench)
author: Psmin
data: 2023-05-08 19:01:23 +0900
categories: [Knowledge, CS]
tags: [Mysql, Workbench]
---

## 물리적 데이터 모델링

![physical-modeling](/assets/img/physical-modeling.png){: .w-80}

물리적 데이터 모델링은 사용할 데이터베이스를 선택하고 실제로 테이블을 만드는 작업을 말합니다.

MySQL Workbench에서는 ER 다이어그램을 DB 물리 스키마로 생성하는 **Forward Engineer**를 제공합니다.

---

### 사용방법

1. <kbd>File</kbd> 탭에서 <kbd>New Model</kbd>를 선택합니다.

   ![new-model](/assets/img/new-model.png){: .w-80}

2. **EER(Diagram) - ADD Diagram** 아이콘을 클릭합니다.

   ![add-diagram](/assets/img/add-diagram.png)
   {: .w-80}

   창이 열리면 간판하게 GUI 환경에서 ER 다이어그램을 그릴 수 있습니다.

   또한, 좌측의 도구를 이용해 테이블을 추가하거나 관계를 이어줍니다.

   ![eerd](/assets/img/eerd.png){: .w-80}

   <kbd>File</kbd> 탭에서 <kbd>Save Model</kbd>로 저장할 수 있습니다. (Ctrl + S 가능)

   우선은 간단한 예시로 ER 다이어그램을 하나 가져오겠습니다.

   <kbd>File</kbd> 탭에서 <kbd>Open Model</kbd>로 모델을 불러올 수 있습니다.

   ![eerd-ex-01](/assets/img/eerd-ex-01.png){: .w-80}

   해당 모델로 DB를 생성해보겠습니다.

3. <kbd>Database</kbd> 탭에서 <kbd>Forward Engineer</kbd> 클릭

4. 창이 뜨면 Mysql Server 연결 정보를 선택한 후 Next 클릭

   ![forward-engineer](/assets/img/forward-engineer.png){: .w-80}

5. Options 설정 후 Next 클릭

   ![set-option](/assets/img/set-option.png){: .w-80}

6. SQL 확인한 후 Next 큺릭

   ![eerd-sql](/assets/img/eerd-sql.png){: .w-80}

7. 생성된 스키마 확인

   ![check-schema](/assets/img/check-schema.png){: .w-80}
