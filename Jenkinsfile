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

    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/majoha99/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }


     stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/majoha99/webapp/refs/heads/patch-1/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.144.98.253:/home/ubuntu/Prod/apache-tomcat-9.0.102/webapps/webapp.war'
              }      
           }       
    }

    
  }
}
