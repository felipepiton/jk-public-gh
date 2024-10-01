pipeline {
    agent any

    stages {
        stage('Cloning Git Repository') {
            steps {
                // Verifica a branch correta
                git branch: 'main', url: 'https://github.com/felipepiton/jk-public-gh.git'
            }
        }

        stage('Building Image') {
            steps {
                // Usa a variável BUILD_NUMBER para criar a tag da imagem
                sh 'docker build -t webapp:${BUILD_NUMBER} .'
            }
        }

        stage('Deploying Application') {
            steps {
                // Para o contêiner se ele estiver em execução e o remove
                sh '''
                  # docker stop webapp_ctr || true
                  # docker rm -f webapp_ctr || true
                  docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        failure {
            // Notifica quando o pipeline falha
            echo 'Pipeline failed!'
        }
    }
}
