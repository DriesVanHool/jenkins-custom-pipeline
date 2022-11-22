pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'jdk-17'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('10-programming-fundamentals-java') {
            steps {
                sh 'cd 10-programming-fundamentals-java && mvn -Dmaven.test.failure.ignore=true test'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}