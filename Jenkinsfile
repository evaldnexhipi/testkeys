def remote = [:]
remote.name = "ubuntu"
remote.host = "18.191.55.200"
remote.allowAnyHosts = true
node {
   withCredentials([sshUserPrivateKey(credentialsId: 'fb85b400-eef6-45e8-9aae-0f27e2cc7fbe', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
      remote.user = userName
      remote.identityFile = identity
      stage("install awscli") {
            sh 'sudo apt-get update'
            sh 'sudo apt-get install awscli -y'
      }
      stage("install kops"){
            sh 'curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d \'\"\' -f 4)/kops-linux-amd64 ' 
            sh 'chmod +x kops-linux-amd64'
            sh 'sudo mv kops-linux-amd64 /usr/local/bin/kops'
      }
      stage("install kubectl"){
            sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
            sh 'chmod +x ./kubectl'
            sh 'sudo mv ./kubectl /usr/local/bin/kubectl'
      }
       withCredentials(
        [[
            $class: 'AmazonWebServicesCredentialsBinding',
            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
            credentialsId: '0ae70636-5d3d-4818-b781-4aa102e8d03f',  // ID of credentials in kubernetes
            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) {
            stage("create s3 bucket"){
                sh 'aws s3 mb s3://xlajd.k8s.io'
            }
            stage("Expose environment variable"){
                sh 'export KOPS_STATE_STORE=s3://xlajd.k8s.io'
            }
            stage("generate ssh-keygen"){
                sh 'sudo ssh-keygen -t rsa -N "" -f /root/.ssh/id_rsa -y'
            }
            stage("create the cluster"){
                sh 'kops create secret --name xlajd.k8s.io --state s3://xlajd.k8s.iosshpublickey admin -i /root/.ssh/id_rsa'
                // sh 'kops create cluster xlajd.k8s.io --cloud=aws --node-count 2 --zones us-east-2b --node-size t2.micro --master-size t2.micro --image ami-05c1fa8df71875112 --state=s3://xlajd.k8s.io --dns-zone=islajd.io --dns private'
                sh 'kops create cluster --name=xlajd.k8s.io --cloud=aws --zones=us-east-2b --name=xlajd.k8s.io --node-size=t2.micro --master-size=t2.micro --image ami-05c1fa8df71875112 --state s3://xlajd.k8s.io --dns-zone=islajd.io --dns private'
                // sh 'kops create cluster --cloud=aws --zones=us-east-2b --name=xlajd.k8s.io --dns-zone=islajd.io --dns private'
                
            }
        }
   }
}
