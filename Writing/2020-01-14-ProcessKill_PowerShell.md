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

# Code
```PowerShell
$Path = "D:\"
Get-ChildItem $Path -Recurse -Include "*.exe" | ForEach-Object {Stop-Process -Name $_.BaseName}
```

# PowerShell Script 실행 옵션 설정