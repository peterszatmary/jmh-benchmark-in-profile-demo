# jmh-benchmark-demo
[![Build Status](https://travis-ci.org/peterszatmary/jmh-benchmark-in-profile-demo.svg?branch=master)](https://travis-ci.org/peterszatmary/jmh-benchmark-in-profile-demo)

[Java Microbenchmark Harness (JMH)](http://openjdk.java.net/projects/code-tools/jmh/) that runs with Junit and Maven.

Project is about how to run JMH benchmark tests from test scope with Maven separately from other
test with maven profile.

Is the second version from project [jmh-benchmark-demo](https://github.com/peterszatmary/jmh-benchmark-demo)


## How to run other tests except benchmarks

Excluding the benchmark test source files from test compiling is done with [maven-compiler-plugin]
(https://maven.apache.org/plugins/maven-compiler-plugin/). See the **testExcludes** configuration
 part with used pattern.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerVersion>${java.version}</compilerVersion>
        <source>${java.version}</source>
        <target>${java.version}</target>
    </configuration>
    <!-- excluding benchmark test source java files from test compile phase -->
    <executions>
        <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
                <goal>testCompile</goal>
            </goals>
            <configuration>
                <testExcludes>
                    <exclude>**/benchmark/*Test.java</exclude>
                    <!-- add here more if you need exclude additional files -->
                </testExcludes>
            </configuration>
        </execution>
    </executions>
</plugin>
```

You can run other than benchmark test with command
```
mvn clean test
```

Output :

```
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.szatmary.peter.junittest.ClassicJunitTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.13 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```


## How to run just benchmark tests

Excluding is also done with [maven-compiler-plugin](https://maven.apache.org/plugins/maven-compiler-plugin/).
Excluded is everything except benchmark tests.


```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerVersion>${java.version}</compilerVersion>
        <source>${java.version}</source>
        <target>${java.version}</target>
    </configuration>
    <!-- exclude tests, just benchmarks we need to have here -->
    <executions>
        <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
                <goal>testCompile</goal>
            </goals>
            <configuration>
                <testExcludes>
                    <exclude>**/junittest/*Test.java</exclude>
                    <!-- add here more if you need exclude additional files -->
                </testExcludes>
            </configuration>
        </execution>
    </executions>
</plugin>
```

You can run other than benchmark test with command
```
mvn clean test -P benchmark-test
```

Output :

```
Benchmark                   Mode  Cnt   Score   Error  Units
SampleBenchmarkTest.newWay  avgt    2  12.372          ms/op
SampleBenchmarkTest.oldWay  avgt    2  10.632          ms/op
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 7.857 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 13.193 s
[INFO] Finished at: 2017-03-05T09:21:02+01:00
[INFO] Final Memory: 16M/178M
[INFO] ------------------------------------------------------------------------
```