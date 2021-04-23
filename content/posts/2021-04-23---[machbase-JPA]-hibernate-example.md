---
title: "[machbase JPA] hibernate example"
date: "2021-04-23"
template: "post"
draft: false
slug: "[machbase-JPA]-hibernate-example"
category: "machbase"
tags:
  - "#machbase"
  - "#JPA"
  - "#Spring Framework"
  - "#Java"
description: ""
---

machbase Hibernate를 사용해봅시다.

### 시스템 사양
+ cpu : AMD Ryzen 7 2700X Eight-Core Processor (16CPUs), ~3.7GHz
+ OS : Microsoft Windows 10 Education
+ JDK : 11.0.6
+ DB : machbase-fog-6.1.14.official-WINDOWS-X64-release
+ build : Maven 4.0.0
+ IDE : IntelliJ iDEA Ultimate

### 사전 준비
1. [machbase fog edition](http://dl.machbase.com/dist/6.1.14/machbase-fog-6.1.14.official-WINDOWS-X64-release.msi)을 다운로드하고 Machbase 서버를 실행합니다.
![machbase-client-01.png](/media/posts/2021-04-23---machbase-JPA-hibernate-example/machbase-client-01.png)

2. [MACHBASE/hibernate-example](https://github.com/MACHBASE/hibernate-example)에서 코드를 다운로드 받습니다.  

### pom.xml 수정
저는 jdk 11.0.6버전을 사용하므로 수정해줬습니다.
```xml
<properties>
    <java.version>11</java.version>
</properties>
```

### 테이블 생성
`machbase JPA`는 아직 DDL을 지원해주지 않아 테이블을 직접 생성해주어야 합니다.
```SQL
Mach> create table log (name varchar(32), age integer);
Created successfully.
Elapsed time: 0.172
```

### Compile & Run

```bash
mvn compile
```
컴파일합니다.

```bash
"C:\Program Files\Java\jdk-11.0.6\bin\java.exe" -Dmaven.multiModuleProjectDirectory=C:\hibernate-example\sample\log "-Dmaven.home=C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\plugins\maven\lib\maven3" "-Dclassworlds.conf=C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\plugins\maven\lib\maven3\bin\m2.conf" "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\lib\idea_rt.jar=11757:C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\bin" -Dfile.encoding=UTF-8 -classpath "C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\plugins\maven\lib\maven3\boot\plexus-classworlds-2.5.2.jar" org.codehaus.classworlds.Launcher -Didea.version2019.1.2 compile
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building log 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ log ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ log ---
[INFO] Nothing to compile - all classes are up to date
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.655 s
[INFO] Finished at: 2021-04-23T14:18:18+09:00
[INFO] Final Memory: 19M/77M
[INFO] ------------------------------------------------------------------------

Process finished with exit code 0
```
성공적으로 빌드가 됩니다.

```bash
mvn spring-boot:run
```
실행합니다.

```bash
"C:\Program Files\Java\jdk-11.0.6\bin\java.exe" -Dmaven.multiModuleProjectDirectory=C:\hibernate-example\sample\log "-Dmaven.home=C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\plugins\maven\lib\maven3" "-Dclassworlds.conf=C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\plugins\maven\lib\maven3\bin\m2.conf" "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\lib\idea_rt.jar=8195:C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\bin" -Dfile.encoding=UTF-8 -classpath "C:\Program Files\JetBrains\IntelliJ IDEA 2019.1.2\plugins\maven\lib\maven3\boot\plexus-classworlds-2.5.2.jar" org.codehaus.classworlds.Launcher -Didea.version2019.1.2 spring-boot:run
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building log 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] >>> spring-boot-maven-plugin:2.2.6.RELEASE:run (default-cli) > test-compile @ log >>>
[INFO] 
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ log ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ log ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ log ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\hibernate-example\sample\log\src\test\resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ log ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] <<< spring-boot-maven-plugin:2.2.6.RELEASE:run (default-cli) < test-compile @ log <<<
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.2.6.RELEASE:run (default-cli) @ log ---
[INFO] Attaching agents: []

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.2.6.RELEASE)

2021-04-23 13:36:29.916  INFO 21072 --- [  restartedMain] com.machbase.log.LogApplication          : Starting LogApplication on DESKTOP-K8QCH5R with PID 21072 (C:\hibernate-example\sample\log\target\classes started by JW in C:\hibernate-example\sample\log)
2021-04-23 13:36:29.919  INFO 21072 --- [  restartedMain] com.machbase.log.LogApplication          : No active profile set, falling back to default profiles: default
2021-04-23 13:36:29.960  INFO 21072 --- [  restartedMain] o.s.b.devtools.restart.ChangeableUrls    : The Class-Path manifest attribute in C:\Users\JW\.m2\repository\org\glassfish\jaxb\jaxb-runtime\2.3.2\jaxb-runtime-2.3.2.jar referenced one or more files that do not exist: file:/C:/Users/JW/.m2/repository/org/glassfish/jaxb/jaxb-runtime/2.3.2/jakarta.xml.bind-api-2.3.2.jar,file:/C:/Users/JW/.m2/repository/org/glassfish/jaxb/jaxb-runtime/2.3.2/txw2-2.3.2.jar,file:/C:/Users/JW/.m2/repository/org/glassfish/jaxb/jaxb-runtime/2.3.2/istack-commons-runtime-3.0.8.jar,file:/C:/Users/JW/.m2/repository/org/glassfish/jaxb/jaxb-runtime/2.3.2/stax-ex-1.8.1.jar,file:/C:/Users/JW/.m2/repository/org/glassfish/jaxb/jaxb-runtime/2.3.2/FastInfoset-1.2.16.jar,file:/C:/Users/JW/.m2/repository/org/glassfish/jaxb/jaxb-runtime/2.3.2/jakarta.activation-api-1.2.1.jar
2021-04-23 13:36:29.960  INFO 21072 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disable
2021-04-23 13:36:30.443  INFO 21072 --- [  restartedMain] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
2021-04-23 13:36:30.503  INFO 21072 --- [  restartedMain] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 51ms. Found 1 JPA repository interfaces.
2021-04-23 13:36:30.756 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : Driver class com.machbase.jdbc.driver found in Thread context class loader org.springframework.boot.devtools.restart.classloader.RestartClassLoader@6583e775
2021-04-23 13:36:30.847 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : HikariPool-1 - configuration:
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : allowPoolSuspension.............false
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : autoCommit......................true
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : catalog.........................none
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : connectionInitSql...............none
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : connectionTestQuery.............none
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : connectionTimeout...............30000
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : dataSource......................none
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : dataSourceClassName.............none
2021-04-23 13:36:30.849 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : dataSourceJNDI..................none
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : dataSourceProperties............{password=<masked>}
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : driverClassName................."com.machbase.jdbc.driver"
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : healthCheckProperties...........{}
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : healthCheckRegistry.............none
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : idleTimeout.....................600000
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : initializationFailTimeout.......1
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : isolateInternalQueries..........false
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : jdbcUrl.........................jdbc:machbase://localhost:5656/mhdb
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : leakDetectionThreshold..........0
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : maxLifetime.....................1800000
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : maximumPoolSize.................10
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : metricRegistry..................none
2021-04-23 13:36:30.850 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : metricsTrackerFactory...........none
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : minimumIdle.....................10
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : password........................<masked>
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : poolName........................"HikariPool-1"
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : readOnly........................false
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : registerMbeans..................false
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : scheduledExecutor...............none
2021-04-23 13:36:30.851 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : schema..........................none
2021-04-23 13:36:30.852 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : threadFactory...................internal
2021-04-23 13:36:30.852 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : transactionIsolation............default
2021-04-23 13:36:30.852 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : username........................"SYS"
2021-04-23 13:36:30.852 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.HikariConfig           : validationTimeout...............5000
2021-04-23 13:36:30.852  INFO 21072 --- [  restartedMain] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2021-04-23 13:36:30.880  INFO 21072 --- [  restartedMain] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Driver does not support get/set network timeout for connections. (null)
2021-04-23 13:36:30.884 DEBUG 21072 --- [  restartedMain] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@104e10fc
2021-04-23 13:36:30.886  INFO 21072 --- [  restartedMain] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2021-04-23 13:36:30.989 DEBUG 21072 --- [l-1 housekeeper] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Pool stats (total=1, active=0, idle=1, waiting=0)
2021-04-23 13:36:31.006 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@5d8208ac
2021-04-23 13:36:31.022 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@5345ed6c
2021-04-23 13:36:31.038 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@577a63dc
2021-04-23 13:36:31.054 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@1278e757
2021-04-23 13:36:31.070 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@5f724adc
2021-04-23 13:36:31.087 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@2320f2d3
2021-04-23 13:36:31.102 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@27c001e8
2021-04-23 13:36:31.118 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@72571818
2021-04-23 13:36:31.134 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection com.machbase.jdbc.MachConnection@3f3e8824
2021-04-23 13:36:31.134 DEBUG 21072 --- [onnection adder] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - After adding stats (total=10, active=0, idle=10, waiting=0)
2021-04-23 13:36:32.194  INFO 21072 --- [  restartedMain] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2021-04-23 13:36:32.534  INFO 21072 --- [  restartedMain] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2021-04-23 13:36:32.549  INFO 21072 --- [  restartedMain] com.machbase.log.LogApplication          : Started LogApplication in 2.988 seconds (JVM running for 3.584)
2021-04-23 13:36:32.550  INFO 21072 --- [  restartedMain] com.machbase.log.LogApplication          : TestLogApplication...

findAll()
User(_arrival_time=2021-04-23 13:36:32.622, name=John, age=5)
User(_arrival_time=2021-04-23 13:36:32.621, name=John, age=4)
User(_arrival_time=2021-04-23 13:36:32.619, name=John, age=3)
User(_arrival_time=2021-04-23 13:36:32.618, name=John, age=2)
User(_arrival_time=2021-04-23 13:36:32.617, name=John, age=1)
User(_arrival_time=2021-04-23 13:36:32.616, name=John, age=31)
User(_arrival_time=2021-04-23 13:36:32.614, name=Noah, age=28)
User(_arrival_time=2021-04-23 13:36:32.551, name=Nana, age=17)
User(_arrival_time=2021-04-23 13:29:35.78, name=John, age=5)
User(_arrival_time=2021-04-23 13:29:35.778, name=John, age=4)
User(_arrival_time=2021-04-23 13:29:35.777, name=John, age=3)
User(_arrival_time=2021-04-23 13:29:35.776, name=John, age=2)
User(_arrival_time=2021-04-23 13:29:35.775, name=John, age=1)
User(_arrival_time=2021-04-23 13:29:35.774, name=John, age=31)
User(_arrival_time=2021-04-23 13:29:35.773, name=Noah, age=28)
User(_arrival_time=2021-04-23 13:29:35.671, name=Nana, age=17)

findByName('Noah')
[User(_arrival_time=2021-04-23 13:36:32.614, name=Noah, age=28), User(_arrival_time=2021-04-23 13:29:35.773, name=Noah, age=28)]
2021-04-23 13:36:32.825  INFO 21072 --- [extShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
2021-04-23 13:36:32.829  INFO 21072 --- [extShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
2021-04-23 13:36:32.829 DEBUG 21072 --- [extShutdownHook] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Before shutdown stats (total=10, active=0, idle=10, waiting=0)
2021-04-23 13:36:32.830 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@104e10fc: (connection evicted)
2021-04-23 13:36:32.831 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@5d8208ac: (connection evicted)
2021-04-23 13:36:32.831 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@5345ed6c: (connection evicted)
2021-04-23 13:36:32.831 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@577a63dc: (connection evicted)
2021-04-23 13:36:32.832 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@1278e757: (connection evicted)
2021-04-23 13:36:32.832 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@5f724adc: (connection evicted)
2021-04-23 13:36:32.832 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@2320f2d3: (connection evicted)
2021-04-23 13:36:32.833 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@27c001e8: (connection evicted)
2021-04-23 13:36:32.833 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@72571818: (connection evicted)
2021-04-23 13:36:32.833 DEBUG 21072 --- [nnection closer] com.zaxxer.hikari.pool.PoolBase          : HikariPool-1 - Closing connection com.machbase.jdbc.MachConnection@3f3e8824: (connection evicted)
2021-04-23 13:36:32.834 DEBUG 21072 --- [extShutdownHook] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - After shutdown stats (total=0, active=0, idle=0, waiting=0)
2021-04-23 13:36:32.834  INFO 21072 --- [extShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 7.641 s
[INFO] Finished at: 2021-04-23T13:36:33+09:00
[INFO] Final Memory: 23M/87M
[INFO] ------------------------------------------------------------------------

Process finished with exit code 0
```
로그 테이블의 삽입, 조회가 잘 진행되는 걸 확인해볼 수 있습니다.

##### reference
[[MACHBASE JPA] spring data](https://www.machbase.com/kor/resource/howToUse/123)  
[MACHBASE/hibernate-example](https://github.com/MACHBASE/hibernate-example)