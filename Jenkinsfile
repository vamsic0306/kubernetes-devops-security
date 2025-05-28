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
                sh 'sudo dockerbuild -t VamsiDevDocker/devsec: ""$GIT_COMMIT"" .'
                sh 'sudo docker push VamsiDevDocker/devsec: ""$GIT_COMMIT"" .'
            
            }
        }
    }
}
