# Texture Compression

## 정의

Texture Compression은 **Texture를 압축하여 VRAM 사용량을 줄이는 기법**이다.

---

## 왜 사용하는가?

압축 전

```text
100MB
```

↓

압축

↓

```text
25MB
```

↓

VRAM 감소

↓

GPU 메모리 절약

↓

Rendering 성능 향상

---

## 장점

- VRAM 절약
- 메모리 절약
- GPU 부하 감소
- Texture 로딩 속도 향상

---

## Unity에서

```text
Texture

↓

Inspector

↓

Texture Import Settings

↓

Compression
```

또는

```text
Format

↓

ASTC

ETC2

DXT
```

설정 가능. 플랫폼에 따라 자동 변경 가능

---

## 플랫폼별

Android

↓

ETC2 / ASTC

---

PC

↓

DXT / BC

---

iOS

↓

ASTC

---

## 면접 포인트

압축 포맷 이름보다

```text
Compression

↓

VRAM 절약

↓

성능 향상
```

이 더 중요하다.
