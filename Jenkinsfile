pipeline {
    agent any
    tools {
        maven 'M3' // Replace 'M3' with the name you gave your Maven installation in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
