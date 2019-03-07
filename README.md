# Deploy Wordpress and MySQL pods with Persistent Disks

![image] (https://1.bp.blogspot.com/-EV7l9Z5e3ro/XDc54JgpHoI/AAAAAAAAFFc/Xg2ZtO99QGsguaJU6U1lwM53WZTG0c9DQCLcBGAs/s1600/EKS-LAMP.jpg)

**Step 1- Create EKS Cluster:**

**Create EKS cluster if doesn't exist already**
```
eksctl create cluster --name=eks-test --tags environment=test --region=us-east-1 --zones=us-east-1a,us-east-1b,us-east-1d --vpc-cidr=10.0.133.0/22 --nodes=1 --nodes-min=1 --nodes-max=2 --node-volume-size=20 --node-type=t3.medium --nodegroup-name=test-web --asg-access --ssh-access --ssh-public-key=OPSDEV
```
**Step 2- Git Checkout**

**Checkout Git repository**
```
git clone https://github.com/amarsingh3d/kubernetes-wp-mysql.git
```

**Change directory**
```
cd kubernetes-wp-mysql
```
**Step 3- Create MySQL Deoployment, Secret and Service**

**A- Create MySQL Persistent Volume**
```
kubectl create -f mysql-volumeclaim.yaml
```
**B- Check Persistent Volume**
```
 kubectl get pv

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                       STORAGECLASS   REASON   AGE
pvc-ee72ff75-40ac-11e9-9b22-12355d9983be   2Gi        RWO            Delete           Bound    default/mysql-volumeclaim   gp2                     33s
```
**C- Check Persistent Volume Claims**
```
kubectl get pvc

NAME                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-volumeclaim   Bound    pvc-ee72ff75-40ac-11e9-9b22-12355d9983be   2Gi        RWO            gp2            36s
```
**D- Create MySQL password in secret**
```
kubectl apply -f secret.yaml
```
**E- check created secret**
```
kubectl get secret
NAME                  TYPE                                  DATA   AGE
mysql                 Opaque                                1      46s
```
**F- Create MySQL Deployment**
```
kubectl create -f mysql-Deployment.yaml
```
**G- Verify Created Deployment**
```
kubectl get deploy
NAME    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
mysql   1         1         1            0           44s
```
**H- Create MySQL Service**
```
kubectl create -f mysql-service.yaml
```
**I- Verfiry MySQL Service**
```
kubectl get svc

NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
mysql        ClusterIP   172.20.53.185   <none>        3306/TCP   50s
```

**Step 4 -Create WordPress Deoployment and Service**


**A- Create Persistent Volume for Wordpress**
```
create -f wordpress-volumeclaim.yaml
```
**B- Verify wordpress persistent volume**
```
kubectl get pvc

NAME                    STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-volumeclaim       Bound    pvc-3778448a-40c6-11e9-969f-12ba04f5e3ee   2Gi        RWO            gp2            3m
wordpress-volumeclaim   Bound    pvc-8f0a8655-40c6-11e9-969f-12ba04f5e3ee   2Gi        RWO            gp2            47s
```
**C- Create Wordpress Deployment**
```
create -f wordpress-Deployment.yaml
```
**D- Verify Wordpress Dployment**
```
kubectl get deploy
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
mysql       1         1         1            1           3m
wordpress   2         2         2            2           28s
```
**E- Verify running pods***
```
kubectl get pods -o=wide

NAME                         READY   STATUS    RESTARTS   AGE   IP             NODE                           NOMINATED NODE
mysql-5bfd5f74dd-rcxwn       1/1     Running   0          4m    10.0.132.230   ip-10-0-132-253.ec2.internal   <none>
wordpress-78c9b8d684-8d69f   1/1     Running   1          43s   10.0.132.239   ip-10-0-132-253.ec2.internal   <none>
wordpress-78c9b8d684-lvtgq   1/1     Running   0          43s   10.0.132.186   ip-10-0-132-253.ec2.internal   <none>
```
**F- Create Service for Wordpress**
```
kubectl create -f wordpress-service.yaml
```
**G- Verify wordpress Service**
```
 kubectl get svc -o=wide

NAME         TYPE           CLUSTER-IP      EXTERNAL-IP                                                              PORT(S)        AGE   SELECTOR
kubernetes   ClusterIP      172.20.0.1      <none>                                                                   443/TCP        20m   <none>
mysql        ClusterIP      172.20.24.199   <none>                                                                   3306/TCP       5m    app=mysql
wordpress    LoadBalancer   172.20.43.88    a09a35f1a40c711e9818c0ab75c2116c-572098965.us-east-1.elb.amazonaws.com   80:31986/TCP   48s   app=wordpress
```

**Step 5-  Verify Wordpress and MySQL Setup **

Open browser and access wordpress service external address- http://a09a35f1a40c711e9818c0ab75c2116c-572098965.us-east-1.elb.amazonaws.com

Follow the instruction and finish the installation 



