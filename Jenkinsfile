pipeline {
    options {
        skipDefaultCheckout true
    }
    agent any
    tools {
      maven "3.9.8"
    }
    stages {
        stage('Build Config Server') {
            steps {
                script {
                    dir("configServer") {
                        sh 'mvn clean package -DskipTests'
                    }
                }
            }
        }
    }
}