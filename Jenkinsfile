pipeline{
    agent{
        node{
            label "dev"
        }
    }
    environment{
        scannerHome=tool "mysonar"
    }
    
    stages{
        stage("code"){
            steps{
                git 'https://github.com/bharathperala/docker-project-phpcode.git'
            }
        }
        stage("cqa"){
            steps{
               withSonarQubeEnv("mysonar") {
                  sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=cqa"
               } 
            }
        }
        stage("image"){
                steps{
                    sh "docker build -t bharath0803/php:db-v1 database"
                    sh "docker build -t bharath0803/php:app-v1 ."
            }
        }  
        stage("image push"){
            steps{
               script{
                   withDockerRegistry(credentialsId: '181bce8c-1705-4076-b620-707ca93b8888') {
                       sh "docker push bharath0803/php:db-v1"
                       sh "docker push bharath0803/php:app-v1"
                   }
               } 
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose up -d"
            }
        }
    }
}
