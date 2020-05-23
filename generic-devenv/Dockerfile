# escape=`

# Windows image (build 1903) with MSVC 2017
FROM windows/1903/msvc:2017

# Fix Windows shell for correct batch processing
SHELL ["cmd", "/S", "/C"]

WORKDIR C:\

# Python 3
ADD https://www.python.org/ftp/python/3.7.7/python-3.7.7-amd64.exe python3.exe
RUN python3.exe /quiet /passive InstallAllUsers=1 PrependPath=1 Include_test=0 TargetDir="%ProgramFiles%\Python37" && `
    del /F python3.exe && `
    mklink "%ProgramFiles%\Python37\python3.exe" "%ProgramFiles%\Python37\python.exe" && `
    mklink "%ProgramFiles%\Python37\python3.7.exe" "%ProgramFiles%\Python37\python.exe" && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install pip --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install setuptools --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install --upgrade pip-tools && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install wheel && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install urllib3 --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install numpy --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install pyyaml --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install typing --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install virtualenv --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install p4python --upgrade && `
    "%ProgramFiles%\Python37\python3.exe" -m pip install conan --upgrade

# Git
ADD https://github.com/git-for-windows/git/releases/download/v2.26.2.windows.1/Git-2.26.2-64-bit.exe git.exe
RUN git.exe /SP- /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /NOICONS /DIR="%ProgramFiles%\Git" && `
    del /F git.exe && `
    setx PATH "%ProgramFiles%\Git\bin;%PATH%"

# Perforce
ADD http://www.perforce.com/downloads/perforce/r20.1/bin.ntx64/helix-p4-x64.exe p4.exe
RUN p4.exe /S /V "/quite /passive /norestart" && `
    del /F p4.exe && `
    setx PATH "%ProgramFiles%\Perforce;%PATH%"
