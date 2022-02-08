pipeline {
   agent any 
   parameters {
      string(defaultValue: 'master', description: 'Test Branch', name: 'BLD_BRANCH');
      string(defaultValue: '5.0.0', description: 'Test Build', name: 'BLD_VERSION');      
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
      stage('BuildCode') {
         steps {
          sh '''
            echo 'Building BackEnd'
            cd /var/jenkins_home/code/k8s_master/db_server
            docker build -t db_server:${BLD_VERSION} .
          '''

            
         }
         
      }
   }
}
