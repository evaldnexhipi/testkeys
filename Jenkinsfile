def remote = [:]
remote.name = "ubuntu"
remote.host = "18.191.55.200"
remote.allowAnyHosts = true
node {
   withCredentials([sshUserPrivateKey(credentialsId: 'fb85b400-eef6-45e8-9aae-0f27e2cc7fbe', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
      remote.user = userName
      remote.identityFile = identity
      stage("install kubectl"){
           sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
           sh 'chmod +x ./kubectl'
           sh 'sudo mv ./kubectl /usr/local/bin/kubectl'
      }
      stage("install kops") {
           sh 'curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64'
           sh 'chmod +x kops-linux-amd64'
           sh 'sudo mv kops-linux-amd64 /usr/local/bin/kops'
      }
   }
}
