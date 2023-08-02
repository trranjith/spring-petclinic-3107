pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                git branch: 'main', credentialsId: 'git-credentials', url: 'https://github.com/trranjith/spring-petclinic-3107.git'
            }
        }
        stage('Build') {
            steps {
                bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        /*stage('Code-Analysis') {
            steps {
                bat "mvn sonar:sonar -Dsonar.language=java"
            }
            post {
                success {
                    withSonarQubeEnv(credentialsId: 'sonarqube-credentials') {
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-credentials' 
                    }
                    
                }
            }
        }*/
        stage('Code-Analysis') {
            steps {
                //withSonarQubeEnv(credentialsId: 'sonarqube-credentials') {
                    bat "mvn sonar:sonar -Dsonar.language=java" 
                //}
            }
        }
        stage('Deploy-to-Tomcat') {
            steps {
                deploy adapters: [tomcat7(credentialsId: 'Tomcat-Credentials', path: '', url: 'http://localhost:8088')], contextPath: '/petclinic', war: '**/*.war'
            }
        }
    }
}