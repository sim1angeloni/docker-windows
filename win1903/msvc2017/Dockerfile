# escape=`

# Windows image (build 1903)
FROM mcr.microsoft.com/windows:1903

# Fix Windows shell for correct batch processing
SHELL ["cmd", "/S", "/C"]

WORKDIR C:\

# =======================================================
# VS installer log collector
# =======================================================
# If you want to collect the log files, edit the RUN command as following:
#     RUN vs_LogCollector.cmd vs_BuildTools.exe ...
# and uncomment the next two lines:
#     COPY vs_LogCollector.cmd C:\
#     ADD https://aka.ms/vscollect.exe vs_Collect.exe
# A file called vs_Logs.zip will be created in C:\
# =======================================================

# VS BuildTools (MSVC 2017 - version 15)
COPY .vsconfig C:\
ADD https://aka.ms/vs/15/release/vs_BuildTools.exe vs_BuildTools.exe
RUN vs_BuildTools.exe --config C:\.vsconfig --quiet --noUpdateInstaller --wait --norestart --nocache && `
    del /F vs_BuildTools.exe && `
    del /F .vsconfig

# =======================================================
# VS installer log collector
# =======================================================
#     RUN del /F vs_LogCollector.cmd && del /F vs_Collect.exe
# =======================================================

# Activate VS devenv
ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\BuildTools\\VC\\Auxiliary\\Build\\vcvarsall.bat", "x64", "&&", "cmd"]
