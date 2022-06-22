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
      stage ('connect to host') {
        steps {
          sh 'scp -i "/root/.ssh/id_rsa" -o StrictHostKeyChecking=no root@178.154.198.133:/home/avasekho/tomcat-docker var/lib/jenkins/workspace/assembly_pipe/Dockerfile'
          sh 'ls -a'
        }
      }
      }
  }