pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh 'mvn clean package -DskipTests=true'
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    jacoco execPattern: 'target/jacoco.exec'
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                sh 'printenv'
                sh 'docker build -t ijogun/numericapp:"$GIT_COMMIT" .'
                sh 'docker push ijogun/numericapp:"$GIT_COMMIT"'
            }
        }
    }
}
