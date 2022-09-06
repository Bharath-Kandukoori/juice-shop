pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
         }
     }
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/Bharath-Kandukoori/juice-shop.git > trufflehog'
        sh 'cat trufflehog'
      }
    }

    stage ('Source Composition Analysis') {
      steps {
        sh 'rm owasp || true'
        sh 'wget https://raw.githubusercontent.com/Bharath-Kandukoori/juice-shop/master/Jenkinsfile'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
        }
    }
    
    
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh ' cat target/sonar/report-task.txt'
        }
      }
    }
    
     options {
        timeout(time: 3, unit: 'SECONDS')
    }

    stages{
        stage('run') {
            steps {
                sh 'sleep 5'
                // working:
                // sleep(time: 5, unit: 'SECONDS')
            }
        }
    }
      
     stage ('Build') {
       steps {
       sh 'mvn clean package'
    }
     }
    
   }
}
