FROM mcr.microsoft.com/powershell:lts-nanoserver-1809

USER Administrator
RUN COPY "c:\program files\powershell\pwsh.exe" "c:\program files\powershell\powershell.exe" 

USER ContainerUser
COPY ./config.json ./
COPY ./agent.exe ./
COPY ./scripts ./scripts
CMD ["agent.exe", "--agent", "--container"]
