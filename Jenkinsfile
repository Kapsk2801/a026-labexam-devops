pipeline {
    agent any
    
    tools {
        jdk 'JDK'  // Java JDK - configure in Jenkins Global Tool Configuration
        maven 'M3'  // Maven - configure in Jenkins Global Tool Configuration
        // Alternative names: 'JDK', 'jdk', 'Java', 'java', or your configured JDK name
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                bat 'mvn clean compile'
            }
            post {
                success {
                    echo 'Build stage completed successfully!'
                }
                failure {
                    echo 'Build stage failed!'
                }
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
            post {
                always {
                    // Publish test results
                    junit 'target/surefire-reports/*.xml'
                }
                success {
                    echo 'All tests passed!'
                }
                failure {
                    echo 'Tests failed!'
                }
            }
        }
        
        stage('Package') {
            steps {
                echo 'Packaging the application...'
                bat 'mvn package -DskipTests'
            }
            post {
                success {
                    echo 'Package created successfully!'
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution completed'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

