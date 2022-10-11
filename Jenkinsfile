pipeline {
    environment {
        dockerSwarmManager = '10.40.1.26:2375'
        dockerhost = '10.40.1.26'
    }
    agent any
    tools {
        maven "mvn 363"
    }
    stages {
        stage('maven 363') {
          steps {
              script{ 
                  sh "mvn clean package"
              }
          }
        }
              
        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/shanmukhashan022/jenkinspipeline-dockertest.git'
            }
        }

        stage('Build Docker Image') {
          steps {
              sh 'docker build -t shanmukhashan022/new_jenkins:${BUILD_NUMBER}.'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    withCredentials([string(credentialsId: 'DOCKER', variable: 'passwd')]) {
           sh    'docker login -u shanmukhashan022 -p ${passwd}'
           sh    'docker push shanmukhashan022/new_jenkins1:${BUILD_NUMBER}'
           }
          }
        }

        stage('Deploy to Docker Host') {
          steps {
           sh "docker -H tcp://$dockerSwarmManager service rm testing1 || true"
           sh "docker -H tcp://$dockerSwarmManager service create --name testing1 -p 8000:80 -p 8000:80 shanmukhashan022/new_jenkins'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh 'curl http://$dockerhost:8100 || exit 1'
          }
        }

    }
}
