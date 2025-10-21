pipeline {
    agent any
    
    tools {
        maven 'Maven-3'
        jdk 'JDK-17'
    }
    
    stages {
        stage('1. Clone Repository') {
            steps {
                echo 'Cloning the repository...'
                checkout scm
            }
        }
        
        stage('2. Compile') {
            steps {
                echo 'Compiling the project...'
                bat 'mvn clean compile'
            }
        }
        
        stage('3. Run Tests') {
            steps {
                echo 'Running unit tests...'
                bat 'mvn test'
            }
        }
        
        stage('4. Package') {
            steps {
                echo 'Creating WAR/JAR file...'
                bat 'mvn package -DskipTests'
            }
        }
        
        stage('5. Archive Artifacts') {
            steps {
                echo 'Archiving the build artifacts...'
                archiveArtifacts artifacts: '**/target/*.war', allowEmptyArchive: true
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}