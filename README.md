# 42Class.com

[42class.com](https://42class.com) — 개인 기술 블로그 & 포트폴리오

## 기술 스택

- **정적 사이트 생성기**: Hugo
- **테마**: Stack (CaiJimmy/hugo-theme-stack)
- **호스팅**: GitHub Pages (GitHub Actions)

## 프로젝트 구조

```
├── hugo.yaml                # Hugo 사이트 설정
├── content/
│   ├── posts/               # 블로그 포스트 (Markdown)
│   └── page/
│       ├── about/           # About 페이지
│       ├── dev/             # Development 페이지
│       ├── env/             # Environment 페이지
│       ├── archives/        # Archives 페이지
│       └── search/          # 검색 페이지
├── static/
│   └── img/                 # 이미지 파일
├── assets/
│   └── img/                 # Hugo에서 처리하는 이미지 (아바타 등)
├── layouts/
│   └── _markup/             # 테마 오버라이드 (이미지 렌더링)
├── themes/
│   └── stack/               # Stack 테마 (git submodule)
└── .github/
    └── workflows/
        └── hugo.yml         # GitHub Actions 배포 워크플로우
```

## 로컬 개발

### Hugo 설치

```bash
brew install hugo
```

### 로컬 서버 실행

```bash
hugo server --buildDrafts
```

`http://localhost:1313`에서 확인 가능

### 새 포스트 작성

```bash
hugo new content posts/my-new-post.md
```

`draft: true`로 생성됨 — 배포 시 자동 제외, 로컬에서 `--buildDrafts`로 미리보기

## 배포

`master` 브랜치에 push하면 GitHub Actions를 통해 자동 빌드/배포됩니다.

GitHub 레포 Settings > Pages > Source를 **GitHub Actions**로 설정 필요.
