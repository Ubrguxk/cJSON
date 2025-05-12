pipeline {
    agent {
        docker {
            image 'gcc:13'
        }
    }

    environment {
        BUILD_DIR = 'build'
    }

    stages {
        stage('Start') {
            steps {
                echo "Start pipeline dla projektu cJSON"
                sh 'gcc --version'
            }
        }

        stage('Zależności') {
            steps {
                sh 'apt-get update && apt-get install -y cmake make'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    mkdir -p ${BUILD_DIR}
                    cd ${BUILD_DIR}
                    cmake ..
                    make
                '''
            }
        }

        stage('Testy') {
            steps {
                sh '''
                    cd ${BUILD_DIR}
                    ctest --output-on-failure | tee test_output.log
                '''
            }
            post {
                always {
                    archiveArtifacts artifacts: "${BUILD_DIR}/test_output.log", fingerprint: true
                }
            }
        }
    }
}
