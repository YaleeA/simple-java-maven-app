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

        stage('Test') {
            steps {
                sh 'mvn test'
            }

            post {
                always {
                    junit 'target/surefire-reports/*.xml'
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

        // stage('Deliver') { 
        //     steps {
        //         sh './jenkins/scripts/deliver.sh' 
        //     }
        // }
    }
}
