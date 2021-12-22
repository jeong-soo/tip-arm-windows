ARM Windows 팁
======================

# 들어가며
한 순간의 실수로 구매해버린 ARM Windows를 이용하기 위한 팁 입니다.
[Windows on ARM documentation](https://docs.microsoft.com/en-us/windows/arm/?WT.mc_id=thomasmaurer-blog-thmaure)

# 1. 개발 환경 구성
## 자바 JDK [Microsoft Build of OpenJDK 다운로드](http://whatismarkdown.com/)
해당 링크에서 마음에 맞는 AArch64 Windows 자바를 다운받아 사용하면 됩니다.
### AArch64 Windows JDK 다운로드 링크
갱신일 : 2021-12-22
#### zip
[11.0.13](https://aka.ms/download-jdk/microsoft-jdk-11.0.13.8.1-windows-aarch64.zip)
[16.0.2](https://aka.ms/download-jdk/microsoft-jdk-16.0.2.7.1-windows-aarch64.zip)
[17.0.1](https://aka.ms/download-jdk/microsoft-jdk-17.0.1.12.1-windows-aarch64.zip)
#### msi
[11.0.13](https://aka.ms/download-jdk/microsoft-jdk-11.0.13.8.1-windows-aarch64.msi)
[16.0.2](https://aka.ms/download-jdk/microsoft-jdk-16.0.2.7.1-windows-aarch64.msi)
[17.0.1](https://aka.ms/download-jdk/microsoft-jdk-17.0.1.12.1-windows-aarch64.msi)

## VSCODE
아래 vscode 공식 사이트에서 ARM 윈도우용 파일이 있습니다.
[다운로드 링크](https://code.visualstudio.com/Download)

## Intellij
ARM Windows 지원 intellij를 준비 중 이라고 하였지만 정확한 시기는 발표하지 않았습니다.

windows 11로 넘어오며 x64 app을 지원하여 에뮬레이팅으로 정상 구동이 가능하지만, 
프리징과 에뮬레이팅으로 오는 극심한 성능저하로 사용하기 어렵습니다.

intellij는 JAVA JVM 위에서 구동이 되기 때문에 실행을 위한 여러 해결책이 나왔습니다.
여러가지 방법 중 가장 간편하여 현재 사용하고 있는 방법을 소개합니다.
[해결 방법 원본 글](https://youtrack.jetbrains.com/issue/JBR-2074)
1. AArch64 JDK를 설치 후 JAVA_HOME 설정 [Microsoft Build of OpenJDK 다운로드](http://whatismarkdown.com/)
2. mvn -version 등을 이용하여 네이티브 자바 구동 확인
3. intellij 설치 경로의 "jbr" 폴더 삭제 (x64 jvm 제거)
4. bin\idea.bat파일을 열고 "%JAVA_EXE%" 아래에 "--illegal-access=warn" 를 추가
5. bin\runnerw64.exe 파일 삭제

위 작업 완료 후 bin\idea.bat 파일을 실행하면 ARM JDK로 구동되는 intellij 볼 수 있습니다.
경고 문구가 나오지만 정상적으로 사용 가능합니다.

idea.bat으로 실행하기 번거롭거나 실행 아이콘이 마음에 들지 않는 경우 아래의 작업을 추가하여 실행할 수 있습니다.
1. bin\idea.vbs 파일 생성 후 아래 text 입력 후 저장
CreateObject("Wscript.Shell").Run """" & WScript.Arguments(0) & """", 0, False
2. 바로가기 속성 -> 대상 아래와 같이 변경
C:\Windows\System32\wscript.exe "intellij 설치 폴더\bin\idea.vbs" "intellij 설치 폴더\bin\idea.bat"
예시 : C:\Windows\System32\wscript.exe "C:\Program Files\JetBrains\IntelliJ IDEA 2021.1.3\bin\idea.vbs" "C:\Program Files\JetBrains\IntelliJ IDEA 2021.1.3\bin\idea.bat"
3. 바로가기 아이콘 intellij 설치 폴더 내 idea.ico로 변경

## WSL
AArch64 Windwos를 지원하는 프로그램을 구하기는 매우 어렵습니다.
Source를 구하더라도 빌드를 해야하는 매우 번거로운 과정이 남아있습니다.
하지만 WSL을 이용하면 리눅스에서 제공하는 방대한 양의 AArch64 프로그램을 설치하여 사용할 수 있습니다.
[Linux용 Windows 하위 시스템 설명서](https://docs.microsoft.com/ko-kr/windows/wsl/)
[이전 버전 WSL의 수동 설치 단계](https://docs.microsoft.com/ko-kr/windows/wsl/install-manual)
[ARM Windows Linux 커널 업데이트 패키지 다운로드](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi)

# 2. WSA
[Windows Subsystem for Android 소개 링크](https://docs.microsoft.com/en-us/windows/android/wsa/)