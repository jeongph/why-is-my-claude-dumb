# why-is-my-claude-dumb

> 왜 내 클로드는 멍청할까? 환경을 분석하고 플러그인을 추천해드립니다.

Claude Code 사용자의 로컬 환경과 코드베이스를 분석하여,
설치하면 도움이 될 공식 마켓플레이스 플러그인을 추천하는 플러그인입니다.

## 설치

### 마켓플레이스로 설치

```bash
# 마켓플레이스 추가
/plugin marketplace add jeongph/why-is-my-claude-dumb

# 플러그인 설치
/plugin install why-is-my-claude-dumb@jeongph-why-is-my-claude-dumb
```

### 로컬 설치 (개발/테스트)

```bash
git clone https://github.com/jeongph/why-is-my-claude-dumb.git
claude --plugin-dir ./why-is-my-claude-dumb
```

## 사용법

### 커맨드로 실행 (풀 분석)

```
/why-is-my-claude-dumb:why-dumb
```

대화형으로 니즈를 파악하고, 환경을 분석한 뒤, 맞춤 플러그인을 추천합니다.

### 자동 트리거 (가벼운 추천)

Claude에게 "멍청하다", "왜 이렇게 못해" 같은 타박을 하면,
dumb-doctor가 자동으로 환경 점검을 제안합니다.

## 분석 항목

- OS/플랫폼
- 설치된 언어/런타임
- 설치된 플러그인 목록
- Claude Code 설정 (hooks, permissions)
- MCP 서버 연결 상태
- 프로젝트 타입 (레포 안에서 실행 시)
- CLAUDE.md 유무

## License

MIT
