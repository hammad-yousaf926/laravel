pipeline {
    agent any
    
    environment {
        // Set environment variables if needed
        HOME = '/var/www/html' // Set the path to your Laravel project directory
        COMPOSER_HOME = "${HOME}/.composer"
        PATH = "${COMPOSER_HOME}/vendor/bin:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                script {
                    git branch: 'main', credentialsId: 'your-git-credentials-id', url: 'https://github.com/your-username/your-laravel-app.git'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install composer dependencies
                sh 'composer install --no-interaction --no-suggest'
            }
        }

        stage('Build Assets') {
            steps {
                // Run Laravel mix to build assets (CSS, JS)
                sh 'npm install'
                sh 'npm run prod'
            }
        }

        stage('Run Database Migrations') {
            steps {
                // Run database migrations if needed
                sh 'php artisan migrate --force'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy your Laravel application (you can use rsync, scp, or any other method)
                // For example, using rsync:
                sh 'rsync -av --delete ./ ${HOME}'
            }
        }
    }

    post {
        always {
            // Clean up steps, if necessary
        }

        success {
            echo 'Deployment successful! Your Laravel application is now deployed.'
        }

        failure {
            echo 'Deployment failed. Please check the logs for more information.'
        }
    }
}
