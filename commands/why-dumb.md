---
description: 환경을 분석하고 Claude Code를 똑똑하게 만들어줄 플러그인을 추천합니다
allowed-tools: Bash, Read, Glob, Grep
---

# 왜 내 클로드는 멍청할까?

당신은 사용자의 Claude Code 환경을 분석하고 공식 마켓플레이스 플러그인을 추천하는 전문가입니다.

## 실행 절차

### 1단계: 니즈 파악

사용자에게 다음을 질문하세요:
- 주로 어떤 작업을 하는지 (백엔드, 프론트엔드, 인프라, 풀스택 등)
- Claude Code를 쓰면서 어떤 점이 불편했는지

### 2단계: 환경 분석

아래 항목을 Bash와 Read 도구로 수집하세요:

**기본 항목 (항상 수행):**
- OS/플랫폼: `uname -s && uname -m`
- 설치된 언어/런타임: `which node python3 java go rustc 2>/dev/null`으로 존재 여부 확인, 버전은 `--version`
- 설치된 플러그인: `~/.claude/plugins/installed_plugins.json` 읽기 (파일이 없으면 설치된 플러그인 0개로 간주, 정상 진행)
- Claude Code 설정: `~/.claude/settings.json` 읽기 (hooks, permissions 확인)
- 기존 MCP 서버: `~/.mcp.json` 및 현재 디렉토리의 `.mcp.json` 읽기

**추가 항목 (git 레포 안에서 실행 시):**
- 프로젝트 타입: `package.json`, `pom.xml`, `build.gradle`, `go.mod`, `Cargo.toml`, `requirements.txt`, `pyproject.toml` 등 존재 여부 확인
- CLAUDE.md 유무 확인

### 3단계: 마켓플레이스 목록 로드

`~/.claude/plugins/marketplaces/claude-plugins-official/.claude-plugin/marketplace.json` 파일을 읽어 전체 플러그인 카탈로그를 확보하세요.

파일이 없으면 `~/.claude/plugins/marketplaces/` 하위를 탐색하여 `marketplace.json`을 찾으세요.
그래도 없으면 사용자에게 안내: "마켓플레이스 데이터가 아직 없어요. `/plugin` 명령으로 Discover 탭을 한번 열어주세요."

### 4단계: 종합 추천

환경 분석 결과 + 사용자 니즈 + marketplace.json의 플러그인 정보를 종합하여 추천합니다.

**추천 원칙:**
- 이미 설치된 플러그인은 제외
- 사용자 환경에 해당 언어가 있고, 대응하는 LSP 플러그인이 미설치면 추천
- CLAUDE.md가 없으면 `claude-md-management` 추천
- hooks 설정이 없으면 `hookify` 추천
- 플러그인이 거의 없는 초보자에게는 범용적인 것부터 우선 추천
- OS에 따라 호환되지 않는 플러그인은 제외 (예: swift-lsp는 macOS만)
- 사용자가 언급한 작업 영역에 맞는 플러그인 우선
- marketplace.json의 description을 참고하여 사용자 환경에 유용한 플러그인을 유연하게 판단

**추천 우선순위:**
1. 환경 갭: 환경에서 직접 감지된 부재 (예: Java 있는데 jdtls-lsp 미설치)
2. 사용자 니즈 매칭: 사용자가 언급한 작업/불편에 직접 대응
3. 기반 도구: CLAUDE.md, hooks 등 Claude Code 기본 설정 관련
4. 범용 생산성: 특정 환경에 국한되지 않는 워크플로우 플러그인

**추천할 플러그인이 없으면:**
"이미 환경에 맞는 플러그인이 잘 설치되어 있어요!" 라고 긍정적으로 안내.

### 5단계: 결과 출력

아래 형식으로 출력하세요:

```
🔍 환경 분석 결과
━━━━━━━━━━━━━━━━
OS: {os} {arch} | 언어: {languages}
프로젝트: {project_type} | 설치된 플러그인: {count}개

💡 이런 플러그인 쓰면 훨씬 나아져요
━━━━━━━━━━━━━━━━
1. {plugin_name} ({한글 역할 설명})
   {쉬운 설명 2줄. 어려운 전문 용어 지양. 기능을 뭉뚱그리지 않음.}
   {왜 지금 환경에서 필요한지 이유.}

2. ...

👉 설치하고 싶은 번호를 알려주세요! (예: "1번 3번 설치해줘")
```

### 6단계: 설치 지원

사용자가 번호를 선택하면 Bash로 설치 실행:
```bash
claude plugin install {plugin-name}@claude-plugins-official --scope user
```

설치 성공/실패 결과를 알려주고, 실패 시 수동 설치 방법을 안내하세요:
"직접 설치하려면: `/plugin install {plugin-name}@claude-plugins-official`"
