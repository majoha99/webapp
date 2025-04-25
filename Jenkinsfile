pipeline {
  agent any
  tools { maven 'maven'}
  stages {
    stage ('Initialize') {
      steps {
        sh '''
            echo "PATH=$(PATH)"
            echo "M2_HOME=$(M2_HOME)"
          '''
      }
    }

    
    stage ('Build'){
      steps {
      sh 'mvn clean package' 
      }
    }

    
   stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@34.224.167.93:/Prod/apache-tomcat-9.0.102/webapps/webapp.war'
              }      
           }       
    }

    
  }
}
