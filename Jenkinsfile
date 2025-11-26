pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                // Optional: if Python and pip are in PATH, you can uncomment this:
                // bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Run unit tests on Windows
                bat 'python -m unittest discover -s .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Create folder and copy app.py (Windows style)
                bat '''
                if not exist "%WORKSPACE%\\python-app-deploy" mkdir "%WORKSPACE%\\python-app-deploy"
                copy /Y "%WORKSPACE%\\app.py" "%WORKSPACE%\\python-app-deploy\\"
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Running application...'
                // Run Flask app in background (Windows style)
                bat '''
                cd "%WORKSPACE%\\python-app-deploy"
                start /b "" python app.py > app.log 2>&1
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Testing application...'
                // This test_app.py uses Flask test_client; no server URL needed
                bat 'python test_app.py'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
