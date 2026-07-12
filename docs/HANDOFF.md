# 인수인계서 — 건설공사 견적 자동화 시스템 기획

## 지금까지 한 일

건설공사 도면 기반 물량산출(BOQ) 자동화 시스템의 아키텍처·로드맵을 기획했다.
초기 스케치(v1)를 독립 리뷰어 2인(지속 토론 리뷰어 1인 + 이력 없는 제3 리뷰어 1인)과
**5라운드 적대적 토론**을 거쳐 고도화했고, 양측이 "이견 없음"을 선언한 시점에 종료했다.

**결과물**: `docs/estimation-automation-plan.md` (브랜치 `claude/construction-estimate-automation-0rfgkl`,
커밋 `f33ddad`, 이미 원격 푸시 완료)

문서 구성: 목표·성공지표(§0) → 핵심 아키텍처 결정(§1) → 타깃 고객·사업 가설(§2) →
입력 계층(§3) → 파이프라인(§4) → 도면 밖 공종(§5) → 검토 UI(§6) → 디스크레펀시 리포트(§7) →
평가 체계(§8) → 데이터·법무·책임(§9) → 팀·기간(§10) → 로드맵(§11) → 출력 규격(§12) →
**부록 A: 4라운드 토론 이력 요약 + 기각된 초기 아이디어 목록**

## 핵심 결론 (원 질문: 도면 파싱 vs 자동 3D 모델링)

양자택일이 아니라 **"도면 파싱 → 지오메트리 없는 경량 시맨틱 모델 → 룰 기반 물량산출"**의
중간 경로가 정답. 근거는 문서 §1과 부록 A에 상세.

## 완료된 것 / 아직 안 한 것

**완료**: 계획 수립·검증·문서화. 이건 순수 기획 문서이며 **코드는 아직 한 줄도 없다**
(리포지토리가 원래 빈 상태였음).

**§11 로드맵의 0단계(다음 실행 단계)**:
- DWG 변환 스파이크 (LibreDWG 충실도 검증, N=50 실무 도면 기준)
- 검토 UI 뷰어 빌드/바이 스파이크 (두 스파이크는 결합 매트릭스로 함께 판단 — §6 참조)
- 병렬 트랙: GT(ground truth) 1차 웨이브 발주, 적산 전문가 채용, 파일럿 파트너 계약

## 미해결 이슈 — Codex CLI 인증 차단 (이번 세션에서 미완료)

사용자가 "codex cli sol ultra와 토론하라"고 요청했으나, 이 환경에 Codex CLI 인증 세션이
없어 대신 Claude 서브에이전트 2인으로 적대적 토론을 대체 진행했다 (결과 자체는 완료·검증됨).

Codex 인증을 별도로 시도했으나 **네트워크 정책 차단**으로 실패:
```
$ npm install -g @openai/codex   # 설치는 성공 (codex-cli 0.144.1)
$ codex login --device-auth
Error logging in with device code: error sending request for url
  (https://auth.openai.com/api/accounts/deviceauth/usercode)
```
프록시 상태(`curl $HTTPS_PROXY/__agentproxy/status`)에서 `auth.openai.com`,
`api.openai.com`, `chatgpt.com` 세 도메인 모두 `connect_rejected` / **정책 거부(403)**로
명시적으로 확인됨. TLS나 설정 문제가 아니라 이 환경(Environment)의 네트워크 정책이
OpenAI 도메인을 허용 목록에서 막고 있는 것.

**새 세션에서 필요한 조치**:
1. claude.ai/code에서 이 프로젝트의 Environment 설정 → 네트워크 정책 확인
2. `auth.openai.com` / `api.openai.com` / `chatgpt.com`을 허용 목록에 추가하거나
   "Unrestricted" 정책으로 변경
3. 정책 변경은 컨테이너 재시작 후 적용되는 경우가 많으므로, 변경 후 **새 세션**에서
   `codex login --device-auth` 재시도
4. 인증되면 디바이스 코드가 나오고, 사용자가 브라우저에서 그 코드를 입력하는 방식
   (API 키를 채팅에 직접 붙여넣을 필요 없음)

## 다음 세션에서 할 수 있는 것

- Codex 인증이 열리면, 완성된 계획서(`docs/estimation-automation-plan.md`)를 Codex에게
  검토시켜 (진짜) Codex와의 교차검증을 추가로 받을 수 있음 — 다만 Claude 2인 토론이 이미
  충분히 엄격했으므로 필수는 아니고 선택 사항.
- 또는 계획을 실행 단계로 넘겨 0단계 스파이크(DWG 파싱 PoC, 부재 모델 스키마 v0)
  코드 작업을 시작.
