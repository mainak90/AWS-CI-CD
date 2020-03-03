# Setup build env
## Install and configure Jenkins and required packages

Connect to your instance using your private key:

```
$ ssh -i "path-to-key/key.pem" ec2-user@<instance-name>.<instance-region>.compute.amazonaws.com
$ sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat/jenkins.repo
$ sudo rpm — import https://pkg.jenkins.io/redhat/jenkins.io.key
$ sudo yum install jenkins -y
$ sudo usermod -aG root jenkins 
$ sudo usermod -aG docker $USER
$ sudo usermod -aG docker jenkins
$ sudo chkconfig jenkins on
$ sudo chkconfig docker on
$ sudo chkconfig --list
$ sudo service jenkins start
$ sudo service docker start
$ sudo iptables -I INPUT 1 -p tcp --dport 8080 -j ACCEPT
$ sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT
$ sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
$ sudo iptables -I INPUT 1 -p tcp --dport 8443 -j ACCEPT
$ sudo iptables -I INPUT 1 -p tcp --dport 443 -j ACCEPT
$ sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8443
$ cd provision
$ echo 'variable "WEB_INSTANCE_AMI" { default = "'${AMI_ID_WEB}'" }' > amivar_web.tf
$ aws s3 cp amivar_web.tf s3://node-aws-jenkins-terraform/amivar_web.tf
$ terrafrom init
$ terraform apply -auto-approve -var RDS_PASSWORD=$RDS_PASSWORD
```

