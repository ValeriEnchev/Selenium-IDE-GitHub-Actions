pipeline {
    agent any

    environment {
        CHROME_VERSION = '127.0.6533.73'
        CHROMEDRIVER_VERSION = '127.0.655.72'
        CHROME_INSTALL_PATH = '/Applications/Google Chrome.app/Contents/MacOS'
    }

    stages {
        stage('Install .NET SDK 6') {
            steps {
                sh '''
                echo "Installing .NET SDK 6.0"
                brew install --cask dotnet-sdk6
                '''
            }
        }

        stage('Install Google Chrome') {
            steps {
                sh '''
                echo "Installing Google Chrome version $CHROME_VERSION"
                brew install --cask google-chrome
                '''
            }
        }

        stage('Install ChromeDriver') {
            steps {
                sh '''
                echo "Downloading ChromeDriver version $CHROMEDRIVER_VERSION"
                curl -LO https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_mac64.zip
                unzip -o chromedriver_mac64.zip -d .
                mv chromedriver "$CHROME_INSTALL_PATH/chromedriver"
                chmod +x "$CHROME_INSTALL_PATH/chromedriver"
                '''
            }
        }

        stage('Restore dependencies') {
            steps {
                sh 'dotnet restore SeleniumIde.sln'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }

        stage('Run tests') {
            steps {
                sh 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
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