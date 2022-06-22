pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent-1.2'
      args '-u root'
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
          sh 'ssh-keyscan -H 178.154.198.133 >> ~/.ssh/known_hosts'
          sh 'scp -i "/root/.ssh/id_rsa" root@178.154.198.133:/home/avasekho/tomcat-docker ~/hwk/Dockerfile'
          sh 'la -a ~/hwk/'
        }
      }
      }
  }