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

DOCKER (MAKE SURE IN PIPELINE SCRIPT U GIVEN YOUR DOCKER USERNAME AND GIT URL)
DA 4:
Dockers:
Github 
deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: html-demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: html-demo
  template:
    metadata:
      labels:
        app: html-demo
    spec:
      containers:
      - name: html-demo
        image: swetab84/html-demo:latest
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: html-demo-service
spec:
  type: NodePort
  selector:
    app: html-demo
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30010

Create index.html:

<!DOCTYPE html>
<html>
<head>
<title>CI/CD Demo</title>
<style>
body{
text-align:center;
font-family:Times New Roman;
background-color:#af0d0d;
}
h1{
color:#0ef777;
}
</style>
</head>
<body>
<h1>CI/CD Pipeline Working</h1>
<h2>Deployed using Jenkins + Docker + Kubernetes</h2>
<p>This webpage is automatically deployed using a CI/CD pipeline.</p>
</body>
</html>
Create a Dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
GO to dockerhub -> In settings check kubernets check its enable and also check kubeadm
After that open Jenkins-> In Jenkins Setting --> Credentials -> system global click -> add crentencials->  (Docker hub)User name:  Password:    ID:dockerhub and then click create
add Credentials – kind:Secret file –>file(choose config in window c -user-admin-kube.-config)->id kuberconfig->description: Kubernetes crendentials-> and click create
After this : Create New Item -> Pipeline Paste the code in pipeline script - Update URL of GIT repository - Make sure credentialsId: “dockerhub” username
Pipeline: 
pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "kaviyasankar2006/html-demo"
    }
    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/prithiyangadevi-r/dockers.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat '''
                    echo %PASS% | docker login -u %USER% --password-stdin
                    docker push %DOCKER_IMAGE%
                    '''
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kuberconfig', variable: 'KUBECONFIG')]) {
                    bat '''
                    set KUBECONFIG=%KUBECONFIG%
                    kubectl apply -f deployment.yaml --validate=false
                    '''
                }
            }
        }
    }
}
and then click build now and check console output
after this open docker: builders,containers,images->take all screenshot
open that link which mam provided So check if the HTML file open in http://localhost:30010 


