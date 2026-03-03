# Pixel Agents - Multi-Agent Edition 🎮

BSD Class 수강생용 커스텀 빌드입니다.

## ✨ 새로운 기능

- **멀티 에이전트 지원**: Claude, Gemini, GPT 세 가지 에이전트 타입 선택 가능
- **에이전트 타입 드롭다운**: `+ Agent` 버튼 클릭 시 선택 메뉴 표시
- **개선된 상태 감지**: Thinking 상태에서도 "Work" 표시

## 📦 설치 방법

### 방법 1: 자동 설치 (권장)

1. 압축 해제 후 `install.ps1` 실행:
```powershell
powershell -ExecutionPolicy Bypass -File install.ps1
```

2. VS Code / Antigravity 재시작

### 방법 2: 수동 설치

1. 기존 확장 폴더 백업:
```
%USERPROFILE%\.antigravity\extensions\pablodelucca.pixel-agents-1.0.0
```

2. 이 폴더의 내용물로 덮어쓰기

3. 터미널에서 빌드:
```bash
cd %USERPROFILE%\.antigravity\extensions\pablodelucca.pixel-agents-1.0.0
npm install
npm run build
```

4. VS Code / Antigravity 재시작

## 🎯 사용법

1. VS Code / Antigravity에서 Pixel Agents 패널 열기
2. `+ Agent` 버튼 클릭
3. 에이전트 타입 선택:
   - 🟠 **Claude Code** - Anthropic Claude (기본)
   - 🔵 **Gemini CLI** - Google Gemini
   - 🟢 **GPT / Codex** - OpenAI GPT

4. 선택한 에이전트가 터미널에서 실행됨

## ⚙️ 필수 CLI 설치

각 에이전트를 사용하려면 해당 CLI가 설치되어 있어야 합니다:

```bash
# Claude Code
npm install -g @anthropic-ai/claude-code

# Gemini CLI
npm install -g @google/gemini-cli

# OpenAI Codex (선택)
npm install -g @openai/codex
```

## 📁 폴더 구조

```
dist/
├── dist/          # 컴파일된 확장 코드
├── src/           # TypeScript 소스
│   └── agents/    # 에이전트 Provider 시스템
├── webview-ui-src/ # React 웹뷰 소스
└── package.json
```

## 🔧 개발/수정

소스 수정 후 빌드:
```bash
npm run build
```

## 📝 변경 이력

### v1.0.0-multiagent (2026-02-23)
- 멀티 에이전트 지원 추가
- Claude/Gemini/GPT Provider 시스템
- 에이전트 타입 선택 UI
- Thinking 상태 감지 개선

## 🆘 문제 해결

### "에이전트가 Rest 상태로만 표시됩니다"
- VS Code를 완전히 재시작해보세요
- 터미널에서 에이전트가 실제로 실행 중인지 확인

### "Gemini/GPT 에러가 발생합니다"
- 해당 CLI가 설치되어 있는지 확인: `gemini --version` 또는 `codex --version`

---

Made with 🦞 by Clawd for BSD Class
