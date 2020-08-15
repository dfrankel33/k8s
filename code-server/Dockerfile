FROM codercom/code-server


# Download the Linux package of PowerShell and the PowerShell extension
ADD https://github.com/PowerShell/PowerShell/releases/download/v7.0.3/powershell_7.0.3-1.ubuntu.18.04_amd64.deb /tmp/powershell.deb
# ADD https://github.com/PowerShell/vscode-powershell/archive/v2020.6.0.zip /tmp/vscode-powershell.zip

# Define ENVs for Localization/Globalization
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    # Set a fixed location for the Module analysis cache
    PSModuleAnalysisCachePath=/var/cache/microsoft/powershell/PSModuleAnalysisCache/ModuleAnalysisCache
    
USER root
RUN apt-get update \
    # Install PowerShell
    && apt-get install -y /tmp/powershell.deb \
    # Install PowerShell's dependencies
    && apt-get install -y \
    # less is required for help in PowerShell
        less \
    # Required for SSL
    #    ca-certificates \
    #    gss-ntlmssp \
    #&& apt-get dist-upgrade -y \
    #&& apt-get clean \
    # && rm -rf /var/lib/apt/lists/* \
    # Cleanup PowerShell package
    && rm /tmp/powershell.deb \
    && pwsh -NoLogo -NoProfile -Command " \
        \$ErrorActionPreference = 'Stop' ; \
        \$ProgressPreference = 'SilentlyContinue' ; \
        #
        # Intialize powershell module cache
        #
        while(!(Test-Path -Path \$env:PSModuleAnalysisCachePath)) { \
            Write-Host "'Waiting for $env:PSModuleAnalysisCachePath'" ; \
            Start-Sleep -Seconds 6 ; \
        } ; \
        #
        # Extract and move PowerShell extension to the correct place, then cleanup
        #
        # Expand-Archive /tmp/vscode-powershell.zip /tmp/vscode-powershell/ ; \
        # \$null = New-Item -Force -ItemType Directory ~/.local/share/code-server/extensions/ ; \
        # Move-Item /tmp/vscode-powershell/extension ~/.local/share/code-server/extensions/ms-vscode.powershell-${PS_EXTENSION_VERSION} ; \
        # Remove-Item -Recurse -Force /tmp/vscode-powershell/ ; \
        "

ARG IMAGE_NAME=dfrankel33/code-server