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
docker run --rm --network bridge -v "%cd%:/usr/src" sonarsource/sonar-scanner-cli ^
-Dsonar.projectKey=smart-parking-devops ^
-Dsonar.sources=. ^
-Dsonar.host.url=http://sonarqube:9000 ^
-Dsonar.token=sqp_553637bbcfd3333d19ace0d13ec58903b6c5da45
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
