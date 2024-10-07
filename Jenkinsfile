pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
        skipDefaultCheckout true
    }

       environment {
        MAVEN_OPTS = "-Xms512m -Xmx1024m"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        // stage('Test') {
        //     steps {
        //         sh 'mvn test'
        //     }

        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }


        stage('Parallel Testing') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'mvn test'
                    }
                }
            }

            pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
        skipDefaultCheckout true
    }

       environment {
        MAVEN_OPTS = "-Xms512m -Xmx1024m"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        // stage('Test') {
        //     steps {
        //         sh 'mvn test'
        //     }

        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }


        stage('Parallel Testing') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'mvn test'
                    }
                }

            }


            stage('Integration Tests') {
                steps {
                    sh 'mvn verify'
                }
            }
        }

        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}

        }



        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
