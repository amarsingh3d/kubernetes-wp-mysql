# Deploy Wordpress and MySQL pods with Persistent Disks

**Create EKS cluster if doesn't exist already**
```
eksctl create cluster --name=eks-test --tags environment=test --region=us-east-1 --zones=us-east-1a,us-east-1b --vpc-cidr=10.0.111.0/22 --nodes=1 --nodes-min=1 --nodes-max=1 --node-volume-size=10 --node-type=t2.micro --nodegroup-name=test-web --asg-access --ssh-access --ssh-public-key=OPSDEV
```
**Checkout Git repository**
```
git clone https://github.com/amarsingh3d/kubernetes-wp-mysql.git
```

**Change directory**
```
cd kubernetes-wp-mysql
```

**Create MySQL Persistent Volume**
```
kubectl create -f mysql-volumeclaim.yaml
```
**Check Persistent Volume**
```
 kubectl get pv

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                       STORAGECLASS   REASON   AGE
pvc-ee72ff75-40ac-11e9-9b22-12355d9983be   2Gi        RWO            Delete           Bound    default/mysql-volumeclaim   gp2                     33s
```
**Check Persistent Volume Claims**
```
kubectl get pvc

NAME                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-volumeclaim   Bound    pvc-ee72ff75-40ac-11e9-9b22-12355d9983be   2Gi        RWO            gp2            36s
```
**Create MySQL Deployment**
```
kubectl create -f mysql-Deployment.yaml
```
**Verify Created Deployment**
```
kubectl get deploy
NAME    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
mysql   1         1         1            0           44s
```

