pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent'
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
          sh 'ls -a'
      }
      }
      stage ('build docker') {
        steps {
          sh 'sudo rsync -avz -e "ssh -i /root/.ssh/id_rsa" root@178.154.198.133:/home/avasekho/tomcat-docker var/lib/jenkins/workspace/assembly_pipe/Dockerfile'
          sh 'cp ./target/hello-1.0.war ./ && docker build --tag=boxfuze-app .'
          sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
          sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }
      }
  }

}