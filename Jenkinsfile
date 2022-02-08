pipeline {
   agent any 
   parameters {
      string(defaultValue: 'master', description: 'Test Branch', name: 'BLD_BRANCH');
      string(defaultValue: '5.0.0', description: 'Test Build', name: 'BLD_VERSION');  
      string(defaultValue:'All', decsiption: 'Component to Build', name:'component');
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
            expression { params.component == 'only-DB' ||  params.component == 'All'}
         
         }
         steps {
          sh '''
            echo 'Building BackEnd'
            cd /var/jenkins_home/code/db_server
            docker build -t db_server:${BLD_VERSION} .
          '''

            
         }
         
       stage('BuildAppCode') {
          when { 
             expression {params.component == 'Only-AppServer' || params.component == 'All' }
          }
         steps {
          sh '''
            echo 'Building BackEnd'
            cd /var/jenkins_home/code/app_server
            docker build -t app_server:${BLD_VERSION} .
          '''

            
         }
         
      }
         
         stage('BuildUICode') {
            when {
               expression { params.component =='UI' || params.component =='All }
            }
               
            steps {
               
               sh '''
                 cd /var/jenkins_home/code/web_server
                 docker build -t web_server:${BLD_VERSION} .
                '''
            
            }
                
         }
   }
}
