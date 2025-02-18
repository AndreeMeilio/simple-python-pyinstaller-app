pipeline {
    agent none
    environment {
        AWS_IP = '18.136.206.1'
        SSH_KEY = '/home/andreemeilioc/Documents/others/idcampuser'
        USER_NAME = 'idcampuser'
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Manual Approval Stage') {
            steps {
                input message: "Lanjutkan ke tahap Deploy?"
            }
        }
        stage('Deploy'){
            agent {
                docker {
                    image 'pschmitt/pyinstaller:3.9'
                    args '--entrypoint=""'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
        stage('Upload Artifact To EC2'){
            agent any
            environment {
                SSH_KEY = '~/Documents/others/idcampuser'
                USER_NAME = 'idcampuser'
                IP_AWS = '18.136.206.1'
            }
            steps {
                sh """
                    scp -i ${SSH_KEY} dist/add2vals ${USER_NAME}@${IP_AWS}:/home/idcampuser/add2vals
                """
            }
            post {
                always {
                    sh 'sleep 1m'
                }
            }
        }
    }
}
