### SLF4J에 관하여

자바 로깅 프레임 워크에는 `slf4j, log4j, logback, log4j2` 등이 있다. 각 프레임워크의 차이를 알아보자.

1. #### slf4j (Simple Logging Facade For Java) 

   다른 로깅 프레임워크의 인터페이스 역할을 하는 **추상화 계층**

2. #### log4j (2015년 이후 개발 중단)

   - 가장 오래된 로깅 프레임 워크.

   - 콘솔 및 파일 출력의 형태로 로깅을 도와줌

3. #### logback

   - Slf4j 구현체

   - spring-boot-starter-web 안에 기본으로 포함되어있음

4. #### **log4j2**

   - Slf4j 구현체

   - 람다 표현식과 사용자 정의 로그 레벨도 지원한다.

   - 멀티 쓰레드 환경에서 비동기 로거를 지원 

   - 성능이 타 로깅 프레임워크보다 좋음



-----

### logback -> log4j2 변경하기

1. 충돌 방지를 위해 spring-boot-starter-web에 포함되어 있는 spring-boot-starter-logging 제외

2. log4j2 의존성 추가

   ```
   configurations {
   	compileOnly {
   		extendsFrom annotationProcessor
   	}
   	all {
   		exclude group: 'org.springframework.boot', module: 'spring-boot-starter-logging'
   	}
   }
   
   dependencies {
   	implementation 'org.springframework.boot:spring-boot-starter-log4j2'
   }
   ```

3. `log4j2.yml` 파일 생성

   ```
   # application.yml
   logging:
     config: classpath:log4j2.xml
   ```

   만약 배포 환경이 나누어져 있다면

   ```
   # application.yml
   logging:
     config: classpath:log4j2.yml
     
   # application-develop.yml
   logging:
     config: classpath:log4j2-develop.yml
   
   # application-deploy.yml
   logging:
     config: classpath:log4j2-deploy.yml
   ```

   Configuration을 설정해야 하는데 Properties, Appenders, Loggers 하위 요소도 작성해 줘야한다.

   ```xml
   # log4j2.yml : local
   Configutation:
     name: Default
     status: info
    
     Properties:
       Property:
         name: log-path
         value: "logs"
    
     Appenders:
       Console:
         name: Console_Appender
         target: SYSTEM_OUT
         PatternLayout:
           pattern: "%style{%d{yyyy-MM-dd HH:mm:ss.SSS}}{cyan} %highlight{[%-5p]}{FATAL=bg_red,
               ERROR=red, INFO=green, DEBUG=blue, TRACE=bg_yellow} [%C] %style{[%t]}{yellow}- %m%n"
    
     Loggers:
       Root:
         level: info
         AppenderRef:
           - ref: Console_Appender
           - ref: File_Appender
           - ref: RollingFile_Appender
       Logger:
         - name: com.example
           additivity: false
           level: debug
           AppenderRef:
             - ref: Console_Appender
             - ref: File_Appender
             - ref: RollingFile_Appender
   ```

   - 로그 레벨은 trace > debug > info > warn > error > fatal

---

### jcl을 안쓰는 이유
jcl은 런타임에 클래스로더를 뒤져가면서 동적으로 로그프레임워크를 바인딩함 
(그 안에 있는 복잡한 로직들이 종종 문제을 일으킴)

### slf4j
컴파일 타임에 결정하기 때문에 깔끔하고 안전하다

---

- slf4j - 최상위 추상화 객체

- spring boot starter에 있는 기본 로깅 프레임워크 logback