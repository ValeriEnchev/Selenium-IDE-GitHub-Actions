pipeline {
    agent any
    stages {
        stage("Restore project dependencies") {
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }
        stage("Build the project") {
            steps {
                bat 'dotnet build SeleniumIde.sln --no-restore'
            }
        }
        stage ("Run tests") {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
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