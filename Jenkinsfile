pipeline {
    agent any

    

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/jagdishmodi/hello-world-2.git'
            }
        }

  stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }     

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Add deployment script or copy JAR to server
            }
        }
        }
    

   
}

