pipeline {
    // 1. Evitamos que todo el pipeline corra dentro de Node
    agent none 

    stages {
        stage('Instalar dependencias') {
            // 2. Usamos Node solo para esta etapa
            agent { 
                docker { image 'node:20-alpine' } 
            }
            steps {
                sh 'rm -rf node_modules package-lock.json'
                sh 'npm cache clean --force'
                sh 'npm install --no-audit --no-fund'
            }
        }
        
        stage('Ejecutar tests') {
            // Usamos Node de nuevo para los tests
            agent { 
                docker { image 'node:20-alpine' } 
            }
            steps {
                sh 'npm test'
            }
        }
        
        stage('Construir Imagen Docker') {
            // 3. Volvemos al entorno nativo de Jenkins (que sí tiene el comando docker)
            agent any 
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }
        
        stage('Ejecutar Contenedor') {
            // Seguimos en el entorno nativo de Jenkins
            agent any 
            steps {
                // Ajusta este comando a los puertos que uses en tu aplicación
                sh 'docker run -d -p 3000:3000 --name mi-app hola-mundo-node:latest'
            }
        }
    }
}
