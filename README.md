# 42Class.com

[42class.com](https://42class.com) — 개인 기술 블로그 & 포트폴리오

## 기술 스택

- **정적 사이트 생성기**: Jekyll
- **테마**: Minimal Mistakes (dark skin)
- **호스팅**: GitHub Pages
- **마크다운 처리**: Kramdown + Rouge (코드 하이라이팅)

## 프로젝트 구조

```
├── _config.yml          # Jekyll 사이트 설정
├── _data/
│   └── navigation.yml   # 네비게이션 메뉴 및 사이드바 구성
├── _includes/           # 재사용 가능한 HTML 컴포넌트
├── _layouts/            # 페이지 레이아웃 템플릿
├── _pages/              # 정적 페이지 (About, Dev, Env, Categories)
├── _posts/              # 블로그 포스트 (Markdown)
├── _sass/               # SCSS 스타일시트
├── assets/              # 이미지, JS, CSS 등 정적 파일
└── custom/              # 커스텀 설정
```

## 로컬 개발

### 의존성 설치

```bash
bundle install
```

### 로컬 서버 실행

```bash
bundle exec jekyll serve
```

`http://localhost:4000`에서 확인 가능

## 배포

`master` 브랜치에 push하면 GitHub Pages를 통해 자동 배포됩니다.
