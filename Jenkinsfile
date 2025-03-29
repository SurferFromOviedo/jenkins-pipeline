pipeline {
    agent any

    environment {
        REPO_APP = 'https://github.com/mtararujs/python-greetings'
        REPO_TESTS = 'https://github.com/mtararujs/course-js-api-framework'
    }

    stages {
        stage('install-pip-deps') {
            steps {
                echo 'Cloning repository for application code...'
                bat 'git clone %REPO_APP% app'
                dir('app') {
                    echo 'Listing repository files...'
                    bat 'dir'
                    echo 'Installing pip dependencies...'
                    bat 'pip3 install -r requirements.txt'
                }
            }
        }
        
        stage('deploy-to-dev') {
            steps {
                script {
                    deploy('dev', '7001')
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script {
                    runTests('dev')
                }
            }
        }
        
        stage('deploy-to-staging') {
            steps {
                script {
                    deploy('staging', '7002')
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script {
                    runTests('staging')
                }
            }
        }
        
        stage('deploy-to-preprod') {
            steps {
                script {
                    deploy('preprod', '7003')
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script {
                    runTests('preprod')
                }
            }
        }
        
        stage('deploy-to-prod') {
            steps {
                script {
                    deploy('prod', '7004')
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script {
                    runTests('prod')
                }
            }
        }
    }
}

def deploy(envName, port) {
    echo "Deploying to ${envName} environment..."
    bat "git clone %REPO_APP% app_${envName}"
    dir("app_${envName}") {
        echo "Stopping existing service for ${envName} environment..."
        bat "pm2 delete greetings-app-${envName} & EXIT /B 0"
        echo "Starting service for ${envName} on port ${port}..."
        bat "pm2 start app.py --name greetings-app-${envName} -- --port ${port}"
    }
}

def runTests(envName) {
    echo "Running tests on ${envName} environment..."
    bat "git clone %REPO_TESTS% tests_${envName}"
    dir("tests_${envName}") {
        echo "Installing npm dependencies for tests..."
        bat "npm install"
        echo "Executing tests for ${envName} environment..."
        bat "npm run greetings greetings_${envName}"
    }
}
