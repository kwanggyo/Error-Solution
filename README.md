# Error-Solution
프로그래밍을 하면서 만나는 에러와 해결 방법 정리

<br>

## cmd에 java —version를 했을 때 버전이 나오지 않는 문제

### 원인 파악

1. **Path 설정 실수**
   - JAVA_HOME 변수를 만들고 그곳에 설치한 Java 디렉토리를 매핑한다.
   - Path에서 `%JAVA_HOME%\\bin`과 같이 잡는다.
2. **Java 이중 설정**
   - 위와 같이 했는데 반영이 안되는 경우로 Path안에 또 다른 Java 경로가 있는지 확인한다.
3. **cmd 새로 안 열고 변경 확인**
   - 설정 한 후 cmd를 다시 열어서 확인한다.

### 결과

- 2번째가 원인이었다.
- `C:\\Program Files\\Common Files\\Oracle\\Java\\javapath` 가 Path안에 들어가 있어서 인식을 못한 것이었다! → 삭제하니까 해결 !!

:bulb: 참고 사이트 : https://oingdaddy.tistory.com/302