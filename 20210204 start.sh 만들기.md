    #!/bin/bash
    
    if [ -f ./(프로젝트 pid명).pid ]; then
        kill $(cat (프로젝트 pid명).pid)
        sleep 1
        echo "Exists process killed."
    fi
    
    JAVA_HOME="(사용할 자바가 깔려있는 경로)"
    JAVA="$JAVA_HOME/bin/java"
    JAVA_OPTS="-Xmx2048m -Xms2048m -server"
    
    nohup $JAVA $JAVA_OPTS -jar (jar또는war파일이있는절대경로)/(jar또는war파일명).(확장자) > /dev/null 2>&1 &
    
    
    echo -e 'Starting... \n'
    



### 리눅스 환경에서 서버를 쉽게 킬 수 있게 도와주는 start.sh(shell script)만들기

pid는 프로세스 id(아마도)이여서 프로젝트 내에서 설정해준 파일명을 적으면 된다. 

예를 들어 내가 스프링부트에서  spring.pid.file = applemoon.pid를 적었으면 

`if [ -f ./applemoon.pid ]; then`

라고 적으면 된다. 



```
nohup $JAVA $JAVA_OPTS -jar (jar또는war파일이있는절대경로)/(jar또는war파일명).(확장자) > /dev/null 2>&1 &
```

여기서 hohup은 프로젝트를 실행시키고 난 뒤 백그라운드에서도 돌게 해주는 명령어이다.



또한 스프링부트 기준으로 pid 파일을 생성하게끔 기본 프로젝트Application.java 파일을 변경해줘야하는데

- 기본 메인 메소드

```java
public static void main(String[] args) {
    SpringApplication.run(RimGisTestApplication.class, args);
}
```



- 변경된 메인 메소드

```java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(RimGisTestApplication.class);
    app.addListeners(new ApplicationPidFileWriter());
    app.run(args);
}
```



이 부분을 추가해줘야 pid가 자동 생성되어 start뿐만아니라 stop sh 파일을 사용할 수 있다.
