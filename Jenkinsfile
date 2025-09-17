pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // securely pulls from Jenkins Credentials
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                sh '''
                # Download sonar-scanner
                wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip

                # Unzip
                unzip sonar-scanner-cli-5.0.1.3006-linux.zip

                # Add to PATH
                export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin

                # Run analysis
                sonar-scanner -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }

    post {
        success {
            echo "✅ SonarCloud analysis completed!"
        }
        failure {
            echo "❌ Build failed. Check logs."
        }
    }
}
