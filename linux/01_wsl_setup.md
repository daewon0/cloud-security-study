**# [Cloud-Security 101] WSL2 설치 및 클라우드 보안 실습 환경 구축**
> 
> **1. 목적**
> - Windows 환경에서 Linux 바이너리를 네이티브 수준으로 실행할 수 있는 WSL2(Windows Subsystem for Linux 2)를 구축함.
> - 클라우드 인프라 운영 및 보안 도구(Terraform, AWS CLI, kubectl 등)를 운용하기 위한 격리된 실습 환경을 확보함.
>
> **2. 환경**
> - OS: Windows 11 Pro
> - Virtualization: BIOS/UEFI 가상화 활성화 상태
> 
> **3. 실행 과정 및 로그**
> - WSL 설치 및 기본 배포판(Ubuntu) 구성
> ```bash
> # 1. WSL 기능 활성화 및 설치
> PS> wsl --install
> 
> # 2. 설치된 배포판 확인 및 버전 체크
> PS> wsl --list --verbose
>   NAME      STATE           VERSION
> * Ubuntu    Running         2
> 
> # 3. 리눅스 패키지 최신화 (보안 패치 포함)
> $ sudo apt update && sudo apt upgrade -y
> ```
> > 
> **4. 트러블슈팅**
> - **문제**: `Error: 0x80370102` 발생 (가상화 미활성화)
> - **해결**: PC 재부팅 후 BIOS 설정 진입 -> Intel VT-x 또는 AMD-V 기능을 'Enabled'로 변경하여 가상화 지원 활성화 확인 후 정상 구동됨.
> 
> **5. 배운 점 (보안 통찰)**
> - **환경 격리(Isolation)**: 호스트 OS(Windows)와 분리된 Linux 커널 환경을 사용함으로써, 실습 중 발생할 수 있는 보안 위협이나 시스템 변조의 영향을 최소화할 수 있음을 이해함.
> - **표준화**: 클라우드 서버의 90% 이상이 Linux 기반임을 고려할 때, WSL2를 통해 로컬에서 클라우드와 동일한 명령 체계를 구축하는 것이 인프라 운영의 일관성(Consistency) 유지에 필수적임을 인지함.
