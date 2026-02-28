# 🎨 Claude Code 팀 에이전트 색상 태그 활성화 완벽 가이드

> **색상 태그가 나오게 하는 3가지 핵심 설정**  
> BSD 바이브코딩 교육센터 - 실전 설정 가이드

---

## 📋 목차

1. [색상 태그란?](#색상-태그란)
2. [3가지 필수 설정](#3가지-필수-설정)
3. [1단계: settings.json 완벽 설정](#1단계-settingsjson-완벽-설정)
4. [2단계: tmux 256 컬러 모드 활성화](#2단계-tmux-256-컬러-모드-활성화)
5. [3단계: 터미널 확인](#3단계-터미널-확인)
6. [4단계: 완벽한 실행 순서](#4단계-완벽한-실행-순서)
7. [원클릭 자동 설정 스크립트](#원클릭-자동-설정-스크립트)
8. [문제 해결](#문제-해결)
9. [설정 체크리스트](#설정-체크리스트)

---

## 🎨 색상 태그란?

tmux에서 팀 에이전트를 실행하면 각 pane의 **오른쪽 상단**에 에이전트 이름과 색상이 표시됩니다:

```
┌─────────────────────────────────────┐
│  Backend Task Assignment            │
│                     @backend-dev  ← 파란색
├─────────────────────────────────────┤
│  QA Testing Task                    │
│                     @qa-engineer  ← 노란색
├─────────────────────────────────────┤
│  Frontend Development               │
│                     @frontend-dev ← 초록색
└─────────────────────────────────────┘
```

**장점:**

- ✅ 각 에이전트를 한눈에 구분
- ✅ 작업 진행 상황 모니터링 용이
- ✅ 전문적인 개발 환경 구축

---

## 🎯 3가지 필수 설정

색상 태그가 나오려면 **모든 조건**이 충족되어야 합니다:

1. ✅ `~/.claude/settings.json` - 환경 변수 설정
2. ✅ `~/.tmux.conf` - tmux 컬러 모드 활성화
3. ✅ 터미널 256 컬러 지원 확인

하나라도 빠지면 색상이 나오지 않습니다!

---

## 1단계: settings.json 완벽 설정

### 파일 위치

```bash
~/.claude/settings.json
```

### 필수 설정 내용

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
    "TERM": "xterm-256color",
    "COLORTERM": "truecolor"
  },
  "teammateMode": "tmux",
  "defaultModel": "claude-sonnet-4-5-20250929"
}
```

### 설정 확인 방법

```bash
# env 설정만 확인
jq .env ~/.claude/settings.json

# 전체 설정 확인
cat ~/.claude/settings.json | jq .

# JSON 유효성 검증
python3 -m json.tool ~/.claude/settings.json
```

**출력 예시 (정상):**

```json
{
  "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
  "TERM": "xterm-256color",
  "COLORTERM": "truecolor"
}
```

### 추가 권장 설정 (선택사항)

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
    "TERM": "xterm-256color",
    "COLORTERM": "truecolor",
    "FORCE_COLOR": "1"
  },
  "teammateMode": "tmux",
  "defaultModel": "claude-sonnet-4-5-20250929",
  "allowlist": {
    "bash": ["npm*", "pnpm*", "git*"],
    "write": ["src/**", "docs/**", "*.md", "*.json"]
  }
}
```

---

## 2단계: tmux 256 컬러 모드 활성화

### Option A: tmux 설정 파일 생성 (권장)

#### 1) `.tmux.conf` 파일 생성

```bash
nano ~/.tmux.conf
```

#### 2) 다음 내용 추가

```bash
# 256 컬러 지원 활성화
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:Tc"

# True Color 지원 (권장)
set -g default-terminal "tmux-256color"
set -as terminal-overrides ',xterm*:sitm=\E[3m'

# 마우스 지원 활성화 (편의 기능)
set -g mouse on

# 상태바 색상 설정 (선택사항)
set -g status-bg colour235
set -g status-fg colour136

# Pane 경계선 색상 (선택사항)
set -g pane-border-style fg=colour238
set -g pane-active-border-style fg=colour51
```

#### 3) 저장 후 설정 리로드

```bash
# tmux 설정 리로드
tmux source-file ~/.tmux.conf

# 또는 모든 tmux 세션 종료 후 재시작
tmux kill-server
```

### Option B: iTerm2 Control Mode (macOS - 가장 쉬움!)

```bash
# iTerm2에서 실행 (256 컬러 자동 지원)
tmux -CC

# 이 모드에서는 색상이 자동으로 표시됨!
```

**iTerm2 Control Mode 장점:**

- ✅ 자동 컬러 지원
- ✅ 네이티브 macOS 창 관리
- ✅ 별도 설정 불필요

---

## 3단계: 터미널 확인

### 현재 컬러 지원 확인

```bash
# 1. TERM 변수 확인
echo $TERM
# 출력: xterm-256color 또는 screen-256color 또는 tmux-256color

# 2. 256 컬러 지원 확인
tput colors
# 출력: 256

# 3. COLORTERM 확인
echo $COLORTERM
# 출력: truecolor 또는 24bit
```

### 실제 색상 테스트

```bash
# 256 컬러 팔레트 출력
for i in {0..255}; do
    printf "\x1b[38;5;${i}mcolor%-5i\x1b[0m" $i
    if ! (( ($i + 1 ) % 8 )); then
        echo
    fi
done
```

**정상 출력:** 다양한 색상의 숫자가 표시됨

### 색상이 안 나온다면

```bash
# 터미널 재시작 후 TERM 변수 설정
export TERM=xterm-256color

# 영구 설정 (~/.zshrc 또는 ~/.bashrc에 추가)
echo 'export TERM=xterm-256color' >> ~/.zshrc
source ~/.zshrc
```

---

## 4단계: 완벽한 실행 순서

### 순서대로 실행

```bash
# 1️⃣ 설정 파일 확인
cat ~/.claude/settings.json | jq .

# 2️⃣ tmux 컬러 모드로 시작
tmux -2  # 256 컬러 강제

# macOS + iTerm2라면
tmux -CC  # Control Mode (자동 컬러)

# 3️⃣ Claude Code 실행
claude

# 4️⃣ 팀 생성 (색상 태그 확인!)
# 프롬프트에 팀 생성 요청 입력
```

### 팀 생성 예시 프롬프트

```
Todo 앱을 위한 3명의 팀을 만들어줘:

리더는 Opus 4.5 (Delegate Mode)

팀원 3명 (Sonnet 4.5):
- @backend-dev (백엔드)
- @frontend-dev (프론트엔드)
- @qa-engineer (QA)
```

### 색상 태그 확인

각 pane의 **오른쪽 상단**에 다음이 표시되면 성공:

```
@backend-dev   ← 파란색
@frontend-dev  ← 초록색
@qa-engineer   ← 노란색
```

---

## 🚀 원클릭 자동 설정 스크립트

### 스크립트 파일 생성

```bash
nano ~/setup-claude-colors.sh
```

### 스크립트 내용

```bash
#!/bin/bash
# Claude Code 팀 에이전트 색상 설정 자동화 스크립트
# BSD 바이브코딩 교육센터

echo "🎨 Claude Code 팀 에이전트 색상 설정 시작..."
echo ""

# 1. .claude 디렉토리 생성
mkdir -p ~/.claude
echo "✅ ~/.claude 디렉토리 확인"

# 2. settings.json 생성/업데이트
cat > ~/.claude/settings.json << 'EOF'
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
    "TERM": "xterm-256color",
    "COLORTERM": "truecolor",
    "FORCE_COLOR": "1"
  },
  "teammateMode": "tmux",
  "defaultModel": "claude-sonnet-4-5-20250929",
  "allowlist": {
    "bash": [
      "npm*",
      "pnpm*",
      "git*"
    ],
    "write": [
      "src/**",
      "docs/**",
      "*.md",
      "*.json"
    ]
  }
}
EOF

echo "✅ settings.json 생성 완료"

# 3. .tmux.conf 생성/업데이트
cat > ~/.tmux.conf << 'EOF'
# 256 컬러 지원
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:Tc"

# True Color 지원
set -g default-terminal "tmux-256color"
set -as terminal-overrides ',xterm*:sitm=\E[3m'

# 마우스 지원
set -g mouse on

# 상태바 설정
set -g status-bg colour235
set -g status-fg colour136

# Pane 경계선 색상
set -g pane-border-style fg=colour238
set -g pane-active-border-style fg=colour51

# 히스토리 크기 증가
set -g history-limit 10000
EOF

echo "✅ .tmux.conf 생성 완료"

# 4. Shell 설정 (.zshrc 또는 .bashrc)
SHELL_RC=""
if [ -f ~/.zshrc ]; then
    SHELL_RC=~/.zshrc
elif [ -f ~/.bashrc ]; then
    SHELL_RC=~/.bashrc
fi

if [ -n "$SHELL_RC" ]; then
    if ! grep -q "export TERM=xterm-256color" "$SHELL_RC"; then
        echo "" >> "$SHELL_RC"
        echo "# Claude Code 256 컬러 지원" >> "$SHELL_RC"
        echo "export TERM=xterm-256color" >> "$SHELL_RC"
        echo "✅ $SHELL_RC에 TERM 변수 추가"
    else
        echo "✅ TERM 변수 이미 설정됨"
    fi
fi

# 5. 설정 확인
echo ""
echo "=== 설정 확인 ==="
echo "1. TERM: $(echo $TERM)"
echo "2. Colors: $(tput colors)"
echo "3. settings.json:"
cat ~/.claude/settings.json | python3 -m json.tool 2>/dev/null || cat ~/.claude/settings.json
echo ""

echo "🎉 설정 완료!"
echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "다음 명령어로 적용하세요:"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo ""
if [ -f ~/.zshrc ]; then
    echo "  source ~/.zshrc"
elif [ -f ~/.bashrc ]; then
    echo "  source ~/.bashrc"
fi
echo "  tmux kill-server"
echo "  tmux -CC  # (iTerm2)"
echo "  # 또는"
echo "  tmux -2   # (일반 터미널)"
echo "  claude"
echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "이제 팀 에이전트를 생성하면"
echo "색상 태그가 표시됩니다! 🎨"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
```

### 스크립트 실행

```bash
# 실행 권한 부여
chmod +x ~/setup-claude-colors.sh

# 실행!
~/setup-claude-colors.sh

# Shell 재로드
source ~/.zshrc  # 또는 source ~/.bashrc

# tmux 재시작
tmux kill-server

# 컬러 모드로 실행
tmux -CC  # iTerm2
# 또는
tmux -2   # 일반 터미널

# Claude Code 시작
claude
```

---

## 🔧 문제 해결

### 문제 1: 색상이 전혀 안 나옴

#### 증상

- 모든 텍스트가 흑백
- 에이전트 태그가 보이지 않음

#### 해결 방법

```bash
# 1. TERM 변수 확인
echo $TERM
# xterm-256color가 아니면:
export TERM=xterm-256color

# 2. tmux 완전 재시작
tmux kill-server
tmux -2

# 3. settings.json 재확인
jq .env ~/.claude/settings.json

# 4. 컬러 지원 확인
tput colors
# 256이 아니면 터미널 앱 설정 확인
```

---

### 문제 2: tmux는 컬러인데 Claude Code만 흑백

#### 증상

- tmux 상태바는 색상이 나옴
- Claude Code 출력만 흑백

#### 해결 방법

`~/.claude/settings.json` 수정:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
    "TERM": "xterm-256color",
    "COLORTERM": "truecolor",
    "FORCE_COLOR": "1"  ← 이것 추가!
  }
}
```

재시작:

```bash
tmux kill-server
tmux -2
claude
```

---

### 문제 3: 일부만 색상, 일부는 흑백

#### 증상

- 어떤 pane은 색상, 어떤 pane은 흑백
- 색상이 깜빡임

#### 해결 방법

`~/.tmux.conf` 확인:

```bash
cat ~/.tmux.conf
```

다음이 **모두** 있어야 함:

```bash
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:Tc"
```

없다면 추가 후:

```bash
tmux source-file ~/.tmux.conf
# 또는
tmux kill-server
```

---

### 문제 4: macOS에서 색상 안 나옴

#### 증상

- macOS iTerm2/Terminal.app
- 설정은 다 맞는데 색상 없음

#### 해결 방법

**iTerm2 Control Mode 사용 (최고 추천):**

```bash
tmux -CC
claude
```

**Terminal.app:**

```bash
# Preferences → Profiles → Advanced
# "Declare terminal as:" → xterm-256color로 변경

# 재시작
tmux -2
claude
```

---

### 문제 5: Linux에서 색상 안 나옴

#### 증상

- Ubuntu/Debian/CentOS
- 256 컬러 미지원

#### 해결 방법

```bash
# 1. 터미널 에뮬레이터 확인
echo $TERM

# 2. 256 컬러 지원 터미널 설치
sudo apt install gnome-terminal  # Ubuntu
# 또는
sudo apt install konsole  # KDE
# 또는
sudo apt install tilix  # 현대적인 터미널

# 3. 새 터미널에서 실행
tmux -2
claude
```

---

### 문제 6: Windows WSL2에서 색상 안 나옴

#### 증상

- WSL2 + Windows Terminal
- 색상 미지원

#### 해결 방법

**Windows Terminal 설정:**

1. Settings (Ctrl+,)
2. Profiles → Defaults
3. "Color scheme" → "One Half Dark" 선택
4. Advanced → Terminal type → "xterm-256color"

```bash
# WSL2에서
export TERM=xterm-256color
echo 'export TERM=xterm-256color' >> ~/.zshrc
source ~/.zshrc

tmux -2
claude
```

---

## ✅ 설정 체크리스트

### 자동 체크 스크립트

```bash
# 복사해서 터미널에 붙여넣기

echo "=== Claude Code 팀 에이전트 색상 설정 체크 ==="
echo ""

# 1. TERM 변수
echo "1️⃣ TERM 변수: $TERM"
if [[ $TERM == *"256color"* ]]; then
    echo "   ✅ 256 컬러 지원"
else
    echo "   ❌ TERM 변수를 xterm-256color로 설정 필요"
    echo "      → export TERM=xterm-256color"
fi
echo ""

# 2. tmux 컬러 지원
echo "2️⃣ 컬러 지원 수:"
COLORS=$(tput colors 2>/dev/null)
echo "   $COLORS colors"
if [ "$COLORS" -eq 256 ]; then
    echo "   ✅ 256 컬러 지원"
else
    echo "   ❌ 256 컬러 미지원"
    echo "      → tmux -2 로 실행하거나 .tmux.conf 설정 필요"
fi
echo ""

# 3. settings.json 존재
echo "3️⃣ settings.json 존재 여부:"
if [ -f ~/.claude/settings.json ]; then
    echo "   ✅ 존재함"
    echo ""
    echo "   env 설정:"
    if command -v jq &> /dev/null; then
        jq .env ~/.claude/settings.json 2>/dev/null

        # 필수 변수 확인
        HAS_TEAMS=$(jq -r '.env.CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS' ~/.claude/settings.json 2>/dev/null)
        HAS_TERM=$(jq -r '.env.TERM' ~/.claude/settings.json 2>/dev/null)

        if [ "$HAS_TEAMS" = "1" ]; then
            echo "   ✅ CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS 설정됨"
        else
            echo "   ❌ CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS 없음"
        fi

        if [[ "$HAS_TERM" == *"256color"* ]]; then
            echo "   ✅ TERM 256 컬러 설정됨"
        else
            echo "   ❌ TERM 256 컬러 설정 필요"
        fi
    else
        echo "   ⚠️ jq가 설치되지 않음. 수동 확인 필요:"
        echo "      cat ~/.claude/settings.json"
    fi
else
    echo "   ❌ ~/.claude/settings.json 파일 없음"
    echo "      → 자동 설정 스크립트 실행 필요"
fi
echo ""

# 4. tmux.conf 존재
echo "4️⃣ tmux.conf 존재 여부:"
if [ -f ~/.tmux.conf ]; then
    echo "   ✅ 존재함"
    if grep -q "256color" ~/.tmux.conf 2>/dev/null; then
        echo "   ✅ 256 컬러 설정 확인"
    else
        echo "   ⚠️ 256 컬러 설정 없음 (추가 권장)"
    fi
else
    echo "   ⚠️ ~/.tmux.conf 파일 없음"
    echo "      → 자동 설정 스크립트로 생성 권장"
fi
echo ""

# 5. tmux 실행 여부
echo "5️⃣ tmux 실행 상태:"
if [ -n "$TMUX" ]; then
    echo "   ✅ tmux 내부에서 실행 중"
else
    echo "   ⚠️ tmux 외부"
    echo "      → 색상을 보려면 tmux -2 또는 tmux -CC 실행 필요"
fi
echo ""

# 6. Shell 설정
echo "6️⃣ Shell 설정:"
if [ -f ~/.zshrc ]; then
    if grep -q "TERM=xterm-256color" ~/.zshrc; then
        echo "   ✅ .zshrc에 TERM 설정됨"
    else
        echo "   ⚠️ .zshrc에 TERM 설정 없음"
    fi
elif [ -f ~/.bashrc ]; then
    if grep -q "TERM=xterm-256color" ~/.bashrc; then
        echo "   ✅ .bashrc에 TERM 설정됨"
    else
        echo "   ⚠️ .bashrc에 TERM 설정 없음"
    fi
fi
echo ""

echo "=== 체크 완료 ==="
echo ""
echo "💡 문제가 있다면:"
echo "   1. 자동 설정 스크립트 실행"
echo "   2. Shell 재로드 (source ~/.zshrc)"
echo "   3. tmux 재시작 (tmux kill-server; tmux -2)"
echo "   4. Claude Code 재시작 (claude)"
```

---

## 📸 올바른 설정 확인 예시

### 1. settings.json 확인

```bash
jq .env ~/.claude/settings.json
```

**정상 출력:**

```json
{
  "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
  "TERM": "xterm-256color",
  "COLORTERM": "truecolor"
}
```

### 2. 컬러 지원 확인

```bash
tput colors
```

**정상 출력:**

```
256
```

### 3. tmux 설정 확인

```bash
grep "256color" ~/.tmux.conf
```

**정상 출력:**

```bash
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:Tc"
```

### 4. 실제 색상 테스트

```bash
printf "\x1b[38;5;196mRed\x1b[0m "
printf "\x1b[38;5;46mGreen\x1b[0m "
printf "\x1b[38;5;21mBlue\x1b[0m\n"
```

**정상 출력:** Red, Green, Blue가 각각 빨강, 초록, 파랑 색으로 표시

---

## 🎓 수강생을 위한 Quick Start

### 가장 빠른 설정 방법 (3분)

```bash
# 1. 자동 설정 스크립트 다운로드 및 실행
curl -o ~/setup-claude-colors.sh https://raw.githubusercontent.com/.../setup-claude-colors.sh
chmod +x ~/setup-claude-colors.sh
~/setup-claude-colors.sh

# 2. Shell 재로드
source ~/.zshrc  # 또는 source ~/.bashrc

# 3. tmux 시작
tmux -CC  # macOS iTerm2
# 또는
tmux -2   # 일반 터미널

# 4. Claude Code 시작
claude

# 5. 팀 생성 테스트
# → 색상 태그 확인!
```

---

## 💡 유용한 팁

### Tip 1: 색상 테마 커스터마이징

`~/.tmux.conf`에서 색상 변경:

```bash
# 파란색 테마
set -g status-bg colour17
set -g pane-active-border-style fg=colour39

# 초록색 테마
set -g status-bg colour22
set -g pane-active-border-style fg=colour46

# 보라색 테마
set -g status-bg colour53
set -g pane-active-border-style fg=colour141
```

### Tip 2: 색상 팔레트 확인

```bash
# 모든 256 색상 보기
for i in {0..255}; do
    printf "\x1b[48;5;${i}m%3d\x1b[0m " $i
    [ $(( ($i + 1) % 16 )) -eq 0 ] && echo
done
```

### Tip 3: 에이전트별 색상 규칙

Claude Code는 자동으로 색상 할당:

- **Backend/Server**: 파란색 계열
- **Frontend/UI**: 초록색 계열
- **Test/QA**: 노란색 계열
- **Data/DB**: 보라색 계열

---

## 🔗 참고 자료

### 공식 문서

- [tmux 매뉴얼](https://man.openbsd.org/tmux.1)
- [iTerm2 Control Mode](https://iterm2.com/documentation-tmux-integration.html)

### 추가 학습

- Claude Code CLI 팀 에이전트 완벽 가이드
- tmux 단축키 치트시트
- 256 컬러 터미널 설정 가이드

---

## 📞 문제 해결 지원

### 여전히 색상이 안 나온다면

1. **체크리스트 스크립트** 실행
2. **자동 설정 스크립트** 다시 실행
3. **모든 터미널 재시작**
4. **바이브코딩 커뮤니티에 문의**

---

## 📝 버전 정보

- **가이드 버전**: 1.0
- **최종 업데이트**: 2025-02-24
- **작성**: BSD 바이브코딩 교육센터
- **대상**: 바이브코딩 수강생

---

**🎨 색상 설정 완료 후 개발이 더 즐거워집니다!**

**Happy Coding! 💪✨**
