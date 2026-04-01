pipeline {
    agent {
        docker {
            // Maven image with JDK 17 (change to 11/8 if your project requires)
            image 'maven:3.9.6-eclipse-temurin-17'
            // Run as root to avoid permission issues writing to workspace/.m2.
            args '-u root:root -v $HOME/.m2:/root/.m2'
            reuseNode true
        }
    }

     environment {
        // Optional: speed up Maven, reduce noise
        MAVEN_OPTS = '-Dmaven.repo.local=/root/.m2/repository'
        MVN_CMD    = 'mvn -B -ntp'
    }
    

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/jagdishmodi/hello-world-2.git'
                 sh 'java -version'
                sh 'mvn -version'
            }
        }

 stage('Build + Test') {
            steps {
                sh '${MVN_CMD} clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

            stage('Parallel Tests') {
            failFast false
            parallel {
                stage('Unit Tests') {
                    steps { sh 'mvn test -Dtest=*Unit*' }
                    post { always { junit 'target/surefire-reports/*.xml' } }
                }
                stage('Integration Tests') {
                    steps { sh 'mvn verify -Dtest=*IT*' }
                }
                stage('Code Quality') {
                                        steps {
                        sh "mvn sonar:sonar -Dsonar.host.url=${SONAR_URL}"
                    }
                }
            }
        }
        stage('Package') {
            steps {
                sh '${MVN_CMD} package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // This project is packaging=war, so archive WAR (also keep JAR if produced)
                archiveArtifacts artifacts: 'target/*.war, target/*.jar', fingerprint: true
            }
        }
    }
   
}

