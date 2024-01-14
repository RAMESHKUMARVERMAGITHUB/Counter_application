pipeline{

    agent any 

    stages{

        stage('Git Checkout'){
           
            steps{

                script{
                 
                 git branch: 'main', url: 'https://github.com/rameshkumarvermagithub/Counter_application.git'

                }
            }
        }
        stage('Unit Test'){

             steps{

              script{
                   
                   sh 'mvn test'

                }
             }
        }
        stage('Integration Test'){

             steps{

              script{
                   
                   sh 'mvn verify -DskipUnitTests'

                }
             }
        }
        stage('Static Code Analysis'){

             steps{

              script{
                   
                  withSonarQubeEnv(credentialsId: 'sonar') {
                     
                     sh 'mvn clean package sonar:sonar'
                  }
                }
             }
        }
        stage('Quality Gate status Check'){

             steps{

              script{
                   
                   waitForQualityGate abortPipeline: false, credentialsId: 'sonar'

                }
             }
        }
        stage('Maven Build'){

             steps{

              script{
                   
                   sh 'mvn clean install'

                }
             }
        }
        stage('Docker image Building'){

             steps{

              script{
                   
                   sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                   sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rameshkumarverma/$JOB_NAME:v1.$BUILD_ID'
                   sh 'docker image tag $JOB_NAME:v1.$BUILD_ID rameshkumarverma/$JOB_NAME:latest'

                }
             }
        }
        stage('Docker image push'){

             steps{

              script{
                   withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
                     
                     sh 'docker login -u rameshkumarverma -p ${docker}'
                     sh 'docker image push rameshkumarverma/$JOB_NAME:v1.$BUILD_ID'
                     sh 'docker image push rameshkumarverma/$JOB_NAME:latest'
                  }
                }
             }
        }        
    }
}

// squ_531616e45ae941db981c0e09f5721d0ca515e4d4
