pipeline {
    agent any
    environment {
    DOCKERHUB_CREDENTIALS = credentials('jk-dh-tk')
    }

    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', credentialsId: 'jk-gh-tk', url: 'https://github.com/felipepiton/jk-private-gh.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t felipepiton/webapp:$BUILD_NUMBER .'            
            }
        }
           
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
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
                  docker stop webapp_ctr || true
                  docker rm -f webapp_ctr || true
                  docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}
                '''
            }
        }
    }
}
