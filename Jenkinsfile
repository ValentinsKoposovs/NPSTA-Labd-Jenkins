pipeline {
    agent any

    stages {
        stage('install-pip-deps') {
            steps {
                script {
                    build()
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                script {
                    deploy("dev", 7001)
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script {
                    test("dev")
                }
            }
        }

        stage('deploy-to-staging') {
            steps {
                script {
                    deploy("staging", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script {
                    test("staging")
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                script {
                    deploy("preprod", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script {
                    test("preprod")
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                script {
                    deploy("prod", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script {
                    test("prod")
                }
            }
        }
    }
}

def build() {
    echo "Installing all necessary dependencies.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "python -m venv venv"
    bat "venv/Scripts/activate"
    bat "python -m pip install -r requirements.txt"
    echo "Dependencies successfully installed"
}

def deploy(String environment, int port)
{
    echo "Deployment to ${environment} environment has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "\\node_modules\\.bin\\pm2 delete greetings-app-${environment} || exit 0"
    bat "./node_modules/.bin/pm2 start app.py --name greetings-app-${environment} --interpreter python -- --port ${port}"
    echo "Deployment to ${environment} environment has finished"
}

def test(String environment)
{
    echo "Testing Sample Book Application service has started on ${environment} environment.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    bat "npm install"
    bat "npm run greetings greetings_${environment}"
    echo "Testing Sample Book Application service finished"
}