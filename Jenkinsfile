pipeline {
    agent any

    options {
        skipDefaultCheckout true
    }

    environment {
        MAVEN_OPTS = "-Xms512m -Xmx1024m"
    }

    stages {
        // stage('Security Scan') {
        //     steps {
        //         sh 'mvn org.owasp:dependency-check-maven:check'
        //     }
        // }

        stage('Build') {
            steps {
                cache(path: 'maven-repo', key: "maven-${env.BRANCH_NAME}") {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Parallel Testing') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'mvn test'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh 'mvn verify'
                    }
                }

                stage('Static Analysis') {
                    steps {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                script {
                    def targetEnv = input message: 'Select environment', 
                    parameters: [choice(name: 'env', choices: ['dev', 'staging', 'prod'], description: 'Choose environment')]
                    sh "mvn deploy -P${targetEnv}"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }

        failure {
            mail to: 'team@example.com',
                 subject: "Build Failed: ${currentBuild.fullDisplayName}",
                 body: "The build ${currentBuild.fullDisplayName} has failed."
        }
    }
}

