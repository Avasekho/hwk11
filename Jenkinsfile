pipeline {
  agent {
    docker {
      image 'avasekho/jenkins:jenkins-agent-1.4'
      args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  environment {
    DOCKERHUB_CREDS = credentials('34d2a98c-ee5a-4a65-939e-44a8a9c18d97')
  }
    stages {
      stage ('Ensure Docker is running') {
        steps {
          sh 'service docker start'
          sh 'service docker status'
        }
        }
      stage ('git clone') {
        steps {
          git 'https://github.com/boxfuse/boxfuse-sample-java-war-hello.git'
      }
      }
      stage ('build war') {
        steps {
          sh 'mvn package'
          sh 'mkdir -p ~/hwk/ && cp /var/lib/jenkins/workspace/assembly_pipe/target/hello-1.0.war ~/hwk/'
      }
      }
      stage ('connect to host') {
        steps {
          sh 'ssh-keyscan -H 178.154.196.234 >> ~/.ssh/known_hosts'
          sh 'scp root@178.154.196.234:/dockerfiles/Dockerfile ~/hwk/'
        }
      }
      stage ('build docker') {
        steps {
          sh 'cd ~/hwk/ && docker build -t boxfuze-app .'
          sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW'
          sh 'docker tag boxfuze-app avasekho/jenkins:boxfuze-app && docker push avasekho/jenkins:boxfuze-app'
      }
      }
      stage('Run docker on slave host') {
      steps {
        sh 'ssh-keyscan -H 178.154.197.91 >> ~/.ssh/known_hosts'
        sh 'ssh root@178.154.197.91 docker pull avasekho/jenkins:boxfuze-app && docker run -d -p 8080:8080 avasekho/jenkins:boxfuze-app'
      }
    }
   
      }
  }