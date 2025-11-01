pipeline {
    agent any
    tools {
        maven 'M3' // Replace 'M3' with the name you gave your Maven installation in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
    }
}
