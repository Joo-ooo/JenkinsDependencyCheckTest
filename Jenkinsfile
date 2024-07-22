pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
        git 'Default' // Name of the Git installation in Jenkins
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Joo-ooo/JenkinsDependencyCheckTest'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube Frontend-User') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Frontend-User -Dsonar.sources=."
                    }
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint''', odcInstallation: 'OWASP Dependency-Check' // Name of the Dependency-Check installation in Jenkins
            }
        }
    }
    post {
        always {
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }
    }
}
