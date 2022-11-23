## SkipTests
``` java
Commands: -DskipTests VS -Dmaven.test.skip=true
"maven.test.skip.exec=true" the tests get compiled, but not executed.
"maven.test.skip=true" doesn't compile or execute the tests.
"-DskipTests" is the same as "maven.test.skip.exec=true"

<project>
  [...]
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.0.0-M7</version>
        <configuration>
          <skipTests>true</skipTests>
        </configuration>
      </plugin>
    </plugins>
  </build>
  [...]
</project>
```
