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
- Codex 기본 Astra/high 설정, 기존 계정을 유지하는 실행 함수, OpenAI Docs MCP 설정
- Orca·브라우저·컴퓨터 사용 공용 스킬. 기존 호스트 파일과 Orca/Herdr 연결은 보존

이 배포의 Codex 실행 함수에는 승인된 개인 운영 방식에 따라 승인·샌드박스 우회 옵션이 포함되어 있습니다. 공용 배포의 설정을 적용하기 전에 미리보기를 확인하세요.

Claude·Codex와 호스트 앱 설치, 계정 로그인, 추가 계정 경로 설정, Chrome/Sites 등 별도 플러그인의 설치·인증은 각 Mac에서 확인합니다. 계정 인증·토큰·세션·캐시·개인 업무 스킬은 이 공개 저장소에 포함하지 않습니다. 설정 파일 검증만으로 모든 외부 도구의 연결 성공을 의미하지는 않습니다.

기존 사용자 설정을 안전하게 병합할 수 없으면 적용을 중단합니다. 설정 복구는 앱의 백업 선택과 **복구…**를 사용합니다. 설치 이후 별도로 수정한 내용은 덮어쓰지 않고 충돌을 알립니다.

이 저장소는 공개 배포 전용입니다. 릴리스에는 앱 ZIP, 지침 ZIP, 각각의 서명된 승인 문서와 SHA-256 목록이 있습니다.
