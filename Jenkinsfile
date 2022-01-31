pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
            post {
              always {
                  junit 'target/surefire-reports/*.xml'
              }
              success{
                  archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
              }
            }
        }
        stage('Deploy') {
            steps {
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                sh 'nohup java -jar -Dserver.port=8080 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }
        }
    }
}
