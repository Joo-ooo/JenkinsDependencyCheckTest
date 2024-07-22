pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git branch: 'main', url: 'https://github.com/Joo-ooo/JenkinsDependencyCheckTest'
			}
		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
			}
		}
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}