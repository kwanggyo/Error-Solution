# Error-Solution
프로그래밍을 하면서 만나는 에러와 해결 방법 정리

<br>

# `07.11`

## cmd에 java —version를 했을 때 버전이 나오지 않는 문제

### 원인 파악

1. Path 설정 실수
   - JAVA_HOME 변수를 만들고 그곳에 설치한 Java 디렉토리를 매핑한다.
   - Path에서 `%JAVA_HOME%\\bin`과 같이 잡는다.
2. Java 이중 설정
   - 위와 같이 했는데 반영이 안되는 경우로 Path안에 또 다른 Java 경로가 있는지 확인한다.
3. cmd 새로 안 열고 변경 확인
   - 설정 한 후 cmd를 다시 열어서 확인한다.

### 해결 방법

- 2번째가 원인이었다.
- `C:\\Program Files\\Common Files\\Oracle\\Java\\javapath` 가 Path안에 들어가 있어서 인식을 못한 것이었다! → 삭제하니까 해결 !!
- 다른 해결 방법으로 `C:\\Program Files\\Common Files\\Oracle\\Java\\javapath` 부분을 아래로 내리면 된다 !

📌 참고 사이트 : https://oingdaddy.tistory.com/302

<br>

# `07.12`

## MySQL 비밀번호 설정

MySQL을 깔아서 비밀번호를 설정하려 했지만 설정하는 창이 나오지 않았다.

그 전에 8.0 버전을 깔았었는데 execute 부분을 하지 않아서 그런지 비밀번호 설정을 한 기억이 없다..

해결하기 위해 cmd에서 `myslq -uroot -p` 입력 후 Enter password이 나오면 Enter → 에러가 뜬다.

### 원인

1. 기본적으로 server 부분을 깔지 않았기 때문에 나타나는 에러이다.
2. 비밀번호를 설정하지 않았는데 설정되어 있는 것처럼 나오는 에러

### 해결 방법 1

1. 8.0을 삭제하고 5.7을 깔려고 시도 → 삭제하는 과정에서 MySQL을 하나씩 검색해서 삭제했지만 너무 많아서(10분 넘게 검색 삭제를 해도 많이 남음) → 포기! → 중간에 포기를 하게 된다면 파일이 다 망가져서 실행이 안된다..(신중하게 선택하자)
   - 컴퓨터 복구 과정 : 설정 → 고급 시스템 설정 → 시스템 복원에 나와있는 시점을 선택하여 복원
2. 삭제하는 과정을 전부 다 삭제하지 않고 `제어판, Program Files, Program Files(x86)`에 있는 MySQL을 삭제한다. → 삭제 후 MySQL 5.7 버전을 깔아준다.(필요한 버전) → 설치할 때 execute 눌러서 server 등 전부 깔아주기 !

⇒ 여기까지가 재설치 과정

### 해결 방법 2

- 재설치를 맞췄지만 여전히 `mysql -uroot -p` 부분이 정상 동작하지 않음

- `service mysql start --skip-grant-tables` 이 명령어도 동작하지 않음

  - 에러 → 로컬에서 MySQL 서버에 연결(접속)할 수 없는 에러이다.

    ```bash
    error 2003 (hy000) can't connect to mysql server on 'localhost' (10061)
    ```

1. 제어판 → 시스템 및 보안 → 관리 도구 → 서비스로 이동(검색 : services.msc)

2. MySQL을 찾고 서비스 시작

   - 시작을 눌렀을 때 에러 발생

     → 환경 변수를 설정해줘야 한다.

   - 환경변수 설정 후에도 시작이 안되고 새로운 에러 발생

     ```bash
     서비스가 로컬 컴퓨터에서 시작했다가 중지되었습니다.
     ```

     → `mysqld —initialize —console` 을 입력하면 켜진다.

     이때  —console을 붙여줘서 shell 마지막 쪽에 있는 임시 비밀번호를 저장해둔다.

     이 후 mysql -uroot -p 입력 후 위에서 저장했던 임시 비밀번호를 입력해주면 해결된다.

3. 비밀번호 변경(1234)

   ```bash
   mysql> SET PASSWORD = PASSWORD('1234');
   ```

📌 참고 사이트

- https://hoon93.tistory.com/9
- https://k-channel.tistory.com/entry/MySQL-서비스가-로컬-컴퓨터에서-시작했다가-중지되었습니다-오류-해결방법
- https://kogun82.tistory.com/122

✔️ 새로운 프로그램을 까는 경우 잘 모르겠는 부분은 검색해서 찾아볼 것,,

✔️ 에러 내용을 구글에 검색하면서 내 상황과 가장 비슷한 내용 찾기

<br>

## 이클립스 import 문제

21.06 버전의 이클립스에서 sts를 설치한 후 import → spring project Nature → lombok.jar → gradle하는 과정에서 에러 발생(import 부터 폴더가 이상함)

### 원인

1. 20.06 버전의 이클립스를 사용하라고 했는데 깔려있던 21.06버전을 사용했던 것이 문제였던 것 같다(?)

### 해결 방법

1. 에러 로그를 확인하여 찾아보는데 버전이 Java와 안 맞을 수 있다는 것을 보고 이클립스 삭제 후 20.06 버전을 재설치하였다.

   - 하는 과정에서 실수로 workspace에 들어있던 파일들이 날라갔다😭

     → 되돌리기 위해 윈도우 복원을 해봤지만 이 부분은 복원되지 않았다. 위의 MySQL은 복원 됐었는데 이건 왜 안되지..

2. 재설치 후 순차적으로 진행했더니 해결되었다. import에서 제대로 된 폴더가 떴다면 이후는 정상적으로 진행된다.

📌 참고 : 웅현이 머리

✔️ 흐름을 이해하고 진행하는 것이 중요하다! 명세만 따라가면서 하면 에러가 발생했을 때 어느 부분이 문제인지 알 수가 없다.

✔️ 파일 백업은 꼭 해두자!

<br>

## Server를 키는데 Port가 이미 사용중일 때

APPLICATION Failed TO START

![image-20210719213015202](README.assets/image-20210719213015202.png)

### 원인

1. 포트가 이미 실행 중일 때 Spring을 Run하면 실행되는 에러이다.

### 해결 방법

1. 명령 프롬포트(CMD)에서 `netstat -ano`을 실행한다.

2. Port 번호에 해당하는 PID 번호를 찾는다.

   ![image-20210719213205035](README.assets/image-20210719213205035.png)

3. `taskkill /pid 10820 /f` 명령어를 통해 종료해준다.