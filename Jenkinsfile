pipeline{

    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages{

        stage("sonar quality check"){
            
            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonar-password') {


                        sh 'chmod +x gradlew'

                        sh './gradlew sonarqube'

                    }
                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }

                }

            }
             stage("docker build & docker push"){
            steps{
                script{
                   withCredentials([string(credentialsId: 'docker-pass', variable: 'docker-password')]) {
                             sh '''
                                docker build -t 3.110.159.121:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 3.110.159.121:8083 
                                docker push  3.110.159.121:8083/springapp:${VERSION}
                                docker rmi 34.125.214.226:8083/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
        }

    }

}

