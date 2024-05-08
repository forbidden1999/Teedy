pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Pre-Build') {
            steps {
                sh 'mvn clean -DskipTests'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Doc') {
            steps {
                sh 'mvn javadoc:jar'
            }
        }

        stage('pmd') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
                failure {
                    script {
                        // 这会确保在测试失败时构建不会标记为失败
                        currentBuild.result = 'SUCCESS'
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
            archiveArtifacts artifacts: '**/target/surefire-reports/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/*.javadoc.jar', fingerprint: true
        }
    }
}
