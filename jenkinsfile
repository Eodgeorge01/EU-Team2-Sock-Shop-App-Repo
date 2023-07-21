pipeline {
  agent any
  stages {
    stage ('deploying to stage environment'){
      steps {
        sshagent(['ansible']) {
           sh 'ssh -t -t ubuntu@10.0.4.12 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/stage.yml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '26th-june-sock-shop-microservices-kubernetes-project---eu-team-1', message: 'Ready for new deployment to production environment', tokenCredentialId: 'slack'
      }
    }
    stage ('Approval'){
      steps {
         timeout(activity: true, time: 5) {
           input message: 'Please review and approve', submitter: 'admin'
         }
      }
    }
    stage ('deploying to prod environment'){
      steps {
        sshagent(['ansible']) {
           sh 'ssh -t -t ubuntu@10.0.4.12 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/prod.yml"'
        }
      }
    }
  }
}
