           Devops Auomtation 





Build 11 server from the Vcenter

Team : Cyberknight 
Ip Range: 192.168.152.0/23 - 
        192.168.152.10 - 192.168.152.240
Gateway: 192.168.152.1
Domain: ziyotek2.local

Servers:        Servername                  Ip Address
Database  -     prdx-db201                  192.168.152.20  
Webserver1      prdx-webserver201           192.168.152.30 
Webserver2      prdx-webserver202           192.168.152.32 
Webserver3      prdx-webserver203           192.168.152.34
Load Balancer   prdx-haproxy201             192.168.152.40
Nagios          prdx-nagios201              192.168.152.52
Ansible         prdx-ansible201             192.168.152.70
Docker primary  prdx-dprimary201            192.168.152.60
Docker Worker1 prdx-dworker201              192.168.152.62
Docker Worker2 prdx-dworker202              192.168.152.64
Kubernetes      prdx-kube201                192.168.152.90



Instructions:


Team Leads must assign your tasks in Jira -> http://192.168.20.7:8080/secure/Dashboard.jspa

****************************************************************
                    Ansible
****************************************************************

All configurations need to occur in Ansible
Ansible with all servers and service Playbook - We should deploy or build servers with Ansible
Ansible codes should be in github.com 
ansible user should have sudo privileges and be able to log in without password into any box 
Create an Ansible playbook to update kernel and OS  -- This is called patching

Check --- $ansible all -m ping 
Result- there should be no errors

                                                                    
****************************************************************   
             Common (Ziyotek On-prem Servers):
             Security:
****************************************************************          

ssh  "root login" should be disabled on all boxes.
Firewall should be ENABLED on all boxes
SELinux should be disabled on all boxes
password should be "password" for all users

Check: Login with root to any box - This shld be disabled 

***********************************************************************************************
Common packages need to be installed on all boxes using "ansible" (code should be in git repo):
***********************************************************************************************

bind-utils
man
man-pages
mlocate
sysstat
On all Virtual machines

                                             	                                           	                                             	               
****************************************************************
            DNS
 ****************************************************************

DNS   - All servers must resolve from DNS server at http://192.168.37.44/poweradmin
Server Name 	Server	Clients	Check
nameserver                                                	                                            	                                             	
#nslookup client-name nameserver

$ ansible all -m shell -a "cat /etc/resolv.conf"
$ ansible all -m shell -a "cat /etc/hosts"                                                                            

 
*****************************************************************
        Web (apache) and Load Balancer (lb)
********************************************************************

3 webs behind the load balancer
lb must be working - Three web servers must be at lb Load balancer and share the load
All the of the webs should have the same content, but change the #'s to show that it's sharing the load
extra - try to find fancy open source website for the content

Server Name  Apache & Haproxy
Check: Go to browser and type IP of LB. Refresh several to see that LB is actually working 


****************************************************************
         Database
****************************************************************

DB server MariaDB
Create a sample database for your team (including names, states, etc)

Check: From CLI show your database


****************************************************************                                    
        Monitoring
****************************************************************
Nagios Server with GUI - All servers must be monitored from Nagios
Apache Service should be monitored
DNS you need to monitor services that are related to DNS

Check : 
1. Everything should be green
2. VMs should have proper checks (e.g. ftp should check ftp services, not httpd)
3. Shutdown one service then check if monitoring system will catch  

 
****************************************************************  
            Docker
****************************************************************              
Build and configure docker swarm cluster
Create docker image based on the following and deploy on cluster in 3 replicas:

- packages: httpd php mysql php-mysql 
- make sure apache is running
- Untar and copy the following to /var/www/html
   https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/CUR-TF-200-ACACAD/studentdownload/lab-app.tgz
- command: chown apache:root /var/www/html/rds.conf.php

Check: Show running webiste on broswer




****************************************************************  
           Kubernetes
****************************************************************  

Overview
Provision Kubernetes cluster, in order to securely deploy and expose applications to the world.

Goals
Our organization is expanding globally and currently we are looking to adopt a production-grade container scheduling and management system such as Kubernetes.

    1. The Kubernetes solution should secure access to running containers.
    2. Cluster health, application deployments should be viewable via a browser UI for people that do not know kubernetes or has less technical experience (executives, stakeholders )
    3.  DNS should be configured for cluster endpoints. (applications)
    4. All code bases should be stored gitHub!

