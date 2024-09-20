# 🔑 PAM 모듈로 비밀번호 제약 조건 설정

## 📎 개요

Ubuntu 환경에서 PAM(Pluggable Authentication Module)을 사용하여 유저의 비밀번호를 8자리 이상으로 규제하는 방법에 대해 알아보겠습니다.
> PAM이란?
> 
> 리눅스 시스템에서 사용하는 '인증 모듈(Pluggable Authentication Modules)'로써 응용 프로그램(서비스)에 대한 사용자의 사용 권한을 제어하는 모듈

## 🔧 절차

### 1️⃣ `pam_pwquality` 모듈 설치

`pam_pwquality` 모듈을 통해 비밀번호 제약조건을 설정할 수 있습니다.

이를 위해 모듈을 먼저 설치합니다.

```bash
$ sudo apt update
$ sudo apt install libpam-pwquality
```

### 2️⃣ **PAM 설정 파일 수정**

PAM 설정 파일인 `/etc/pam.d/common-password` 파일을 수정하여 비밀번호 정책을 추가할 수 있습니다.

다음 명령으로 파일을 엽니다.

`password    requisite     pam_pwquality.so retry=3` 이라고 적힌 라인을 찾습니다.  
이후 `minlen=8` 옵션을 추가합니다.


```bash
$ sudo vi /etc/pam.d/common-password
# ...
password    requisite    pam_pwquality.so retry=3 minlen=8
```

`minlen=8`은 최소 비밀번호 길이를 8자로 설정하는 옵션입니다.

### ⚠️ option 정리

`pam_pwquality.so`의  옵션에는 다양한 종류가 있습니다.

1. `minlen` : 비밀번호의 최소 길이 지정
2. `dcredit` : 비밀번호의 숫자가 포함될 때 요구되는 최소 숫자 개수
   - 음수로 설정하면 숫자가 포함되도록 강제
   - 양수로 설정하면 숫자를 제한
3. `ucredit` : 요구되는 최소 대문자 개수
4. `lcredit` : 요구되는 최소 소문자 개수
5. `minclass` : 비밀번호에 포함되어야 할 서로 다른 문자 클래스(대문자, 소문자, 숫자, 특수문자)


### 3️⃣ 설정 확인 및 테스트

`passwd` 명령어를 통해 사용자의 비밀번호를 변경할 시 제약 조건을 확인할 수 있습니다.

혹은 새로운 계정을 생성하여 비밀번호를 생성할 때도 제약 조건 테스트가 가능합니다.

테스트 계정을 생성하기 위해 `adduser` 명령어를 사용하겠습니다.

```bash
$ sudo adduser user01

Adding user `user01' ...
Adding new group `user01' (1001) ...
Adding new user `user01' (1001) with group `user01' ...
Creating home directory `/home/user01' ...
Copying files from `/etc/skel' ...
New password:
BAD PASSWORD: The password is shorter than 8 characters
```

비밀번호로 8자리 미만 문자열(1q2w3e)을 설정하려고 시도했을 때 8자리 미만이라는 오류가 발생합니다.
