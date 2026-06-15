pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/yashasvinichalva/smart-parking-devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '"C:\\Users\\mahan\\AppData\\Local\\Programs\\Python\\Python312\\python.exe" --version'
                bat '"C:\\Users\\mahan\\AppData\\Local\\Programs\\Python\\Python312\\python.exe" -m pip install -r requirements.txt'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t smart-parking-app .'
            }
        }

        stage('Run SonarQube Scan') {
            steps {
                bat '''
docker run --rm -v "%cd%:/usr/src" sonarsource/sonar-scanner-cli ^
-Dsonar.projectKey=smart-parking-devops ^
-Dsonar.sources=. ^
-Dsonar.host.url=http://172.17.0.2:9000 ^
-Dsonar.login=squ_9d90919b1accecaf3dbddf73a5347127f9b49b48
                '''
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker rm -f smart-parking-container || exit 0'
                bat 'docker run -d -p 5000:5000 --name smart-parking-container smart-parking-app'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
