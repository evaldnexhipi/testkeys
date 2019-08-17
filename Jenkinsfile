def remote = [:]
remote.name = "ubuntu"
remote.host = "3.15.205.56"
remote.allowAnyHosts = true
node {
   withCredentials([sshUserPrivateKey(credentialsId: 'fb85b400-eef6-45e8-9aae-0f27e2cc7fbe', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
       remote.user = userName
       remote.identityFile = identity
       stage("install AWSCLI") {
            sh 'sudo su'
            sh 'curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip'
            sh 'apt install unzip python -y'
            sh 'unzip awscli-bundle.zip'
            sh './awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws'
       }
        
   }
}
