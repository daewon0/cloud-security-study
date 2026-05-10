**# [Cloud-Security 101] 리눅스 기초 보안 통제 (Access Control) 실습**
> 
> **1. 목적**
> - 클라우드 인프라의 근간이 되는 Linux 환경을 WSL2로 구축하고, 최하단 OS 레벨에서의 최소 권한 원칙(PoLP) 적용 방법을 학습함.
>
> **2. 환경**
> - OS: Windows 11, Ubuntu 22.04 (WSL2)
> 
> **3. 실행 과정 및 로그**
> - `secret.txt` 파일을 생성하고 권한을 변경함.
> ```bash
> $ echo "This is my top secret" > secret.txt
> $ ls -l secret.txt
> -rw-r--r-- 1 username username 22 May 10 16:00 secret.txt
> $ chmod 600 secret.txt
> $ ls -l secret.txt
> -rw------- 1 username username 22 May 10 16:01 secret.txt
> ```
> 
> **4. 트러블슈팅**
> - (이번 실습은 한 번에 성공하여 에러 로그 없음)
> 
> **5. 배운 점 (보안 통찰)**
> - Linux man 페이지 확인 결과, `chmod 600`에서 '6'은 소유자의 읽기(4)+쓰기(2)를 의미하며, 뒤의 '00'은 그룹과 기타 사용자의 모든 권한(0)을 박탈함을 의미함.
> - 만약 클라우드 서버의 중요 설정 파일을 `777(모든 사용자에게 모든 권한 부여)`로 설정할 경우, 비인가 접속자나 악성 스크립트가 해당 파일을 임의로 변조하여 전체 시스템 탈취로 이어질 수 있음을 인지함.
