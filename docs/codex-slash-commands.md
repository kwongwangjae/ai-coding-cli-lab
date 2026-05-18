# Codex CLI Slash Commands

Codex CLI의 interactive session에서 `/`를 입력하면 slash command popup을 열 수 있습니다.
명령어를 선택하거나 이름을 계속 입력해 필터링하고, 작업이 실행 중일 때는 `Tab`으로 다음 turn에 실행할 명령을 queue할 수 있습니다.

## 기준

- 로컬 확인 버전: `codex-cli 0.130.0`
- 확인일: 2026-05-18
- 주요 출처:
  - [OpenAI Developers - Slash commands in Codex CLI](https://developers.openai.com/codex/cli/slash-commands)
  - [OpenAI Developers - Codex CLI](https://developers.openai.com/codex/cli)
  - [OpenAI Developers - Command line options](https://developers.openai.com/codex/cli/reference)

## 자주 쓰는 명령어

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/status` | 현재 session 설정, token 사용량, context 상태를 확인합니다. | 모델, approval policy, writable root가 헷갈릴 때 먼저 실행합니다. |
| `/model` | active model과 reasoning effort를 바꿉니다. | 빠른 확인은 가벼운 모델, 복잡한 설계나 디버깅은 reasoning이 강한 모델로 전환합니다. |
| `/permissions` | Codex가 사용자 확인 없이 할 수 있는 작업 범위를 조정합니다. | 읽기 전용, 자동 승인 등 현재 작업 위험도에 맞춰 권한을 조절합니다. |
| `/plan` | 구현 전에 plan mode로 전환해 실행 계획을 먼저 받습니다. | 요구사항이 크거나 애매할 때 코드 변경 전에 범위와 접근을 잠급니다. |
| `/diff` | 현재 Git diff와 untracked file까지 보여줍니다. | 구현 후 리뷰, 커밋 전 확인, 예상 밖 변경 탐지에 씁니다. |
| `/review` | working tree를 별도 관점으로 code review합니다. | Codex가 만든 변경이나 직접 수정한 변경을 커밋하기 전에 점검합니다. |
| `/compact` | 긴 대화를 요약해 context token을 확보합니다. | 장시간 작업 후 핵심 맥락은 유지하면서 context 압박을 줄일 때 사용합니다. |
| `/mention` | 특정 파일이나 폴더를 conversation에 첨부합니다. | Codex가 반드시 봐야 하는 파일을 명시해 탐색 낭비를 줄입니다. |
| `/mcp` | 현재 session에서 사용 가능한 MCP tool/server를 확인합니다. | 외부 도구 연결 상태를 확인하고, 필요하면 `verbose`로 세부 정보를 봅니다. |
| `/init` | 현재 디렉터리에 `AGENTS.md` scaffold를 만듭니다. | repo나 하위 디렉터리별 지속 지침을 만들 때 시작점으로 사용합니다. |

## 전체 명령어

### 세션 제어

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/clear` | terminal 화면과 현재 chat 흐름을 새로 시작합니다. | 현재 대화를 초기화하고 같은 CLI session에서 깨끗하게 다시 시작할 때 씁니다. |
| `/new` | 같은 CLI session 안에서 새 conversation을 시작합니다. | repo는 유지하되 chat context를 새로 잡고 싶을 때 사용합니다. |
| `/resume` | 저장된 이전 conversation을 session list에서 이어갑니다. | 이전 작업을 중단 지점부터 이어가고 싶을 때 사용합니다. |
| `/fork` | 현재 conversation을 새 thread로 분기합니다. | 기존 맥락은 보존하면서 다른 접근을 실험할 때 적합합니다. |
| `/side` | main transcript를 흐트러뜨리지 않는 임시 side conversation을 시작합니다. | 본 작업과 분리된 짧은 질문이나 확인을 할 때 사용합니다. |
| `/quit` | Codex CLI를 종료합니다. | 중요한 변경은 diff, commit, 저장 상태를 확인한 뒤 종료합니다. |
| `/exit` | `/quit`과 동일하게 Codex CLI를 종료합니다. | 종료 명령의 다른 이름이며 동작은 `/quit`과 같습니다. |

### 모델, 속도, 응답 스타일

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/model` | active model과 reasoning effort를 선택합니다. | 작업 난이도와 비용, 속도 요구에 맞춰 모델을 바꿉니다. |
| `/fast` | 지원 모델에서 Fast mode를 켜거나 끕니다. | 짧은 확인이나 반복 작업은 켜고, 깊은 추론이 필요하면 끕니다. |
| `/personality` | Codex 응답 스타일을 선택합니다. | 더 간결하게, 더 설명적으로 등 협업 톤을 조정할 때 사용합니다. |

### 권한, sandbox, 실행 상태

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/permissions` | command approval과 작업 권한 수준을 조정합니다. | 위험한 변경 전에는 보수적으로, 반복 검증 중에는 필요한 만큼 완화합니다. |
| `/sandbox-add-read-dir` | sandbox가 추가 directory를 읽을 수 있게 합니다. | Windows에서 current readable roots 밖의 절대 경로를 읽어야 할 때 사용합니다. |
| `/ps` | experimental background terminal과 최근 output을 봅니다. | 오래 도는 dev server, test, watch command 상태를 확인합니다. |
| `/stop` | 현재 session이 시작한 background terminal 작업을 중지합니다. | 불필요해진 장기 실행 command를 정리할 때 사용합니다. |
| `/status` | session 설정, token, context, writable root 등을 표시합니다. | Codex가 어떤 조건으로 움직이는지 확인하는 기본 진단 명령입니다. |
| `/debug-config` | config layer와 policy requirement 진단을 출력합니다. | 설정 우선순위나 experimental network 제약이 의심될 때 확인합니다. |

### 변경 확인과 리뷰

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/diff` | Git diff와 untracked file을 함께 보여줍니다. | 작업 중간과 끝에 변경 범위가 의도와 맞는지 확인합니다. |
| `/review` | working tree를 code review 관점으로 점검합니다. | 버그, 회귀, 누락 테스트를 찾고 싶을 때 실행합니다. |
| `/copy` | 최신 완료된 Codex output을 clipboard에 복사합니다. | 최종 답변, plan, review 결과를 빠르게 옮길 때 사용합니다. `Ctrl+O`도 사용할 수 있습니다. |

### context, 파일, instruction

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/compact` | visible conversation을 요약해 context를 줄입니다. | 긴 작업에서 중요한 결정만 남기고 token 여유를 확보합니다. |
| `/mention` | 파일이나 폴더를 conversation에 첨부합니다. | 특정 entrypoint, spec, log, screenshot 경로를 Codex에게 정확히 지정합니다. |
| `/init` | 현재 directory에 `AGENTS.md` scaffold를 생성합니다. | repository coding rule, test command, 리뷰 방식을 지속 지침으로 남길 때 사용합니다. |

### MCP, 앱, 플러그인, hook

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/mcp` | configured MCP tool과 server 상태를 봅니다. | 연결된 외부 context/tool을 확인하고, `verbose`로 server 세부 정보를 볼 수 있습니다. |
| `/apps` | app connector를 탐색하고 prompt에 삽입합니다. | `$app-slug` 형태로 connector를 붙여 외부 앱을 사용하도록 요청합니다. |
| `/plugins` | 설치됐거나 발견 가능한 plugin을 탐색합니다. | plugin tool 확인, 추천 plugin 설치, plugin availability 관리에 씁니다. |
| `/hooks` | lifecycle hook 설정을 검토합니다. | 새 hook이나 변경된 hook을 신뢰할지, unmanaged hook을 비활성화할지 확인합니다. |

### 실험 기능과 agent thread

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/experimental` | experimental feature를 토글합니다. | subagent 같은 선택 기능을 CLI에서 켜고 끌 때 사용합니다. |
| `/agent` | active agent thread를 전환합니다. | spawned subagent thread를 살펴보거나 이어서 작업할 때 사용합니다. |
| `/goal` | long-running task의 experimental goal을 설정하거나 확인합니다. | `features.goals`가 필요하며, 긴 작업의 지속 목표를 추적할 때 씁니다. |

### UI 설정

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/statusline` | TUI status-line field를 interactive하게 설정합니다. | model, context, git, token, session 표시 항목을 고르고 `config.toml`에 저장합니다. |
| `/title` | terminal window/tab title field를 설정합니다. | project, thread, branch, model, task progress 등을 title에 표시하고 싶을 때 씁니다. |
| `/keymap` | TUI keyboard shortcut을 remap합니다. | shortcut binding을 확인하고 원하는 키맵을 `config.toml`에 저장합니다. |

### 계정과 피드백

| 명령어 | 핵심 | 사용 팁 |
| --- | --- | --- |
| `/logout` | Codex local credential에서 sign out합니다. | 공유 머신이나 계정 전환 시 local credential을 정리합니다. |
| `/feedback` | Codex maintainer에게 feedback과 log를 보냅니다. | 버그 리포트나 진단 공유가 필요할 때 사용합니다. |

## Alias와 주의사항

- `/approvals`는 alias로 동작하지만 slash popup 목록에는 더 이상 표시되지 않습니다.
- `/quit`과 `/exit`은 같은 종료 명령입니다.
- 일부 명령어는 feature flag, OS, plan, access level에 따라 보이거나 동작하는 범위가 달라질 수 있습니다.
