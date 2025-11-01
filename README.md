# eks-helm-aws


//Install AWS CLI

sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version


///Install eksctl//

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz"
tar -xzf eksctl_$(uname -s)_amd64.tar.gz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version


////kubctl//

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client


////

Associating IAM OIDC Provider with my cluster

eksctl utils associate-iam-oidc-provider --cluster eks-ebs-csi-cluster  --approve --region ap-south-1

///

Creating IAM role with the necessary permissions for the EBS CSI Driver and sets up a trust relationship between this IAM role and the Kubernetes service account.//

eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster demo-cluster \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --role-only \
  --role-name AmazonEKS_EBS_CSI_Driver_Role \
  --region ap-south-1


  ///Install the AWS EBS CSI Driver Addon//

eksctl create addon --name aws-ebs-csi-driver --cluster eks-ebs-csi-cluster --service-account-role-arn arn:aws:iam::522814693734:role/AmazonEKS_EBS_CSI_Driver_Role --region us-east-2 --force


//




















