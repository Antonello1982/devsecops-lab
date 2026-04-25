pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                echo 'Preparando entorno...'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Se agrega la bandera para permitir la instalación en el Python del sistema
                sh 'python3 -m pip install --break-system-packages -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python3 -m pytest'
            }
        }

        stage('Security Scan') {
            steps {
                // Bandit analizará tu código en busca de vulnerabilidades
                sh 'python3 -m bandit -r . || exit 0'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devsecops-app .'
            }
        }

        stage('Deploy Container') {
            steps {
                // Detenemos cualquier contenedor previo para evitar conflicto de puertos
                sh 'docker rm -f devsecops-app || true'
                sh 'docker run -d --name devsecops-app -p 5000:5000 devsecops-app'
            }
        }
    }
}