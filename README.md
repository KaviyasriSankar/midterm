MAVEN DA
pom.xml
<project xmlns="http://apache.org" xmlns:xsi="http://w3.org"
  xsi:schemaLocation="http://apache.org http://apache.org">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>maven-odd-even-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>

src/main/java/com/example/App.java
scr/test/java/com/example/AppTest.java

App.java

package com.example;

public class App {
    public String checkOddEven(int number) {
        return (number % 2 == 0) ? "Even" : "Odd";
    }
    public static void main(String[] args) {
        System.out.println("5 is " + new App().checkOddEven(5));
    }
}

AppTest.java

package com.example;

import static org.junit.Assert.assertEquals;
import org.junit.Test;

/**
 * Unit test for Odd/Even App.
 */
public class AppTest {

    @Test
    public void testEvenNumber() {
        App app = new App();
        // Assert that 4 returns "Even"
        assertEquals("Even", app.checkOddEven(4));
    }

    @Test
    public void testOddNumber() {
        App app = new App();
        // Assert that 7 returns "Odd"
        assertEquals("Odd", app.checkOddEven(7));
    }

    @Test
    public void testZero() {
        App app = new App();
        // Zero is typically considered Even
        assertEquals("Even", app.checkOddEven(0));
    }
}

1.	CLONE YOUR GIT REPOSITORY IN VSCODE
2.	In the TERMINAL type command: mvn clean test
git add .
git commit -m "Initial resume commit"
git push origin main
NOW OPEN JENKINS:

1.	NEW ITEM
2.	ENTER NAME AND SELECT PIPELINE
3.	CLICK OK

IN THE PIPELINE SCRIPT ADD THE FOLLOWING CODE:
UPDATE THE GITHUB url TO YOUR REPOSITORY URL: 

pipeline {
 agent any
 tools {
 maven 'M3'
 }
 stages {
 stage('Checkout Git') {
 steps {
 git branch: 'main',
 url: 'https://github.com/balaganeshn/simple-maven-app'
 }
 }
 stage('Build and Test') {
 steps {
 bat 'mvn clean test'
 }
 }
 }
}


