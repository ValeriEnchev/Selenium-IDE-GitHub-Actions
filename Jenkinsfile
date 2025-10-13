pipeline {
    agent any
    stages {
        stage("Restore project dependencies") {
            steps {
                bat 'dotnet restore'
            }
        }
        stage("Build the project") {
            steps {
                bat 'dotnet build --no-restore'
            }
        }
        stage ("Run tests") {
            steps {
                bat 'dotnet test --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }
	post {
		always {
			archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
			junit '**/TestResults/*.trx'
		}
	}
}