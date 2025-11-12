pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Checking out source code...'
                git branch: 'main', url: 'https://github.com/KovidMiriyala/demo.git'
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Compiling Java code...'
                sh '''
                    mkdir -p build
                    javac -d build src/*.java
                '''
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running Java application...'
                sh '''
                    cd build
                    java HelloWorld
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Build and test successful! Your Hello World app works.'
        }
        failure {
            echo '❌ Build or test failed. Please check the logs.'
        }
    }
}
