pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent'
    }
  }

    stages {

      stage ('git clone') {
        git 'https://github.com/boxfuse/boxfuse-sample-java-war-hello.git'
      }

      stage ('build war') {
        sh 'cd boxfuse-sample-java-war-hello/'
        sh 'mvn package'
      }

      stage ('build docker') {
        sh 'cp ./target/hello-1.0.war ~/  && cd ~/ && docker build --tag=boxfuze-app .'
        sh 'ssh-keyscan -H 178.154.197.214 >> ~/.ssh/known_hosts'
        sh 'scp root@178.154.198.133:/home/avasekho/tomcat-docker Dockerfile'
        sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }

  }

}