pipeline {
    options {
        skipDefaultCheckout true
    }
    agent none
    stages {


        stage('Build Config Server') {
            agent {
                docker {
                    image 'maven'
                    args '-u root -v $HOME/.m2:/root/.m2'
                }
            }
            steps {
               script {
                  dir('configServer') {
                    sh "mvn clean package -DskipTests"
                    def pom = readMavenPom file:'pom.xml'
                    env.VERSION = pom.version
                  }
               }
            }
        }


        stage('Build Discovery Server') {
            agent {
                docker {
                    image 'maven'
                    args '-u root -v $HOME/.m2:/root/.m2'
                }
            }
            steps {
               script {
                  dir('discorveryServer') {
                    sh "mvn clean package -DskipTests"
                    def pom = readMavenPom file:'pom.xml'
                    env.VERSION = pom.version
                  }
               }
            }
        }

        stage('Build Assurance Server') {
            agent {
                docker {
                    image 'maven'
                    args '-u root -v $HOME/.m2:/root/.m2'
                }
            }
            steps {
               script {
                  dir('assurance') {
                    sh "mvn clean package -DskipTests"
                    def pom = readMavenPom file:'pom.xml'
                    env.VERSION = pom.version
                  }
               }
            }
        }

        stage('Build AssurancePolicy Server') {
            agent {
                docker {
                    image 'maven'
                    args '-u root -v $HOME/.m2:/root/.m2'
                }
            }
            steps {
               script {
                  dir('assurancePolicy') {
                    sh "mvn clean package -DskipTests"
                    def pom = readMavenPom file:'pom.xml'
                    env.VERSION = pom.version
                  }
               }
            }
        }

        stage('Build Gateway Server') {
            agent {
                docker {
                    image 'maven'
                    args '-u root -v $HOME/.m2:/root/.m2'
                }
            }
            steps {
               script {
                  dir('gateway') {
                    sh "mvn clean package -DskipTests"
                    def pom = readMavenPom file:'pom.xml'
                    env.VERSION = pom.version
                  }
               }
            }
        }




            stage('Build ConfigServer Docker Image') {
                agent {
                    docker {
                        image 'docker'
                        args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                }
              steps {
                script {
                  def service = "configserver"
                  def dockerFile = """
        FROM openjdk:17
        EXPOSE 8888
        COPY /configServer/target/configServer-${env.VERSION}.jar configServer-${env.VERSION}.jar
        ENTRYPOINT ["java", "-jar", "configServer-${env.VERSION}.jar"]
        """
                  writeFile file: 'Dockerfile', text: dockerFile

                  def dockerImage = docker.build("fares121/${service}:${env.VERSION}", "-f Dockerfile .")

                  withCredentials([string(credentialsId: 'Docker', variable: 'docker_password')]) {
                    sh 'docker login -u fares121 -p ${docker_password}'
                    dockerImage.push()

                  }
                }
              }
            }
            stage('Build DiscoveryServer Docker Image') {
                agent {
                    docker {
                        image 'docker'
                        args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                }
              steps {
                script {
                  def service = "discoveryserver"
                  def dockerFile = """
        FROM openjdk:17
        COPY /discorveryServer/target/discorveryServer-${env.VERSION}.jar discoveryServer-${env.VERSION}.jar
        ENTRYPOINT ["java", "-jar", "discoveryServer-${env.VERSION}.jar"]
        """
                  writeFile file: 'Dockerfile', text: dockerFile

                  def dockerImage = docker.build("fares121/${service}:${env.VERSION}", "-f Dockerfile .")

                  withCredentials([string(credentialsId: 'Docker', variable: 'docker_password')]) {
                    sh 'docker login -u fares121 -p ${docker_password}'
                    dockerImage.push()

                  }
                }
              }
            }


            stage('Build Assurance Docker Image') {
                agent {
                    docker {
                        image 'docker'
                        args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                }
              steps {
                script {
                  def service = "assurance"
                  def dockerFile = """
        FROM openjdk:17
        COPY /assurance/target/assurance-${env.VERSION}.jar assurance-${env.VERSION}.jar
        ENTRYPOINT ["java", "-jar", "assurance-${env.VERSION}.jar"]
        """
                  writeFile file: 'Dockerfile', text: dockerFile

                  def dockerImage = docker.build("fares121/${service}:${env.VERSION}", "-f Dockerfile .")

                  withCredentials([string(credentialsId: 'Docker', variable: 'docker_password')]) {
                    sh 'docker login -u fares121 -p ${docker_password}'
                    dockerImage.push()

                  }
                }
              }
            }


            stage('Build AssurancePolicy Docker Image') {
                agent {
                    docker {
                        image 'docker'
                        args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                }
              steps {
                script {
                  def service = "assurancepolicy"
                  def dockerFile = """
        FROM openjdk:17
        COPY /assurancePolicy/target/assurancePolicy-${env.VERSION}.jar assurancePolicy-${env.VERSION}.jar
        ENTRYPOINT ["java", "-jar", "assurancePolicy-${env.VERSION}.jar"]
        """
                  writeFile file: 'Dockerfile', text: dockerFile

                  def dockerImage = docker.build("fares121/${service}:${env.VERSION}", "-f Dockerfile .")

                  withCredentials([string(credentialsId: 'Docker', variable: 'docker_password')]) {
                    sh 'docker login -u fares121 -p ${docker_password}'
                    dockerImage.push()

                  }
                }
              }
            }

            stage('Build Gateway Docker Image') {
                agent {
                    docker {
                        image 'docker'
                        args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                }
              steps {
                script {
                  def service = "gateway"
                  def dockerFile = """
        FROM openjdk:17
        COPY /gateway/target/gateway-${env.VERSION}.jar gateway-${env.VERSION}.jar
        ENTRYPOINT ["java", "-jar", "gateway-${env.VERSION}.jar"]
        """
                  writeFile file: 'Dockerfile', text: dockerFile

                  def dockerImage = docker.build("fares121/${service}:${env.VERSION}", "-f Dockerfile .")

                  withCredentials([string(credentialsId: 'Docker', variable: 'docker_password')]) {
                    sh 'docker login -u fares121 -p ${docker_password}'
                    dockerImage.push()

                  }
                }
              }
            }

    }
}