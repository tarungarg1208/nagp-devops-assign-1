pipeline {
    agent any
    options {
            timestamps()
            timeout(60)
    }
    environment {
        registry = 'tarungarg1208/nagp-devops-assign-1'
        username = 'tarungarg02'
        port = 3008
    }
    tools {
        nodejs 'nodejs'
        jdk 'Java'
        dockerTool 'Test_Docker'
    }
    stages {
            stage('checkout') {
                steps {
                    // One or more steps need to be included within the steps block.
                    checkout([$class: 'GitSCM', branches: [[name: '**']], extensions: [], userRemoteConfigs: [[credentialsId: '70c8e005-4a66-4653-b36f-7cb529170c6a', url: 'https://github.com/tarungarg1208/nagp-devops-assign-1']]])
                }
            }
            stage('npm install') {
                steps {
                    // One or more steps need to be included within the steps block.
                    bat 'npm install'
                }
            }
            stage('npm test') {
                steps {
                    // One or more steps need to be included within the steps block.
                    bat 'npm test'
                }
            }
            stage('sonar analysis') {
                steps {
                    bat '..\\..\\tools\\hudson.plugins.sonar.SonarRunnerInstallation\\SonarQubeScanner\\bin\\sonar-scanner.bat -Dsonar.host.url=http://localhost:9000 -Dsonar.login=e0dbf4fbb99dbe5b0f0900fade0aac7d1d8299ca'
                }
            }
            stage('docker build') {
                steps {
                    echo 'Building Docker Image'
                    bat "docker build -t i-${username}-master ."
                }
            }
            stage('docker tag and push') {
                steps {
                    echo 'Tagging and Moving Docker Image'
                    bat "docker tag i-${username}-master ${registry}:${BUILD_NUMBER}"
                    withDockerRegistry([credentialsId: 'DockerHub', url:'']) {
                        bat "docker push ${registry}:${BUILD_NUMBER}"
                    }
                }
            }
            stage('docker run') {
                    steps {
                    echo 'Running Docker Image'
                    bat "docker run --name c-${username}-master -d -p=${port}:7100 ${registry}:${BUILD_NUMBER}"
                    }
            }
    }
}
