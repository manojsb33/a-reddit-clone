pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment{
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = 'reddit-clone-app'
        RELEASE = '1.0.0'
        DOCKER_USER = 'manoj3366'
        DOCKER_PASS = 'jenkins-docker'
        IMAGE_NAME = '${APP_NAME}'
        IMAGE_TAG = '${IMAGE_NAME} ${DOCKER_USER}/${IMAGE_NAME}:latest'

    }
       
    stages {
        stage('Clean Work Space'){
            steps{
                cleanWs()
            }
        }
        stage('Git Check Out'){
            steps{
                git branch: 'main', url: 'https://github.com/manojsb33/a-reddit-clone.git'
            }
        }
        stage('Sonarqube Analysis'){
            steps{
                withSonarQubeEnv('sonarqube-server') {
                    sh '/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar-scanner/bin/sonar-scanner ' +
                           '-Dsonar.projectName=Sathya ' +
                           '-Dsonar.projectKey=Sathya'
                }
                
            }
        }
        stage('Quality gate'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token'
                }
            }
        }
        stage('Install dependencies'){
            steps{
                sh 'npm install'
            }
        }
        stage('Trivy Scan'){
            steps{
                sh 'trivy fs . > trivyfs.txt'
            }
        }
    }
}
