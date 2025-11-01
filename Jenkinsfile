pipeline {
    agent any
    
    tools {
        maven 'M3'  // This references the Maven tool configured in Jenkins Global Tool Configuration
        // Alternative: If your Maven tool has a different name, replace 'M3' with that name
        // Common names: 'M3', 'maven', 'Maven', or the exact name from Global Tool Configuration
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean compile'
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
                sh 'mvn test'
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
                sh 'mvn package -DskipTests'
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

