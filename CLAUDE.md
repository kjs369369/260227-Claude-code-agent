# GenSpark AI Developer 워크숍 프로젝트

## 프로젝트 개요
- AI 강사 대상 GenSpark AI Developer 2시간 실습 강의자료
- Marp 마크다운 슬라이드 기반

## 주요 파일
- `GenSpark-AI-Developer-Workshop.md` - Marp 슬라이드 원본
- `GenSpark-AI-Developer-Workshop.html` - 브라우저용 프레젠테이션
- `docs/plans/` - 강의 설계서
- `Claude Code agent .md` - 팀 에이전트 색상 태그 가이드

## 빌드 명령어
```bash
npx @marp-team/marp-cli@latest GenSpark-AI-Developer-Workshop.md --html --output GenSpark-AI-Developer-Workshop.html
```

## 규칙
- 슬라이드 수정 후 반드시 HTML 재빌드
- 버전 태그 형식: v1.0, v1.1, v2.0
- 외부 링크는 워크숍 전날 반드시 검증
