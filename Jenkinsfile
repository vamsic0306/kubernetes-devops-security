pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh 'mvn clean package -DskipTests=true'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    
    
        stage ('unit test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco execPattern: 'target/jacoco.exec'
                }
            }
        }
        stage('docker_Build_Stage') {
            steps {
                sh 'printenv'
                sh 'docker build -t vamsidevdocker/devsec:${GIT_COMMIT} .'
                sh 'docker push vamsidevdocker/devsec:${GIT_COMMIT}'
            
            }
        }
    }
}
