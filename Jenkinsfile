pipeline {
    agent { label 'jenkins-agent' } 
    stages {
       stage('build docker image') {
        steps {
                 sh 'docker build -t googlescraper .' 
            }
       }
      //  stage("remove unused image"){
      //       steps{
      //         sh 'docker image prune -a -f'  
      //       }
      //   }
    }   
}