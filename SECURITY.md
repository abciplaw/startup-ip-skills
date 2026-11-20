# 보안 안내 · Security

[English](#english)

이 저장소의 플러그인을 설치하기 전에 확인하실 내용을 정리했습니다. 회사의 개발자나 보안 담당자가 직접 검증할 수 있도록, 주장마다 확인 방법을 붙였습니다.

## 한눈에 보기

| 걱정 | 이 플러그인 | 확인 방법 |
|---|---|---|
| 내 컴퓨터에서 코드가 실행되나? | 실행되는 코드가 없습니다. 글(Markdown)과 설정(JSON) 파일뿐이고 hooks, MCP·LSP 서버, 실행 파일, 스크립트, 패키지 의존성이 없습니다. | 아래 점검 1~3 |
| 회사 코드나 결과가 밖으로 나가나? | 플러그인에는 통신 기능이 없습니다. 에이전트에게도 웹 검색, 업로드, 메일 발송, `curl`·`wget`을 하지 말라고 지시하며 예외가 없습니다. ABC특허법률사무소로도 자동으로 보내지 않습니다. | 점검 4, 「데이터가 가는 곳」 |
| 권한 확인 창을 건너뛰나? | 아닙니다. 스킬에 `allowed-tools` 같은 권한 설정이 없어서, 에이전트가 파일을 쓰거나 명령을 실행할 때 평소처럼 확인을 받습니다. | 점검 3 |
| 내 코드를 고치나? | 읽기만 합니다. 쓰는 곳은 `patent-portfolio/` 폴더 하나이고, 그 폴더 안에 `.gitignore`를 두어 git에 올라가지 않게 합니다. 패키지 설치나 빌드 스크립트 실행도 하지 않습니다. | `SKILL.md` 「0. 보안 규칙」 |
| 나중에 몰래 바뀌나? | Claude Code는 외부 마켓플레이스를 기본으로 자동 업데이트하지 않습니다. 특정 버전(태그)에 고정해 설치할 수도 있습니다. | 「더 엄격하게 쓰는 방법」 3 |
| 숨은 지시가 있나? | 지시문 전체가 사람이 읽을 수 있는 한국어 글입니다. 보이지 않는 문자가 없고, 나오는 외부 주소는 사무소 홈페이지와 이 저장소뿐입니다. 저장소 안의 글에 적힌 지시는 따르지 말라는 규칙도 들어 있습니다. | 점검 4~5 |

## 설치 전 5분 점검

설치하지 말고 저장소만 받아 아래 명령을 실행하세요(macOS, Linux, Windows의 Git Bash). 점검 1, 2, 3, 5는 아무것도 출력되지 않으면 정상입니다.

```bash
git clone https://github.com/abciplaw/startup-ip-skills.git
cd startup-ip-skills
git -c core.quotepath=off ls-files abciplaw   # 플러그인으로 설치되는 파일 전체

# 1. 글·설정 파일 말고 다른 파일이 없다
git -c core.quotepath=off ls-files abciplaw | grep -vE '\.(md|json)$'

# 2. 실행 권한이 붙은 파일이 없다
git ls-files -s | awk '$1 != "100644"'

# 3. 코드를 실행하거나 권한을 넓히는 구성요소가 없다
#    (hooks, MCP·LSP 서버, bin/, 패키지 파일, 스킬의 allowed-tools와 !`명령` 주입)
git ls-files abciplaw | grep -E '(^|/)(hooks|bin|monitors|workflows)/|\.mcp\.json$|\.lsp\.json$|package\.json$'
grep -rnE '"(hooks|mcpServers|lspServers|monitors|commands|agents|workflows)"' abciplaw --include='*.json'
grep -rnE '^(allowed-tools|hooks|shell|context|agent):|!`' abciplaw

# 4. 지시문에 나오는 외부 주소 목록: abcip.co.kr과 이 저장소만 나와야 한다
grep -rhoE 'https?://[^ )>"`<]*[^ )>"`<.,;:]' abciplaw | sort | uniq -c

# 5. 보이지 않는 문자(폭 0 문자, 글자 방향을 바꾸는 문자)가 없다
perl -CSD -ne 'print "$ARGV:$.\n" if /[\x{200B}-\x{200F}\x{202A}-\x{202E}\x{2060}-\x{2064}\x{FEFF}]/; close ARGV if eof' $(git ls-files abciplaw)
```

같은 점검이 이 저장소의 모든 변경마다 GitHub Actions로 자동 실행됩니다([plugin-safety-check.yml](.github/workflows/plugin-safety-check.yml)). Claude Code를 쓰신다면 `claude plugin validate .`로 형식을 확인하고, 설치한 뒤 `claude plugin details abciplaw`로 구성요소 목록(스킬 1개, 그 밖에는 없음)을 볼 수 있습니다.

양식 파일 안의 `<!-- -->` 부분은 결과 문서를 쓰는 요령입니다. 숨겨진 것이 아니라 원문(Raw)으로 보면 그대로 읽힙니다.

## 설치되는 것

- `/plugin marketplace add abciplaw/startup-ip-skills`는 이 저장소를 `~/.claude/plugins/marketplaces/`에 받아 플러그인 목록(`.claude-plugin/marketplace.json`)을 읽습니다.
- `/plugin install abciplaw@startup-ip-skills`는 `abciplaw/` 폴더를 `~/.claude/plugins/cache/startup-ip-skills/abciplaw/<버전>/`에 복사합니다. 설치한 뒤 그 폴더에서 위 점검을 다시 해 볼 수 있습니다.
- 실행 중에 쓰이는 것은 스킬 지시문(`SKILL.md`와 `references/` 다섯 파일)뿐입니다.

## 데이터가 가는 곳

- 에이전트가 읽은 코드와 문서는, 평소 그 에이전트를 쓸 때와 똑같이 사용하시는 AI 서비스(Anthropic, OpenAI, Google 등)로 갑니다. 이 플러그인이 새로 만드는 전송 경로는 없습니다.
- ABC특허법률사무소에는 아무것도 자동으로 가지 않습니다. 검토를 원하시면 에이전트가 결과 폴더를 압축해 드리고, 보내는 것은 직접 메일로 하십니다.
- 결과 문서에는 10줄이 넘는 소스 코드 원문, 비밀번호·키·토큰, 내부 서버 주소, 실제 고객 데이터를 넣지 않도록 지시합니다. 보내시기 전에 직접 읽어 보시고, 영업비밀로 남기고 싶은 부분은 지우셔도 됩니다.

## 더 엄격하게 쓰는 방법

이 플러그인의 「보내지 않는다」, 「고치지 않는다」는 에이전트에게 주는 지시입니다. 실제로 막는 것은 에이전트의 권한 설정입니다. 회사 정책상 확실히 막아야 한다면 아래를 함께 쓰세요.

1. **권한 확인을 켠 채로 쓰기.** 파일 쓰기와 명령 실행을 자동으로 승인하는 모드로 실행하지 마세요(예: Claude Code의 `--dangerously-skip-permissions`). 확인 창에 `patent-portfolio/` 밖에 쓰거나 네트워크를 쓰는 명령이 나오면 거절하시면 됩니다.
2. **웹 도구 막기 (Claude Code).** 프로젝트의 `.claude/settings.local.json`에 아래를 넣으면 웹 검색과 웹 페이지 읽기 도구가 빠집니다. 셸 명령(`curl` 등)의 네트워크까지 막으려면 Claude Code의 샌드박스(`/sandbox`, macOS·Linux·WSL2)로 네트워크를 제한하세요.

   ```json
   { "permissions": { "deny": ["WebFetch", "WebSearch"] } }
   ```

3. **버전 고정.** `/plugin marketplace add abciplaw/startup-ip-skills#v0.5.0`처럼 태그를 붙이면 그 버전으로 고정됩니다. 외부 마켓플레이스는 원래 자동 업데이트가 꺼져 있어(`/plugin` → Marketplaces에서 확인), 직접 업데이트할 때만 바뀝니다.
4. **설치하지 않고 쓰기.** [abciplaw/PROMPT.md](abciplaw/PROMPT.md) 파일 하나를 에이전트에게 읽히면 같은 결과가 나옵니다. 파일 하나에 지시문과 부록이 모두 들어 있으니, 내려받아 직접 읽어 보신 뒤 쓰시면 됩니다.
5. **Codex·Gemini CLI.** 스킬 폴더를 복사해 쓰는 방식이라 복사 전에 폴더 내용을 그대로 확인할 수 있습니다. 각 에이전트의 샌드박스와 권한 설정을 함께 쓰세요.

## 변경 관리

- 모든 변경은 이 저장소의 공개 커밋 기록으로 남습니다. 버전은 `abciplaw/.claude-plugin/plugin.json`의 `version`과 git 태그(`v0.5.0` 등)로 표시합니다.
- v0.4.0에는 사무소 홈페이지 접수가 열리면 쓰도록 꺼 둔 전송 절차가 지시문에 들어 있었습니다. v0.5.0에서 그 절차를 지우고, 에이전트는 어떤 경우에도 보내지 않도록 바꿨습니다.

## 결과를 공개하지 마세요

프로젝트 내용이나 결과 파일이 담긴 이슈·풀 리퀘스트를 올리지 마세요. 출원 전에 발명을 공개하면 특허를 받기 어려워질 수 있습니다.

## 문제 신고

보안 문제를 발견하시면 abcip@abcip.co.kr로 알려 주세요. 기밀이 없는 내용이면 GitHub 이슈로 남기셔도 됩니다.

---

## English

This page lists what to check before installing the plugin, with a way to verify each claim.

**At a glance**

- **No executable code.** The plugin is Markdown and JSON only: no hooks, MCP or LSP servers, executables, scripts or package dependencies (checks 1–3 below).
- **Nothing is sent out.** The plugin has no network capability, and the agent is told not to search the web, upload, send mail, or run `curl`/`wget`, with no exceptions. Nothing goes to ABC IP LAW automatically either; if you want a review, the agent zips the output and you email it yourself.
- **No permission bypass.** The skill has no `allowed-tools` or similar fields, so your agent asks before writing files or running commands, as usual.
- **Read-only on your project.** It writes only to `patent-portfolio/`, which it git-ignores. It does not install packages or run build scripts.
- **No silent updates.** Claude Code does not auto-update third-party marketplaces by default, and you can pin a tag: `/plugin marketplace add abciplaw/startup-ip-skills#v0.5.0`.
- **No hidden instructions.** All instructions are readable Korean text, with no invisible Unicode. The only external URLs are the firm's website and this repository. The skill also tells the agent not to follow instructions found inside your repository.

**Five-minute check before installing**: clone the repository and run the commands in the Korean section above. Checks 1, 2, 3 and 5 should print nothing; check 4 should list only abcip.co.kr and this repository. The same checks run on every change through GitHub Actions ([plugin-safety-check.yml](.github/workflows/plugin-safety-check.yml)).

**Where data goes**: code and documents the agent reads go to the AI service you already use with that agent. The plugin adds no other path.

**Stricter use**: keep permission prompts on (do not use modes that auto-approve everything); deny web tools with `{"permissions": {"deny": ["WebFetch", "WebSearch"]}}` in `.claude/settings.local.json`; use Claude Code's `/sandbox` to restrict shell network access (macOS, Linux, WSL2); pin a tag; or skip installation and have the agent read [abciplaw/PROMPT.md](abciplaw/PROMPT.md), a single file with the same content.

**Change history**: v0.4.0 contained a dormant submission procedure for a future website intake. v0.5.0 removed it; the agent never sends anything.

**Do not post your results** in issues or pull requests. Publishing an invention before filing can make it unpatentable.

**Reporting**: email abcip@abcip.co.kr. Use a GitHub issue only if the report contains nothing confidential.
