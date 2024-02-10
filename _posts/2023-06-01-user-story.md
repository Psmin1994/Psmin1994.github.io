---
title: 사용자 스토리 (User Story) 작성법
author: Psmin
data: 2023-06-01 19:32:46 +0900
categories: [Knowledge, CS]
tags: [User Story]
---

# 애자일 방법론에서 사용자 스토리에 대해 알아보자.

---

## 페르소나 (Persona)

페르소나(persona)는 **서비스를 사용할 만한 다양한 사용자 유형들을 대표하는 가상의 인물**을 말합니다.

서비스를 개발하기전 사용자들을 이해하기 위해 사용되는데 **특정 상황이나 환경에서 사용자가 어떤 행동을 할지 예측**하기위해 설정합니다.

페르소나를 리서치를 바탕으로 설정한다면 실제 잠재 고객을 예측할 수 있습니다.

페르소나 없이도 요구 사항을 도출할 수 있지만 페르소나를 활용하면 사용자의 요구 사항을 구체적으로 볼 수 있습니다.

---

## 사용자 스토리 (User Story)

사용자 스토리는 페르소나의 관점에서 작성된 가상의 문장입니다.

보통 사용자가 원하는 것을 얻기 위해 서비스를 통해 할 수 있는 행동을 말합니다.

> 보통 아래와 같은 형태로 작성됩니다.  
> "As a (Who), I want (What), So that (Why)"

**_어떤 사람 (Who)_**은 **_어떤 행동 (What)_**을 하고 싶습니다, **_어떠한 이유 (Why)_** 때문에

간단한 예로는 **'쇼핑을 하는 사람(Who)은 구매한 제품이 도착하기 전 알림을 받기를 원합니다(What) 배송이 오면 바로 수령하기를 원하기 떄문에(Why)'** 같은 문장을 만들 수 있습니다.

---

## 내 생각

사용자의 관점에서 작성되는 문장이므로 개발자를 위해서는 작성 방향성을 **UX에 비중**을 두는 것이 좋을 것 같습니다.

이는 Who, What, Why 중 **'Why에 집중해서 작성하겠다!'**를 의미합니다.

**페르소나를 구체적**으로 잡을수록 **Why에 집중**할 수록 사용자가 보다 유용하고 편리하게 느껴지는 서비스를 제공할 수 있을 것으로 생각됩니다.

---

### Why에 집중

> 문장의 앞에 Why를 작성해보자!  
> "In order to (Why), For (Who), We Will (What)"

위의 예를 바꿔보면 **'배송된 제품을 바로 수령하기 위해(Why) 우리는 제품을 구매한 사용자에게(Who) 제품이 도착하기 전 알림을 제공할 것입니다.(What)'**로 바꿀 수 있습니다.

---

### 현재 상황 반영

개발자에 입장에서는 기능을 추가하거나 변경될 때 현 상태가 중요합니다.

아예 새로운 것을 구축할 경우 많은 작업략을 요구할 것이고 이미 어느정도 구현된 기능을 변경하거나 약간의 기능만 추가할 경우 빠르게 구현될 수 있습니다.

따라서, 기존의 것에서 **얼마나 변경되어야 하는지 알려주는 것**이 좋을 것 같습니다.

> 마지막 줄에 현 상황을 추가해보자!  
> Whereas currently

간단한 예로 **'배송된 제품을 바로 수령하기 위해(Why) 우리는 제품을 구매한 사용자에게(Who) 제품이 도착하기 전 알림을 제공할 것입니다.(What) 반면에 지금은 배송 출발 시에만 알림을 보내줍니다.'**는 기존의 알림 기능에서 배송 도착 시 알림 기능만 추가해주는 작업으로 빠르게 구현할 수 있다면 **'배송된 제품을 바로 수령하기 위해(Why) 우리는 제품을 구매한 사용자에게(Who) 제품이 도착하기 전 알림을 제공할 것입니다.(What) 반면에 지금은 제품 배송에 대해 아무런 알림이 없습니다.'**는 알림 기능을 새로 구현해야하므로 작업량이 많을 것을 예상할 수 있습니다.

초안 개발 단계에서는 생략 가능합니다.

---