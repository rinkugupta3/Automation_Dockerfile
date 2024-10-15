pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout from dev-env branch
                // git branch: 'dev-env', url: 'https://github.com/rinkugupta3/Automation_Dockerfile'
                // main branch
                git branch: 'main', url: 'https://github.com/rinkugupta3/Automation_Dockerfile'
            }
        }
        stage('Set up Python environment') {
            steps {
                bat "C:/Users/dhira/AppData/Local/Programs/Python/Python311/python.exe -m pip install --upgrade pip"
                bat "C:/Users/dhira/AppData/Local/Programs/Python/Python311/python.exe -m pip install -r requirements.txt"
                bat "C:/Users/dhira/AppData/Local/Programs/Python/Python311/python.exe -m pip install pytest-html"
            }
        }
        stage('Install Playwright Browsers') {
            steps {
                bat "C:/Users/dhira/AppData/Local/Programs/Python/Python311/python.exe -m playwright install"
            }
        }
        stage('Run Headless Tests') {
            steps {
                // Run headless tests from the 'tests' folder
                bat '''
                    set HEADLESS=true
                    C:/Users/dhira/AppData/Local/Programs/Python/Python311/python.exe -m pytest tests/ --html=report_playwright_bdd_headless.html --maxfail=3 --disable-warnings -v
                '''
            }
        }
        stage('Run Non-Headless Tests') {
            steps {
                // Check if Jenkins is running on Linux, using sh (shell) instead of bat for Linux
                sh '''
                    export HEADLESS=false
                     C:/Users/dhira/AppData/Local/Programs/Python/Python311/python.exe -m pytest tests_headless_false/ --html=report_playwright_bdd_nonheadless.html --maxfail=3 --disable-warnings -v
                '''
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            archiveArtifacts artifacts: 'screenshots/**/*', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}