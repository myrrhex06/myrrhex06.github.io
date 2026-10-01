---
title: "Rust - Rust 실행 환경 구성"
date: 2026-10-01 18:45:00 +0900
categories: [Language, Rust]
tags: [rust, window, vscode]
---

[rustup-init.exe](https://rust-lang.org/tools/install) 설치

> Visual Studio가 깔려있지 않다면, rustup-init을 실행했을 때 깔 수 있도록 프로세스를 제공해줌.
> {: .prompt-tip }

설치 후 아래 커맨드로 버전 확인

```bash
cargo --version
```

vscode extension 설치

- `rust-analyzer` : rust언어 분석기
- `CodeLLDB` : 디버깅 툴 연동
- `crates-io` : Rust 라이브러리 실시간 버전관리
- `Even Better TOML` : Rust 설정 파일 toml 색인화
- `Error Lens` : 컴파일 결과 실시간 반영

command

- `cargo new 프로젝트명` : 새로운 Rust 프로젝트 생성
- `cargo build` : 현재 Rust 프로젝트를 빌드
- `cargo run`: 현재 프로젝트 실행

## **Reference**

- [https://diy-multitab.tistory.com/105](https://diy-multitab.tistory.com/105)
- [https://wonlf.tistory.com/entry/Rust-2-Rust설치-프로젝트-생성과-기본-입출력-예제](https://wonlf.tistory.com/entry/Rust-2-Rust설치-프로젝트-생성과-기본-입출력-예제)