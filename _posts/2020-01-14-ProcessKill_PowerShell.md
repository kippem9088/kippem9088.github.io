---
title: "Powershell로 특정 디렉토리의 Process 죽이기"
excerpt: "특정 디렉토리에 존재하는 모든 process를 종료해주는 PowerShell Script를 만들어봅시다."

categories:
    - PowerShell
tags:
    - PowerShell
    - Windows
    - Process
last_modified_at: 
---

# PowerShell Script 실행 권한 설정
```shell
Get-ExecutionPolicy # 실행 권한 확인
Set-ExecutionPolicy # 실행 권한 설정
```
[Get-ExecutionPolicy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-executionpolicy?view=powershell-7) 명령어를 이용하여 현재 PowerShell Script를 실행할 수 있는 권한을 확인하고, [Set-ExecutionPolicy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7) 명령어로 실행 권한을 설정할 수 있다. 실행 권한을 설정하기 위해선 Powershell을 관리자 모드로 실행하여야 한다.

### Default
- Window Client에서는 **Restricted**로 설정됩니다.
- Window Server에서는 **RemoteSigned**로 설정됩니다.

### Restricted
- 개별적인 커멘드는 실행할 수 있으나, 스크립트는 실행할 수 없습니다.
- 포맷팅과 설정 파일 (.ps1xml), 모듈 스크립트 파일 (.psm1), 파워셸 프로필 (.ps1)을 포함한 모든 스크립트 파일을 실행할 수 없습니다.

### RemoteSigned
- 스크립트를 실행할 수 있습니다.
- E-Mail이나 기타 메세지 프로그램을 포함한 인터넷에서 다운로드된 파일의 경우, 신뢰할 수 있는 게시자의 서명이 필요합니다.
- 인터넷에서 다운로드되지 않고 로컬에서 작성된 파일의 경우 서명에 관계없이 실행됩니다.
- Unblock-File 등의 명령어를 이용하여 제한이 해제된 스크립트의 경우, 서명이 없더라도 실행할 수 있습니다.
- 인터넷이 아닌 곳으로부터 유입된 서명이 없는 스크립트나, 서명이 되었으나 악의적인 의도를 담은 스크립트를 실행할 위험이 있습니다.

### AllSigned
- 스크립트를 실행할 수 있습니다.
- 로컬에서 작성된 스크립트를 포함하여 모든 스크립트와 설정 파일은 신뢰할 수 있는 게시자의 서명을 포함해야 합니다.
- 신뢰할 수 있는지 여부가 분류되지 않은 게시자의 경우, 실행 여부를 사용자가 선택합니다.
- 서명이 되었으나 악의적인 의도를 담은 스크립트를 실행할 위험이 있습니다.

### Bypass
- 어떠한 스크립트도 제한되지 않고, 경고도 제공되지 않으며, 사용자가 실행 여부를 판단할 수 없습니다.
- 파워셸 스크립트가 대규모 응용 프로그램에 적용되거나, 고유의 보안 체계를 갖고 있는 프로그램에 파워셸 스크립트가 적용되는 경우를 위해 디자인된 정책입니다.

### UnRestricted
- Windows가 아닌 컴퓨터를 위한 디폴트 정책이며, 이 경우 정책을 변경할 수 없습니다.
- 서명되지 않은 스크립트도 실행할 수 있으며, 악의적인 스크립트를 실행할 위험이 있습니다.
- 로컬 인트라넷에서 생성되지 않은 스크립트나 설정 파일을 실행하기 전에 사용자에게 경고합니다.

### Undefined
- 현재 범위에서 설정된 실행 정책이 없음을 의미합니다.
- 모든 범위에서 실행 정책이 **Undefined**로 설정되어 있을 경우, 디폴트 정책인 **Restricted**가 적용됩니다.


# 특정 디렉토리의 process를 종료하기 위한 소스 코드
```shell
$Path = "D:\"
Get-ChildItem $Path -Recurse -Include "*.exe" `
| ForEach-Object {Stop-Process -Name $_.BaseName}
```

# PowerShell Script 실행 옵션 설정
HKEY_CLASSES_ROOT\Microsoft.PowerShellScript.1\Shell Registry 값에 따라 PowerShell Script를 더블 클릭했을 때 실행되는 옵션이 달라진다.
- 0: 스크립트 실행
- Edit: PowerShell IDE로 편집
- Open: Notepad로 편집