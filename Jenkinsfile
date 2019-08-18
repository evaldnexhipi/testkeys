def remote = [:]
remote.name = "ubuntu"
remote.host = "18.191.55.200"
remote.allowAnyHosts = true
node {
   withCredentials([sshUserPrivateKey(credentialsId: 'fb85b400-eef6-45e8-9aae-0f27e2cc7fbe', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
      remote.user = userName
      remote.identityFile = identity
      stage("install awscli") {
           sh 'sudo apt get update'
      }
      stage("install kops"){
      }
   }
}
