---
title: 해시 알고리즘
author: Psmin
data: 2024-02-22 23:07:02 +0900
categories: [Knowledge, Nodejs]
tags: [scrypt, bcrypt, PBKDF2]
---

# 해시 알고리즘에 대해 알아보자.

---

## bcrypt

1999년 USENIX에서 발표한 <u>Blowfish 암호에 기반을 둔 해시 함수</u>로 되었으며 OpenBSD 및 수세 리눅스 등의 일부 리눅스 배포판을 포함한 기타 시스템용 기본 암호 해시 함수 입니다.

SHA 계열에서 약점으로 지적된 빠른 연산으로 인한 `Brute force 공격`에 대한 대비책으로 <u>Blowfish의 특징 중 하나인 key setup phase 라는 일종의 막대한 전처리 요구(Salting)로 느리게 만들고 반복횟수(Key Stretching)를 변수로 지정가능하게 하여 작업량을 조절</u>할 수 있게 하였습니다.

bcrypt는 입력 값으로 72 bytes character를 사용해야 하는 단점이 있습니다.

이후에 설명할 PBKDF2나 scrypt는 이러한 제약이 없습니다.

---

## PBKDF2 (Password-Based Key Derivation Function)

미국 **NIST**에서 승인받은 사용자 패스워드를 기반으로 키(Key) 유도를 하기 위한 함수입니다.

Bcrypt와 유사하게 사용자 패스워드에 해시함수, Salt, 반복 횟수 등을 지정하여 패스워드에 대한 Digest를 생성합니다.

PBKDF2는 아주 가볍고 구현하기 쉬우며, SHA와 같이 검증된 해시 함수만을 사용합니다.

---

## scrypt

PBKDF2와 유사한 adaptive key derivation function으로 Colin Percival이 2012년 9월 17일 설계했습니다.

DIGEST를 생성할 때 메모리 오버헤드를 갖도록 설계되어, brute force 공격을 시도할 때 병렬화 처리가 매우 어렵게 되어 있습니다.

<u>PBKDF2보다 안전하다고 평가</u>되며 미래에 bcrypt에 비해 더 경쟁력이 있다고 여겨지지만 더욱 많은 메모리를 사용해야 한다는 단점이 있습니다.

보안에 아주 민감한 사용자들을 위한 백업 솔루션을 제공하는 Tarsnap에서도 사용하고 있습니다.

---

## 정리

- <u>공인된 기준 ISO-27001 준수하려는 경우</u> - `PBKDF2`을 사용

- <u>더 강력한 패스워드 다이제스트를 구현하고 싶은 경우</u> - `bcrypt`를 사용

- <u>구현하는 시스템이 민감한 정보를 다루고, 보안 시스템을 구현하는데 많은 비용을 투자할 수 있는 경우</u> - `scrypt`를 사용
