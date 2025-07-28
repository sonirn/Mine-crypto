# Set the execution policy to allow script execution
Set-ExecutionPolicy Bypass -Scope Process -Force

# Define the path to the unMineable mining software installer
$installerPath = "C:\path\to\unMiner-Setup.exe" # Replace with the actual path to the installer

# Install the software silently
Start-Process -FilePath $installerPath -Args "/S" -Wait

# Remove the installer file after installation (optional)
# Remove-Item $installerPath

# Add a shortcut to the Startup folder to automatically launch the software
$shortcutPath = "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\unMiner.lnk"
$targetPath = "C:\path\to\unMiner.exe" # Replace with the actual path to the unMiner executable
$wshell = New-Object -ComObject WScript.Shell
$shortcut = $wshell.CreateShortcut($shortcutPath)
$shortcut.TargetPath = $targetPath
$shortcut.Save()
