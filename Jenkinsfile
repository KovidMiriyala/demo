pipeline {
    agent any

    environment {
        BUILD_DIR = 'build'
        DEPLOY_DIR = '/var/www/html'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Compiling Java code...'
                sh '''
                mkdir -p ${BUILD_DIR}
                javac -d ${BUILD_DIR} src/HelloWorld.java
                '''
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests...'
                sh '''
                javac -cp ${BUILD_DIR} -d ${BUILD_DIR} test/HelloWorldTest.java
                java -cp ${BUILD_DIR} HelloWorldTest
                '''
            }
        }

        stage('Package') {
            steps {
                echo '📦 Preparing deployment package...'
                sh '''
                mkdir -p ${BUILD_DIR}/web
                cp web/helloworld.html ${BUILD_DIR}/web/
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to local EC2 web server...'
                sh '''
                cp -r ${BUILD_DIR}/web/* ${DEPLOY_DIR}/index.html
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '🌐 Verifying deployment...'
                sh '''
                PUBLIC_IP=$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
                echo "✅ Application deployed successfully!"
                echo "🌍 Visit: http://$PUBLIC_IP/"
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '❌ Build or deployment failed. Check logs.'
        }
    }
}
