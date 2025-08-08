pipeline {
    agent any

    stages {
        stage('Cloning Git Repository') {
            steps {
                git branch: 'main', url: 'git@github.com:ScriptFull/jk-public-gh.git'
            }
        }

        stage('Building image') {
            steps {
                script {
                    // Detecta qual Dockerfile existe
                    def dockerfilePath = fileExists('Dockerfile') ? 'Dockerfile' :
                                         fileExists('Dockerfile.txt') ? 'Dockerfile.txt' :
                                         null

                    if (dockerfilePath == null) {
                        error "❌ Nenhum Dockerfile encontrado no repositório!"
                    }

                    sh """
                        echo '📦 Construindo imagem usando ${dockerfilePath}...'
                        docker build -t webapp:${BUILD_NUMBER} -f ${dockerfilePath} .
                    """
                }
            }
        }

        stage('Deploy Application') {
            steps {
                sh """
                    #docker stop webapp_ctr || true
                    #docker rm webapp_ctr || true
                    docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}
                """
            }
        }
    }
}
