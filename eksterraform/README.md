
# Installation

Create an EC2 instance, SG opens 8080 (anywhere) and 22 (my IP). 

ssh to ec2-user@IP



### Install Java8 on centos7
```
yum -y update
yum install java-1.8*
java -version
update-alternatives --config java
source ~/.bash_profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.amzn2.0.1.x86_64/jre/bin/
```

### Install git
```
yum install -y git
```

### Install Jenkins on Centos7:
```
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat-stable/jenkins.repo

yum install jenkins java-1.8* –y
systemctl start jenkins
systemctl enable jenkins
Access the URL : http://<Ip-Address-of-your-Server>:8080
```

### Install AWScli on Centos7 :
(No need for EC2 instance.  In vagrant vm, following steps are needed.)
```
sudo visudo
jenkins ALL=(ALL)	NOPASSWD: ALL
sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
yum install -y unzip
unzip awscliv2.zip
sudo ./aws/install
aws --version
which aws
sudo su -
sudo -u jenkins bash
aws configure
(fill in your aws accesskey)
```
### Install python3 on Centos7:
```
sudo yum install centos-release-scl
sudo yum install rh-python36
scl enable rh-python36 bash
python --version
Python 3.6.9
```

### Install terraform on Centos7
download terraform from hishicorp
wget https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip
mv ~/Downloads/terraform /usr/bin/terraform
#### enable tab auto-completion
terraform -install-autocomplete



### Allow SUDO permissions for Jenkins User:
```
sudo visudo
jenkins ALL=(ALL) NOPASSWD: ALL
usermod -a -G sudo jenkins
/etc/ssh/sshd_config : PasswordAuthentication yes
systemctl restart ssh
```

### Install kubectl on Centos7:
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/bin/kubectl
```

### Install aws-iam-authenticator on Centos7:
```
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/aws-iam-authenticator
chmod +x ./aws-iam-authenticator
mv aws-iam-authenticator /usr/bin/
```

## Login to as user Jenkins:
```
sudo su -
sudo -u jenkins bash
aws configure
(fill in your accesskey and secretkey)
```

## If terraform command cannot find:
ln -s /usr/local/bin/terraform /usr/bin/terraform

## NOTE: 
This full configuration utilizes the Terraform http provider to call out to icanhazip.com to determine your local workstation external IP for easily configuring EC2 Security Group access to the Kubernetes master servers. Feel free to replace this as necessary.

# Login to Jenkins, install two plugins:
```
Pipeline: AWS steps
Terraform
```

## Create jenkins job:
New item ->  Name -> Pipeline -> OK. 
Configure -> Pipeline -> (copy in Jenkinsfile)
 