pipeline {
    agent any
    
    tools {
        maven 'Maven-3'
        jdk 'JDK-17'
    }
    
    environment {
        SONAR_PROJECT_KEY = 'barber-management'
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
        
        stage('4. SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('SonarQube') {
                    bat """
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                        -Dsonar.projectName=Barber-Management-System ^
                        -Dsonar.sources=src ^
                        -Dsonar.java.binaries=build/classes
                    """
                }
            }
        }
        
        stage('5. Package') {
            steps {
                echo 'Creating WAR/JAR file...'
                bat 'mvn package -DskipTests'
            }
        }
        
        stage('6. Archive Artifacts') {
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