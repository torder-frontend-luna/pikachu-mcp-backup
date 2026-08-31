# MCP 서버 설정 관리 및 복구

## 목적
MCP (Model Context Protocol) 서버 설정을 관리하고, 메인터넌스/복구 시 필요한 정보를 제공합니다.

## 현재 연결된 MCP 서버

| 서버 | URL | 인증 | 상태 |
|------|-----|------|------|
| **Datadog** | `https://mcp.datadoghq.com/api/unstable/mcp-server/mcp` | `DD-API-KEY`, `DD-APPLICATION-KEY` | ✅ 활성화 |
| **GitHub** | `https://api.githubcopilot.com/mcp/` | Bearer Token (GitHub Copilot) | ✅ 활성화 |
| **Confluence** | *설정 없음* | *필요* | ❌ 미연결 |

## 설정 파일 위치
- `/home/startup067/.hermes/config.yaml`

## Confluence 정보 (복구 필요)
- **공간**: `T1PM` (AI 플랫폼실)
- **회고 폴더 ID**: `555188231`
- **회고 템플릿 ID**: `600703136`
- **배포계획서 템플릿 ID**: `2019369252`

## 복구 절차 (메인터넌스 후)

1. **현재 상태 확인**
   ```bash
   cat ~/.hermes/config.yaml | grep -A 5 "mcp_servers"
   ```

2. **누락된 MCP 서버 식별**
   - 과거 대화 (`session_search`) 에서 설정 정보 검색
   - 키워드: `MCP 설정`, `노동츄 컨테이너`, `메인터넌스`

3. **설정 재등록**
   - Confluence MCP 필요 시: Atlassian API 토큰 또는 사내 Confluence URL 필요
   - `config.yaml` 에 `mcp_servers` 섹션 추가

## 자동화 작업 상태

| 작업 이름 | 목적 | 스케줄 | 상태 |
|-----------|------|--------|------|
| `monthly-deployment-planner` | 월간 배포계획서 생성 | `0 0 1 * *` | ✅ 활성화 |
| `sprint-retrospective` | 격주 회고 문서 생성 | `0 9 * * 2` | ✅ 활성화 |
| `sentry-auto-analysis` | Sentry 이슈 진단 | `0 */6 * * *` | ✅ 활성화 |
| `pr-auto-review` | PR 자동 리뷰 | `*/30 * * * *` | ✅ 활성화 |
| `로컬 코드 최신화` | 7 개 레포 git pull | `0 0 * * *` | ❌ **삭제됨** |

## 주의 사항

- **중복 생성 방지**: Confluence 문서 생성 전 반드시 검색하여 중복 확인
- **권한 제한**: Datadog MCP 는 `monitors_read`, `logs_read_data` 만 사용 가능
- **자동화 작업 누락**: `로컬 코드 최신화` 작업은 삭제됨, 복구 필요 시 재설정

## 관련 도구
- `cronjob` — 자동화 작업 관리
- `session_search` — 과거 설정 정보 검색
- `read_file` — `config.yaml` 확인

---
*최종 업데이트: 2026-08-31 (NHN 메인터넌스 복구 중)*
