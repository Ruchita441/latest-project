pipeline{

    agent any

    stages{

        stage("sonar quality check"){
            
            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonar-password') {


                        sh 'chmod +x gradlew'

                        sh './gradlew sonarqube'

                    }

                }

            }

        }

    }

}

