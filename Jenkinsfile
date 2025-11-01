pipeline {
    agent any
    
    tools {
        // Uncomment and configure the JDK name that matches your Jenkins configuration
        // jdk 'JDK'  // Java JDK - configure in Jenkins Global Tool Configuration
        maven 'M3'  // Maven - configure in Jenkins Global Tool Configuration
    }
    
    environment {
        // Set JAVA_HOME manually - update this path to match your Java installation
        // Common Windows paths:
        // 'C:\\Program Files\\Java\\jdk-21'
        // 'C:\\Program Files\\Java\\jdk-17'
        // 'C:\\Program Files\\Eclipse Adoptium\\jdk-21.0.1+12'
        // Or leave empty to use system Java if available in PATH
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-21'
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                echo "JAVA_HOME is set to: ${env.JAVA_HOME}"
                script {
                    // Use Maven wrapper which doesn't require JAVA_HOME to be set separately
                    bat 'mvnw.cmd clean compile'
                }
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
                script {
                    // Use Maven wrapper
                    bat 'mvnw.cmd test'
                }
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
                script {
                    // Use Maven wrapper
                    bat 'mvnw.cmd package -DskipTests'
                }
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

