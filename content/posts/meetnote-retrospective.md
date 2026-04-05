---
title: "Meetnote 개발 회고 — 로컬 회의록 자동화 Obsidian 플러그인"
slug: "meetnote-retrospective"
date: 2026-04-05
draft: false
categories:
  - dev
tags:
  - obsidian
  - whisper
  - pyannote
  - speaker-diarization
  - python
  - typescript
---

## 왜 만들었나

회의가 끝나고 나면 항상 같은 고민이 반복됐다. "누가 뭐라고 했더라?" 회의록을 작성하자니 회의에 집중이 안 되고, 녹음만 해두면 다시 듣기가 귀찮다. 기존 SaaS 서비스들은 비용도 문제지만, 회의 내용이 외부 서버로 나가는 것 자체가 부담스러웠다.

그리고 결정적인 이유가 하나 더 있었다. 지금 모든 업무 문서를 Obsidian에서 마크다운으로 관리하고 있는데, 이건 AX(AI Transformation) 관점에서 매우 중요한 선택이었다. **마크다운은 AI 에이전트에게 전달하기 가장 좋은 포맷이다.** 회의록도 마크다운으로 생성되면 별도 서비스에서 옮기는 번거로움 없이, 다른 업무 문서들과 함께 한 곳에서 관리할 수 있다. 에이전트가 과거 회의 맥락을 참조하고, 액션 아이템을 추적하고, 관련 문서를 연결하는 것까지 자연스럽게 이어진다.

그래서 만들었다. **완전히 로컬에서 동작하는 회의록 자동화 도구.** 비용 0원, 오프라인 가능, GPU 가속. 그리고 결과물은 Obsidian Vault 안의 마크다운 파일.

## 무엇을 만들었나

[Meetnote](https://github.com/changsu-jin/meetnote)는 Obsidian 플러그인 + Python 백엔드로 구성된 로컬 회의록 자동화 도구다.

핵심 기능:
- **실시간 음성 인식** — Whisper large-v3-turbo, 5초 단위 스트리밍
- **화자 분리** — pyannote 3.1, GPU 자동 감지
- **화자 식별** — 임베딩 DB 기반, 회의할수록 정확도 향상
- **AI 요약** — Claude CLI / Ollama, 액션 아이템 자동 생성
- **이메일 발송** — 참석자별 회의록 HTML 메일
- **암호화** — AES 암호화, 자동 삭제, 감사 로그

Apple Silicon 기준 60분 회의를 약 5분 만에 처리한다.

![Meetnote 실제 사용 화면](/img/meetnote-screenshot.png)

## 기술 스택

| 영역 | 기술 |
|------|------|
| Plugin | TypeScript, Obsidian API, Web Audio API |
| Backend | Python, FastAPI, WebSocket |
| STT | mlx-whisper (Apple Silicon), faster-whisper (CPU) |
| 화자 분리 | pyannote.audio 3.1 |
| 화자 임베딩 | wespeaker-voxceleb-resnet34-LM |
| AI 요약 | Claude CLI, Ollama |
| 배포 | Docker, GitHub Actions, GHCR |

## 아키텍처

```
┌─────────────┐     WebSocket (PCM 16kHz)     ┌──────────────┐
│  Obsidian   │ ──────────────────────────────▶│   FastAPI     │
│  Plugin     │◀────────────── JSON ──────────│   Backend     │
│  (TS)       │                                │   (Python)    │
└─────────────┘                                └──────┬───────┘
                                                      │
                                    ┌─────────────────┼─────────────────┐
                                    │                 │                 │
                              ┌─────▼─────┐   ┌──────▼──────┐  ┌──────▼──────┐
                              │ Whisper   │   │  pyannote   │  │  Speaker    │
                              │ STT       │   │  Diarizer   │  │  Embedding  │
                              └───────────┘   └─────────────┘  └─────────────┘
```

Plugin이 Web Audio API로 마이크 음성을 캡처해서 5초 단위로 WebSocket을 통해 바이너리 전송한다. 백엔드는 Whisper로 실시간 전사하고, 녹음 종료 후 pyannote로 화자 분리를 수행한다. 전사 결과와 화자 분리 결과를 시간 기반으로 병합해서 "누가 무슨 말을 했는지" 최종 결과를 만든다.

## 개발 과정에서 마주친 문제들

### 1. MLX vs faster-whisper — 플랫폼별 분기

Apple Silicon에서는 MLX가 압도적으로 빠르지만, Linux/CUDA 환경에서는 faster-whisper가 낫다. 처음에는 하나로 통일하려 했지만, 성능 차이가 너무 커서 런타임에 플랫폼을 감지하고 분기하는 방식을 택했다.

### 2. 화자 분리와 전사 결과 병합

Whisper와 pyannote는 독립적으로 동작한다. 둘의 타임스탬프가 정확히 일치하지 않기 때문에, 시간 구간의 최대 겹침(temporal overlap)을 기준으로 화자를 배정했다. 같은 화자의 연속 발언은 5초 이내 간격이면 하나로 합친다.

### 3. 환각(Hallucination) 필터링

Whisper는 가끔 무음 구간에서 환각을 일으킨다. `no_speech_prob`과 `compression_ratio`를 기준으로 품질 필터를 넣어 해결했다. LLM 기반 전사 보정도 추가했는데, 고유명사나 전문 용어의 정확도가 체감될 정도로 올라갔다.

### 4. 동시 녹음 세션 처리

Transcriber와 Diarizer는 GPU 메모리를 많이 쓰기 때문에 싱글톤으로 관리한다. 하지만 여러 사용자가 동시에 녹음할 수 있어야 하므로, WebSocket 세션별로 격리하되 공유 리소스는 Lock으로 보호하는 구조로 만들었다.

### 5. Docker 빌드 최적화

처음 Docker 빌드는 46분이 걸렸다. PyTorch + pyannote 의존성이 무거웠기 때문이다. base 이미지와 app 이미지를 분리하고, GitHub Actions에서 base 이미지를 캐싱하는 방식으로 빌드 시간을 3분까지 줄였다. 이후 추가 최적화로 1분대까지 단축했다.

## 누적 학습 — 화자 임베딩 DB

가장 만족스러운 기능이다. 회의에서 새로운 화자가 감지되면 임베딩 벡터를 자동 저장한다. 다음 회의에서 코사인 유사도(임계값 0.70)로 기존 화자와 매칭한다. 회의를 할수록 화자 인식 정확도가 올라가는 구조다.

## 타임라인

22일간 115개 커밋. 혼자서 기획부터 배포까지.

- **Phase 0** — MVP: 실시간 전사, 화자 분리, 요약, 암호화, RAG 검색
- **Phase 1** — 안정화: 버그 수정, 환각 필터, 품질 최적화
- **Phase 2** — UX: 사이드 패널, 화자 관리 UI, 이메일, 원클릭 설치
- **Phase 3** — 배포: Docker, GHA 캐싱, 온보딩 모달, 보안 강화

## 돌아보며

22일이라는 짧은 기간이었지만, "내가 직접 쓸 도구"를 만드는 과정이라 몰입도가 높았다. 특히 화자 임베딩 DB의 누적 학습은 쓸수록 똑똑해지는 느낌이 있어서 뿌듯하다.

아쉬운 점은 Windows 네이티브 지원이 아직 Docker(WSL2) 의존이라는 것. 그리고 CPU 환경에서는 60분 회의 처리에 40분이 걸려서 실용성이 떨어진다.

다음 목표는 웹 기반 대시보드와 팀 단위 화자 DB 공유 기능이다.

---

[GitHub Repository](https://github.com/changsu-jin/meetnote)
