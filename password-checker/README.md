# password_checker

This simple password checker is implemented in Rust. The program is implemented in two parts: a policy checker (that runs in the zkVM) and a host driver (an ordinary command-line program that uses the zkVM to run the policy checker).

The policy checker accepts a password string and a salt from the host driver and checks the validity of the password. A password validity-checking function then examines the password and panics if criteria are not met. If the password meets validity criteria, execution proceeds and the zkVM appends a hash of the salted password to the journal. The journal is a readable record of all values committed by code in the zkVM; it is attached to the receipt (a record of correct execution).

이 간단한 비밀번호 확인기는 Rust로 구현되어 있습니다. 프로그램은 두 부분으로 구성됩니다. 하나는 zkVM에서 실행되는 정책 확인기이고, 다른 하나는 zkVM을 사용하여 정책 확인기를 실행하는 일반적인 명령 줄 프로그램인 호스트 드라이버입니다.

정책 확인기는 호스트 드라이버에서 비밀번호 문자열과 솔트를 받아들이고 비밀번호의 유효성을 확인합니다. 비밀번호 유효성 검사 함수는 비밀번호를 검사하고 기준을 충족하지 않으면 패닉합니다. 비밀번호가 유효 기준을 충족하면 실행이 진행되고, zkVM은 솔트된 비밀번호의 해시를 저널에 추가합니다. 저널은 zkVM의 코드가 커밋한 모든 값들의 기록 가능한 레코드입니다. 이것은 영수증 (올바른 실행 기록)에 첨부됩니다.

# Why use zkVM to run this?

Our goal is to run our own password check locally without having to share our password directly with a recipient, preferring instead to share only a SHA-256 password hash. Because the validity-checking and hashing functionality runs on the zkVM, it generates a receipt that identifies which binary was executed (via the method ID), associates shared results with this particular execution (via the journal), and confirms its own integrity (via the cryptographic seal).

저희의 목표는 SHA-256 비밀번호 해시만 공유하고 실제 비밀번호를 수신자와 직접 공유하지 않고 로컬에서 자체 비밀번호 확인을 실행하는 것입니다. 유효성 검사 및 해싱 기능은 zkVM에서 실행되므로, 메서드 ID를 통해 실행된 이진 파일을 식별하고 (저널을 통해) 해당 특정 실행과 공유된 결과를 연결하며, 암호화된 인증서를 통해 자신의 무결성을 확인하는 영수증을 생성합니다.

# Project organization

The main program that calls a method in the guest ZKVM is in [cli/src/main.rs](cli/src/main.rs). The code that runs inside the ZKVM is in [methods/guest/src/bin/pw_checker.rs](methods/guest/src/bin/pw_checker.rs). The rest of the project is build support.

For the main RISC Zero project, see [here](https://github.com/risc0/risc0)

# Run this example

To build and run this example, use:

```
cargo run --release
```

# And now, some fine print

This repository contains example code meant to illustrate the fundamentals of programming with the zkVM. The password policy (and broader protocol) implemented here is intended for educational purposes only.


## Video Tutorial

For a walk-through of the fundamentals of this example, check out this [excerpt from our workshop at ZK HACK III](https://www.youtube.com/watch?v=vxqxRiTXGBI&list=PLcPzhUaCxlCgig7ofeARMPwQ8vbuD6hC5&index=5).
