# 기본 정보

## 발생 일시

2025.05.18

## 빌생 환경

* MySQL Server 8.0
* MySQL Installer.exe

## 문제 요약

MySQL Installer를 실행 하던 도중 `installing database(may take a long time)`에서 아래와 같은 창을 띄우며 오류를 발생시켰다.
![img.png](img/VCRUNTIME140_1.dll 오류.png)

## 상세 설명

아래와 같은 로그가 띄워지며 실패됨

```
규칙을 1개 삭제했습니다.

확인됨


Adding a Windows Firewall rule for MySQL80 on port 3306.

Attempting to add a Windows Firewall rule with command: netsh.exe advfirewall firewall add rule name="Port 3306" protocol=TCP localport=3306 dir=in action=allow

확인됨


Successfully added the Windows Firewall rule.

Attempting to delete a Windows Firewall rule with command: netsh.exe advfirewall firewall delete rule name="Port 33060" protocol=TCP localport=33060


규칙을 1개 삭제했습니다.

확인됨


Adding a Windows Firewall rule for MySQL80 on port 33060.

Attempting to add a Windows Firewall rule with command: netsh.exe advfirewall firewall add rule name="Port 33060" protocol=TCP localport=33060 dir=in action=allow

확인됨


Successfully added the Windows Firewall rule.

Ended configuration step: Updating Windows Firewall rules


Beginning configuration step: Adjusting Windows service


Attempting to grant the required filesystem permissions to the 'NT AUTHORITY\NetworkService' account.

Granted permissions to the data directory.

Updating existing service...

Existing service updated

Ended configuration step: Adjusting Windows service



Beginning configuration step: Initializing database (may take a long time)



Attempting to run MySQL Server with --initialize-insecure option...

Starting process for MySQL Server 8.0.42...

Failed to start process for MySQL Server 8.0.42.

Database initialization failed.

Ended configuration step: Initializing database (may take a long time)
```

## 영향 범위

MySQL Server 8.0이 설치되지 않아 만들던 웹 애플리케이션을 테스트하는데 영향을 끼침

## 긴급도

즉시 해결 필요

<br>

# 문제 해결 과정

## 시도한 해결 방법

문제 해결을 위해 어떤 단계를 거쳤는지 순서대로 자세하게 기록합니다. 각 시도에 대한 결과(성공/실패)도 함께 기록하는 것이 중요합니다.
(예: 1. 서버 재부팅 -> 실패, 2. 네트워크 케이블 점검 -> 정상,3. 웹 서버 로그 확인 -> 특정 오류 발견)

1. MySQL Server 8.0 삭제 후 재설치 - 실패
2. `my.ini`파일 설치 여부 확인 - 설치 되어있음
3. 명령 프롬프트를 관리자 권한으로 실행한 후 `sfc /scannow` 명령을 입력하고 실행 - 실패
4. Visual C++ 재배포 가능 패키지를 다시 설치 - 성공

## 사용한 도구 및 명령어

* 명령 프롬포트 - `sfc /scannow`
* [Microsoft 에서 C++ 재배포 패키지 다운](https://aka.ms/vs/17/release/vc_redist.x64.exe) - 다운로드 링크

## 특이사항

문제 해결 과정에서 CBS에 문제가 있어 복구를 하였음. 하지만 이는 MySQL Server 8.0과는 연관이 없음.

<br>

# 문제 해결 결과 기록

## 최종 해결 방법

Visual C++ 재배포 가능 패키지를 다시 설치함으로써 설치에 성공함

## 원인 분석

문제의 근본적인 원인을 파악하여 기록합니다. (예: 서버 과부하, 소프트웨어 버그, 네트워크 설정 오류)

컴퓨터에 C++이 오류가 발생하여 이와 같은 문제가 발생하였다. 정확히는 x84는 있었지만 x64가 없었고, MySQL은 x64를 설치하니 설치할 수 있었다.

## 재발 방지 대책

컴퓨터에 설치되어있는 프로그램을 함부로 삭제하는 것은 어떤 문제를 일으킬지 모르니 잘 알아보고 삭제하는 것이 좋을 것이다. 이번 오류는 아마 내가 어떤 경위로 C++을 삭제했기 때문에 발생한 것이라고 생각한다.

## 소요 시간

1시간