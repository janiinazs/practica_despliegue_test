pipeline {
    agent {
        docker { 
            image 'node:20-alpine' 
        }
    }

    stages {
        stage('Instalar dependencias') {
    steps {
        // 1. Eliminar dependencias antiguas para evitar conflictos de sistema de archivos
        sh 'rm -rf node_modules package-lock.json'
        
        // 2. Limpiar caché residual
        sh 'npm cache clean --force'
        
        // 3. Instalar desde cero de forma optimizada
        sh 'npm install --no-audit --no-fund'
    }
}

        stage('Ejecutar tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor') {
            steps {
                sh '''
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}
