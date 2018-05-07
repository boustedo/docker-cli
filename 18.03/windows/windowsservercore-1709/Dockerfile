FROM microsoft/windowsservercore:1709

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ENV DOCKER_HOME C:\\docker
RUN $newPath = ('{0};{1}' -f $env:DOCKER_HOME, $env:PATH); \
	Write-Host ('Updating PATH: {0}' -f $newPath); \
	setx /M PATH $newPath;

ENV DOCKER_VERSION 18.03.0-ce

RUN $url = 'http://constexpr.org/innoextract/files/innoextract-1.6-windows.zip'; \
    Write-Host ('Downloading {0} ...' -f $url); \
    Invoke-WebRequest $url -OutFile 'innoextract.zip' -UseBasicParsing; \
    Write-Host 'Expanding ...'; \
    Expand-Archive innoextract.zip -DestinationPath C:\innoextract; \
    Remove-Item -Path innoextract.zip -Force; \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    $url = ('https://github.com/docker/toolbox/releases/download/v{0}/DockerToolbox-{0}.exe' -f $env:DOCKER_VERSION); \
    Write-Host ('Downloading {0} ...' -f $url); \
    Invoke-WebRequest $url -OutFile 'dockertoolbox.exe' -UseBasicParsing; \
    /innoextract/innoextract.exe --include docker.exe dockertoolbox.exe; \
    Rename-Item -Path app -NewName docker; \
    Remove-Item -Path dockertoolbox.exe -Force; \
    Remove-Item -Path innoextract -Recurse -Force; \
    Write-Host 'Verifying install ...'; \
	Write-Host '  docker --version'; docker --version; \
	Write-Host 'Complete.';

CMD [ "docker.exe" ]