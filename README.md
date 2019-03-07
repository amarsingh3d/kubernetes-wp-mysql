# Deploy Wordpress and MySQL pods with Persistent Disks

Create EKS cluster if doesn't exist already
```
eksctl create cluster --name=eks-test --tags environment=test --region=us-east-1 --zones=us-east-1a,us-east-1b --vpc-cidr=10.0.111.0/22 --nodes=1 --nodes-min=1 --nodes-max=1 --node-volume-size=10 --node-type=t2.micro --nodegroup-name=test-web --asg-access --ssh-access --ssh-public-key=OPSDEV
```
