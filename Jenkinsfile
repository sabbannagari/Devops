pipeline {
   environment {
         AWS_ACCOUNT_ID="312897329659"
         AWS_DEFAULT_REGION="us-east-1"
         REPOSITORY_URI = "https://${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
   }
   agent any 
   parameters {
      string(defaultValue: 'master', description: 'Test Branch', name: 'BLD_BRANCH');
      string(defaultValue: '5.0.0', description: 'Test Build', name: 'BLD_VERSION');  
      string(defaultValue: 'All', description: 'Component to Build', name:'component');
   }
   stages {
    stage('CheckOutCode') {
        steps {
           dir('/var/jenkins_home/code') {
            git branch: 'master',
                credentialsId: 'pqr',
                url: 'https://github.com/sabbannagari/k8s.git'
           }
        }
     }
      
      stage('BuildDBCode') {
          when { 
             expression { params.component == 'Only-AppServer' || params.component == 'All' }
          }
         steps {

          sh '''
         
            echo 'Building BackEnd'
            cd /var/jenkins_home/code/db_server
            docker build -t ${REPOSITORY_URI}/db_server:${BLD_VERSION} .     
            '''
           docker.withRegistry(${REPOSITORY_URI}, 'ecr:us-east-1:mykey') {
           docker.image(${REPOSITORY_URI}/db_server:${BLD_VERSION}).push(${BLD_VERSION})
           }
            
         }
      }  
       stage('BuildAppCode') {
          when { 
             expression { params.component == 'Only-AppServer' || params.component == 'All' }
          }
         steps {
          sh '''
            echo 'Building BackEnd'
            cd /var/jenkins_home/code/app_server
            docker build -t ${REPOSITORY_URI}/app_server:${BLD_VERSION} .
          '''

            
         }
         
      }
         
         stage('BuildUICode') {
          when { 
             expression { params.component == 'Only-AppServer' || params.component == 'All' }
          }
               
            steps {
               
               sh '''
                 cd /var/jenkins_home/code/web_server
                 docker build -t ${REPOSITORY_URI}/web_server:${BLD_VERSION} .
                '''
            
            }
                
         }
   }
}
