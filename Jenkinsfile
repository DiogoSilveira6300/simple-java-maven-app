pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('Sonar') {
            steps {
                sh 'mvn sonar:sonar \
                    -Dsonar.projectKey=com.mycompany.app:my-app \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=76e5a1ba600f7064dbfa54f494d85bc3ad496eea'
            }
        }
    }
}