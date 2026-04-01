pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean -DskipTests compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DSkiptTests'
                sh 'test -f target/spring-petclinic-4.0.0-SNAPSHOT.jar'
            }
        }

        stage('Run') {
            steps {
                sh 'java -jar target/spring-petclinic-4.0.0-SNAPSHOT.jar --server.port=8081 & sleep 10'
                sh 'curl -f http://localhost:8081/owners'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }

        success {
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
