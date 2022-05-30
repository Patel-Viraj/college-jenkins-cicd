pipeline {
  
  agent any
  
  stages {
    
        stage('version check') {
          steps {
            sh '''git --version && jenkins --version
                  '''
          }
        }
    

    stage('Sync file to Main Server') {
      when { branch 'main'}
      steps {
        
        sh '''pwd
              rsync -zhvr . ubuntu@3.84.86.136:/home/ubuntu/main/
              '''
      }
    }

    stage('Install Packages on Main Server') {
      when { branch 'main'}
      steps {
        sh 'ssh ubuntu@3.84.86.136 \'cd /home/ubuntu/main && npm i\''
      }
    }

    stage('Production Live Application') {
      when { branch 'main'}
      steps {
        sh 'ssh ubuntu@3.84.86.136 \"cd /home/ubuntu/main/ && pm2 restart index.js\"'
      }
    }

    
    
    
      stage('Sync file to Stage Server') {
        when { branch 'stage'}
        steps {
        sh '''pwd
              rsync -zhvr . ubuntu@44.202.54.38:/home/ubuntu/stage/
              '''
      }
    }

    stage('Install Packages on Stage Server') {
      when { branch 'stage'}
      steps {
        sh 'ssh ubuntu@44.202.54.38 \"cd /home/ubuntu/stage && npm i\"'
      }
    }

    stage('Staging Live Application') {
      when { branch 'stage'}
      steps {
        sh 'ssh ubuntu@44.202.54.38 \"cd /home/ubuntu/stage/ && pm2 restart index.js\"'
      }
    }

    
    
    
    
  
  }
}
