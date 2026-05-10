**# [Cloud-Security 101] WSL2 설치 및 환경 격리(Isolation) 분석**
> 
> **1. 목적**
> - 클라우드 네이티브 보안 실습을 위해 Windows 11 환경에서 리눅스 커널 기반의 WSL2를 구축함.
> - 호스트 운영체제와 실습 환경 간의 경계를 이해하고, 환경별 패키지 관리 도구의 차이점을 학습함.
>
> **2. 환경**
> - OS: Windows 11 Pro
> - Distribution: Ubuntu 22.04 LTS (WSL2)
> - Shell: PowerShell (Host), Bash (Guest)
> 
> **3. 실행 과정 및 로그**
> - **단계 1: WSL2 설치 및 상태 확인 (PowerShell)**
> ```powershell
> PS> wsl --install
> PS> wsl --list --verbose
>   NAME      STATE           VERSION
> * Ubuntu    Running         2
> ```
> - **단계 2: 리눅스 패키지 업데이트 (Bash)**
> ```bash
> $ sudo apt update && sudo apt upgrade -y
> ```
> 
> **4. 트러블슈팅 (Troubleshooting)**
> - **Case A: 환경 혼동으로 인한 명령어 오류**
>   - **현상**: 리눅스 터미널 내부에서 `wsl --list` 입력 시 `Command 'wsl' not found` 발생.
>   - **원인**: `wsl.exe`는 윈도우 호스트 실행 파일이며, 리눅스 쉘은 이를 기본 바이너리로 인식하지 않음.
>   - **해결**: 해당 명령은 PowerShell에서 수행하거나, 리눅스 내에서 `wsl.exe`로 명시적 확장자를 붙여 호출해야 함.
> 
> - **Case B: Windows Sudo vs Linux Sudo 혼동**
>   - **현상**: PowerShell에서 `sudo apt update` 실행 시 "Sudo is disabled on this machine" 메시지 발생.
>   - **원인**: `apt`는 리눅스 전용 도구이며, PowerShell의 `sudo`는 Windows 11의 개발자 전용 관리자 권한 기능임. 두 환경의 `sudo`는 이름만 같을 뿐 커널 공간이 다름.
>   - **해결**: 리눅스 패키지 관리는 반드시 `wsl` 쉘 진입 후 리눅스 네이티브 `sudo`를 사용해야 함.
> 
> **5. 배운 점 (보안 통찰)**
> - **계층적 방어(Defense in Depth)**: 호스트(Windows)와 게스트(WSL)의 `sudo` 권한이 분리되어 있다는 것은, 게스트 환경이 탈취되더라도 호스트 커널에 대한 즉각적인 관리자 권한 획득이 어렵도록 설계된 보안 경계(Security Boundary)임을 이해함.
> - **상호운용성(Interop)의 양면성**: 리눅스에서 윈도우 명령어를 실행할 수 있는 편리함이 있지만, 이는 반대로 공격자가 리눅스를 통해 호스트 시스템을 탐색할 수 있는 경로가 될 수 있으므로 세심한 권한 관리가 필요함을 인지함.