Specifications

A. Provision kubernetes cluster of your choice, options are

Minikube cluster https://minikube.sigs.k8s.io/docs/start/
Kind cluster https://kind.sigs.k8s.io/docs/user/quick-start/
EKS cluster https://eksctl.io/usage/creating-and-managing-clusters/

B. Provision a Monitoring/Observability solution in order to view cluster health. Use one of the monitoring solutions below:

1. Kubernetes Dashboard https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
2. Grafana + prometheus https://github.com/Ziyotek-Institute/020DO-SU22-minikube/tree/main/prometheus-grafana/helm-tutorial


C. Deploy Ingress-controller in order to expose applications via Ingress rules (HTTP rules) Use one of the ingress-controllers below:
1. https://ziyotek.neolms.com/student_lesson/show/3505627?lesson_id=16217157§ion_id=66530061
2. https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.5.1/deploy/static/provider/aws/deploy.yaml
1. Minikube addons enable ingress
2. If the kubernetes cluster is provisioned in the cloud then use Most popular Ingress-controller-nginx!  https://kubernetes.github.io/ingress-nginx/deploy/#minikube


D. ( Optional )
Create DNS records for your apps 

Milestones by levels ( nailed it!; expert!; unstoppable!; GOD LIKE!; You are hired! )

  I.Cluster is ready to support Deployments (websites), and expose Services ( nailed it!)
    1. Create at least 1 Deployment with at least 1 replica Running!
    2. Create at least 1 Service which is exposed via NodePort or LoadBalancer

  II. Cluster has a monitoring solution viewable via browser using any dashboard of your choice ( expert!)
     Provide a URL that will present cluster health live

  III. Ingress-controller is managing Ingress objects which route traffic to your deployments ( unstoppable! )
       1. Create one Ingress for each Service deployed

  IV.   Internal-DNS is creating DNS records ( GOD LIKE! )
        Navigate to example.com to hit the application
 

Check: 1. Show health of the cluster
       2. Show examples
        
EXTRA: Dashboard                                                                          
 
****************************************************************  
                  CICD Pipeline
****************************************************************  

Build and configure Jenkins at CentOS 7 as EC2 or Container, it's a free choice
A. Deploy the application located at https://github.com/ziyotek/devops-project.git using CICD

1. Fork and clone the repository.

2. The application needs to be running on an AWS EC2 instance. Write the terraform infrastructure with all necessary resources to host the app.


Create 2 separate CICD configurations to deploy the application:


B. Using AWS native tools ( CodeBuild, CodeDeploy ). Appspec.yml is provided in the repo; The pipeline needs to build all the AWS resources and automatically deploy the application.

Example deployment: https://github.com/ziyotek/devops-terraform.git


C. Using Jenkins. (Optionally using skeleton Jenkinsfile which is provided). For this option bootstrap your application to the EC2 instance. The bootstrap script is provided in the repo (jenkins-bootstrap-script.sh);

Example deployment: https://github.com/ziyotek/devops-jenkins.git

 
Check: 
1. Show your Jenkins Dashboard
2. Show one pipeline

                                                                         

****************************************************************   
AWS Portion of the project: 
****************************************************************  
 

Task:

Autoscaling project should be deployed fully using terraform
Terraform code should be deployed using CICD Pipeline (Jenkins or AWS)
Example deployment: https://github.com/rpaskalev/2022-devops-terraform/tree/tf/class6
Instructions :
create a new vpc
create 2 public subnets in this vpc
create an Internet Gateway
Attach the internet gateway to the VPC
Create 1 Public Route table
in the Public Route Table create an additional route to the Internet Gateway ( 0.0.0.0/0 -> internet_gateway)
Create 2 Target groups for load balancers ( 1x Images | 1x videos )
Create 1x Application load balancer
In your application load balancer create proper /path rules :
- /itmeme/* -> images target group
- /video/* -> videos target group 
Create 2x Launch configurations( since it is depreciated, use Launch template ) ( 1x for images instances | 1x for videos instances) 
Create 2x Autoscaling groups ( 1x for images instances | 1x for videos instances)
Change the settings of your autoscaling groups to automatically add the new instances to proper Target groups :
- videos -> videos target group
- images -> images target group
 

Testing the Project:

On EC2 instances, run the following:
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
stress --cpu 2 --timeout 30000
yum install -y htop
