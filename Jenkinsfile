pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent-1.3'
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
          sh 'service docker status'
          sh 'mvn package'
          sh 'mkdir /home/avasekho/hwk/ && cp /var/lib/jenkins/workspace/assembly_pipe/target/hello-1.0.war /home/avasekho/hwk/'
      }
      }
      stage ('connect to host') {
        steps {
          sh 'service docker status'
          sh 'ssh-keyscan -H 178.154.198.133 >> ~/.ssh/known_hosts'
          sh 'scp -i "/root/.ssh/id_rsa" root@178.154.198.133:/dockerfiles/Dockerfile /home/avasekho/hwk/'
        }
      }
      stage ('build docker') {
        steps {
          sh 'service docker status'
          sh 'cd /home/avasekho/hwk/ && docker build -t boxfuze-app .'
          sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
          sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }
      }
      }
  }