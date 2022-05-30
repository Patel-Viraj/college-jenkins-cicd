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
      when { branch 'master'}
      steps {
        
        sh '''pwd
              rsync -zhvr . ubuntu@$MASTER_DEPLOY_IP:/home/ubuntu/nodejs/
              '''
      }
    }

    stage('Install Packages on Main Server') {
      when { branch 'master'}
      steps {
        sh 'ssh -t ubuntu@$MASTER_DEPLOY_IP \'cd /home/ubuntu/nodejs && npm i \''
      }
    }

    stage('Production Live Application') {
      when { branch 'master'}
      steps {
        sh 'ssh ubuntu@$MASTER_DEPLOY_IP \"cd /home/ubuntu/nodejs && pm2 restart index.js\"'
      }
    }

    
    
    
      stage('Sync file to Stage Server') {
        when { branch 'stage'}
        steps {
          sh '''pwd
                rsync -zhvr . ubuntu@$STAGE_DEPLOY_IP:/home/ubuntu/nodejs/
                '''
        }
      }

    stage('Install Packages on Stage Server') {
      when { branch 'stage'}
      steps {
        sh 'ssh ubuntu@$STAGE_DEPLOY_IP \"cd /home/ubuntu/nodejs && npm i \"'
      }
    }

    stage('Staging Live Application') {
      when { branch 'stage'}
      steps {
        sh 'ssh ubuntu@$STAGE_DEPLOY_IP \"cd /home/ubuntu/nodejs && pm2 restart index.js\"'
      }
    }

  }
}
