# Singleton

## 구현 목적

프로젝트 전체에서 하나만 존재해야 하는 객체를 관리합니다.

대표적인 예

- GameManager
- UIManager
- SoundManager

---

## 장점

- 접근이 쉽다.
- 구현이 단순하다.

---

## 단점

- 결합도가 높다.
- 테스트가 어렵다.
- 규모가 커질수록 관리가 어려워진다.

---

## 대체 구조

- [Dependency Injection](DI.md)
- [Service Locator]
- [Event]
