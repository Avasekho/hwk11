pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent'
    }
  }
    stages {
      stage ('git clone') {
        steps {
          git 'https://github.com/boxfuse/boxfuse-sample-java-war-hello.git'
      }
      }
      stage ('build war') {
        steps {
          sh 'mvn package'
          sh 'ls -a'
      }
      }
      stage ('build docker') {
        steps {
          git 'https://github.com/Avasekho/docker_tomcat.git'
          sh 'ls -a'
          sh 'cp ./target/hello-1.0.war ./ && docker build --tag=boxfuze-app .'
          sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }
      }
  }

}