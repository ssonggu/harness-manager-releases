# Harness Manager

Apple Silicon Mac에서 Claude·Codex의 공통 작업 지침, Claude 상태 표시, 모델·추론 설정과 공용 스킬을 적용하고 업데이트하는 앱입니다. macOS 14 이상이 필요합니다.

[최신 앱 다운로드](https://github.com/ssonggu/harness-manager-releases/releases/latest/download/Harness-Manager-macOS-arm64.zip) · [릴리스 목록](https://github.com/ssonggu/harness-manager-releases/releases)

## 설치

1. ZIP을 내려받아 압축을 풉니다.
2. `Harness Manager.app`을 **응용 프로그램** 폴더로 옮겨 엽니다. 기존 1.0.3 이하 앱은 종료하고 한 번 교체합니다.
3. **설치 미리보기 → 적용… → 확인**을 누릅니다. 완료 메시지와 **검증** 결과를 확인합니다.
4. 터미널을 새로 열고 Claude·Codex도 새 세션에서 시작합니다. 기존 세션은 자동으로 재시작하지 않습니다.

앱은 아직 Apple 공증을 받지 않았습니다. macOS가 개발자 확인 때문에 차단하면 앱을 한 번 연 직후 **시스템 설정 → 개인정보 보호 및 보안 → 그래도 열기**를 사용합니다. [Apple의 공식 안내](https://support.apple.com/ko-kr/102445)를 따르세요.

터미널로 다운로드하려면 다음 명령을 사용합니다. GitHub 로그인이나 `gh` 설치는 필요하지 않습니다. 다운로드 폴더가 열리면 위 2번부터 진행합니다.

```sh
harness_download_dir="$(mktemp -d "$HOME/Downloads/harness-manager.XXXXXX")" &&
curl --fail --location --proto '=https' --proto-redir '=https' \
  'https://github.com/ssonggu/harness-manager-releases/releases/latest/download/Harness-Manager-macOS-arm64.zip' \
  --output "$harness_download_dir/Harness-Manager-macOS-arm64.zip" &&
ditto -x -k "$harness_download_dir/Harness-Manager-macOS-arm64.zip" "$harness_download_dir" &&
open "$harness_download_dir"
```

## 업데이트

**업데이트 확인**으로 앱과 지침의 최신 승인 버전을 함께 확인합니다. 앱이 있으면 **앱 설치 및 재시작…**을, 지침은 미리보기 후 **적용…**을 누릅니다. 서명과 파일 해시를 검증한 뒤 설치하며 이전 앱과 설정은 백업합니다.

기본 자동 확인은 **알림**입니다. 앱을 실행해 둔 동안 주기적으로 확인하며 로그인 시 실행도 선택할 수 있습니다. 앱 교체에는 확인이 필요합니다. 지침의 자동 적용을 선택해도 실행 중 작업이나 설정 충돌이 확인되면 보류합니다. 앱을 완전히 종료하면 자동 확인도 중지됩니다.

공개 다운로드에 토큰을 사용하지 않습니다. 다른 Mac도 같은 릴리스를 적용하면 공통 설정이 맞춰지며, 꺼진 Mac까지 동시에 바뀌지는 않습니다.

## 적용 범위

- 공통 작업 지침과 Claude 전용 규칙·프로젝트 템플릿
- Claude 하단 모델·추론·컨텍스트 표시, 공통 모델·환경 설정
- Codex 기본 Astra/high 설정, 하단 모델·추론·5시간/주간 한도 표시, 기존 계정을 유지하는 실행 함수, OpenAI Docs MCP 설정
- Orca·브라우저·컴퓨터 사용 공용 스킬. 기존 호스트 파일과 Orca/Herdr 연결은 보존

이 배포의 Codex 실행 함수에는 승인된 개인 운영 방식에 따라 승인·샌드박스 우회 옵션이 포함되어 있습니다. 공용 배포의 설정을 적용하기 전에 미리보기를 확인하세요.

계정 명령은 `codex`, `codex-c`, `codex-bs`, `codex-bc` 네 개입니다. 기존 Mac의 계정 함수가 있으면 그 연결을 유지합니다. 새 Mac의 추가 계정은 `HARNESS_CODEX_C_HOME`, `HARNESS_CODEX_BS_HOME`, `HARNESS_CODEX_BC_HOME`에 각각의 로그인 폴더를 지정합니다. 새 추가 계정은 `~/.codex-<이름>` 형식을 권장하며, 계정을 나중에 추가하면 앱에서 다시 적용합니다.

앱의 자동 탐색 대상은 `~/.codex`, `config.toml` 또는 `auth.json`이 존재하는 `~/.codex-*`, Orca의 `~/Library/Application Support/orca/codex-accounts/*/home`, 앱 실행 환경의 `CODEX_HOME`입니다. 아래 계정별 선택 파일에 명시한 홈과 이전에 관리한 기존 계정 홈도 포함합니다. `HARNESS_CODEX_*_HOME`에 임의 경로를 넣는 것만으로는 앱이 그 계정을 발견하지 않습니다. 그 밖의 경로는 CLI 배포본에서 `python3 install.py --codex-home "$HOME/my-codex-home"`으로 미리본 뒤 같은 명령에 `--apply`를 붙여 적용합니다. 계정 홈은 대상 사용자 홈 안에 있어야 합니다.

1.1.1의 `codex-b`/`codex-s` 대신 `codex`, `codex-c`, `codex-bs`, `codex-bc` 중 해당 계정의 명령을 사용합니다. 이전 `HARNESS_CODEX_B_HOME`/`HARNESS_CODEX_S_HOME`은 자동 이관되지 않으므로 기존 인증 경로를 해당 계정의 새 변수 `HARNESS_CODEX_C_HOME`/`HARNESS_CODEX_BS_HOME`/`HARNESS_CODEX_BC_HOME`에 명시적으로 설정하세요. 기존 호스트 함수와 인증 경로는 보존하며 인증 파일은 옮기지 않습니다.

Codex 하단 표시의 최초 관리 등록은 기존 하단 배열을 공통 3항목으로 교체합니다. 이전 `status_line` 영수증이 없는 1.1.1 → 1.1.2 업데이트도 여기에 해당합니다. 등록 이후 직접 수정하거나 삭제하면 충돌로 전체 적용을 쓰기 전에 중단합니다. 삭제 후 다시 적용하려면 오류에 표시된 `config.toml`의 `[tui]` 안에 아래 배열을 돌려넣고 재시도하세요. 기존 `[tui]`가 있으면 그 안에 추가하고 중복 테이블이나 키를 만들지 마세요. 다른 설정과 영수증은 삭제하지 않습니다.

```toml
[tui]
status_line = ["model-with-reasoning", "five-hour-limit", "weekly-limit"]
```

Claude·Codex와 호스트 앱 설치, 계정 로그인, 추가 계정 경로 설정, Chrome/Sites 등 별도 플러그인의 설치·인증은 각 Mac에서 확인합니다. 계정 인증·토큰·세션·캐시·개인 업무 스킬은 이 공개 저장소에 포함하지 않습니다. 설정 파일 검증만으로 모든 외부 도구의 연결 성공을 의미하지는 않습니다.

기존 사용자 설정을 안전하게 병합할 수 없으면 적용을 중단합니다. 설정 복구는 앱의 백업 선택과 **복구…**를 사용합니다. 설치 이후 별도로 수정한 내용은 덮어쓰지 않고 충돌을 알립니다.

이 저장소는 공개 배포 전용입니다. 릴리스에는 앱 ZIP, 지침 ZIP, 각각의 서명된 승인 문서와 SHA-256 목록이 있습니다.


## 이 Mac의 계정별 Codex 모델 선택

공통 기본값은 Astra/high다. 이 Mac에서만 다른 값을 쓰려면 사용자가 `~/.config/portable-harness/codex-overrides.json`을 만들거나 편집한 뒤 미리보기·적용을 다시 실행한다. 예를 들어 두 비즈니스 계정이 아래 계정 홈을 사용한다면 두 비즈니스 계정만 Sol/medium이 되고, 지정하지 않은 개인 두 계정은 Astra/high를 유지한다.

```json
{
  "schema": 1,
  "accounts": {
    ".codex-business-one": {
      "model": "gpt-5.6-sol",
      "model_reasoning_effort": "medium"
    },
    ".codex-business-two": {
      "model": "gpt-5.6-sol",
      "model_reasoning_effort": "medium"
    }
  }
}
```

키는 실제 계정 홈의 HOME 기준 상대경로다. 해당 디렉터리는 먼저 존재해야 하며 절대경로, `..`, HOME 밖 링크, 같은 계정의 중복 경로는 거부한다. 값은 `model`과 `model_reasoning_effort`만 허용하며 생략한 값에는 공통 기본값을 사용한다. 지정한 계정은 기본 탐색 경로 밖에 있어도 공통 AGENTS와 하단 표시 적용에 포함된다.

이 JSON은 사용자 소유로 설치·업데이트가 배포하거나 덮어쓰지 않는다. 같은 선택을 다른 Mac에도 쓰려면 **이 파일만** 대상 Mac의 같은 상대 위치로 수동 복사하고, 계정 홈 경로를 그 Mac에 맞춘 뒤 미리보기·적용한다. 인증 파일은 복사하지 않는다. 항목이나 파일을 삭제하고 다시 적용하면 해당 계정이 공통 기본값으로 돌아간다. `config.toml`의 관리 모델 값을 별도로 바꾸면 충돌로 전체 적용이 중단되므로 모델 변경은 이 JSON에서 한다.
