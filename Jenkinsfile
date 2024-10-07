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
                // sh 'mvn -B -DskipTests clean package'
                cache(path: 'maven-repo', key: "maven-${env.BRANCH_NAME}") {
                    sh 'mvn clean install'
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

        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
}
