pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent-1.1'
    }
  }
  environment {
    HOST_CREDS = credentials('55a9dfb4-d756-446f-94d5-88c726378cd8')
    DOCKERHUB_CREDS = credentials('34d2a98c-ee5a-4a65-939e-44a8a9c18d97')
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
      }
      }
      stage ('connect to host') {
        steps {
          sh 'ssh-keygen -t rsa -q -f "$HOME/.ssh/id_rsa" -N ""'
          sh 'sshpass -p $HOST_CREDS_PWD ssh-copy-id -o StrictHostKeyChecking=no root@178.154.198.133'
          sh 'scp -i "$HOME/.ssh/id_rsa" root@178.154.198.133:/home/avasekho/tomcat-docker var/lib/jenkins/workspace/assembly_pipe/Dockerfile'
        }
      }
      stage ('build docker') {
        steps {
          sh 'cp ./target/hello-1.0.war ./ && docker build --tag=boxfuze-app .'
          sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
          sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }
      }
  }

}