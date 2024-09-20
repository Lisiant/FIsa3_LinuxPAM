# 🔑 PAM 모듈로 비밀번호 제약 조건 설정

## 📎 개요

Ubuntu 환경에서 PAM(Pluggable Authentication Module)을 사용하여 유저의 비밀번호를 8자리 이상으로 규제하는 방법에 대해 알아보겠습니다.

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

```bash
$ sudo vi /etc/pam.d/common-password
# ...
password    requisite    pam_pwquality.so retry=3 minlen=8
```

`minlen=8` 옵션은 최소 비밀번호 길이를 8자로 설정하는 것입니다. 이를 추가합니다.

### 3️⃣ 설정 확인 및 테스트

`passwd` 명령어를 통해 사용자의 비밀번호를 변경할 시 제약 조건을 확인할 수 있습니다.

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
