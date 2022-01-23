# aws-ssm-demo 🐳

![Stars](https://img.shields.io/github/stars/tquangdo/aws-ssm-demo?color=f05340)
![Issues](https://img.shields.io/github/issues/tquangdo/aws-ssm-demo?color=f05340)
![Forks](https://img.shields.io/github/forks/tquangdo/aws-ssm-demo?color=f05340)
[![Report an issue](https://img.shields.io/badge/Support-Issues-green)](https://github.com/tquangdo/aws-ssm-demo/issues/new)

## reference
1. [aws](https://aws.amazon.com/getting-started/hands-on/remotely-run-commands-ec2-instance-systems-manager/)
2. [vtitechblog](https://vtitech.vn/connect-den-private-server-thong-qua-session-manager/)
3. [awsstudygroup](https://000031.awsstudygroup.com/vi/)

## ref 1.
### 1-1/ EC2
- launch instance:
- 1. AMI=`linux`
- 2. role name=`AmazonEC2RoleforSSM`
### 1-2/ run command
- SSM click `run command` > `run a command`
- type=`AWS-RunShellScript`
- Command parameters=`sudo yum update -y`
- Targets=`Choose instances manually` > select EC2
![1](screenshots/1.png)
- click `Run`
- see output: choose EC2 > click `View output`
![2](screenshots/2.png)
### 1-3/ Session Manager
- SSM click `Session Manager` > `Start Session`
- Target instances: select EC2
![3](screenshots/3.png)
- click `start session`
![4](screenshots/4.png)

## ref 2.
### 2-1/ y nghia thay cho EC2 bastion
![bastion](screenshots/bastion.png)
### 2-2/ AWS CLI
#### connect to server (like ssh)
```shell
# TH KO attach IAM role AmazonEC2RoleforSSM
aws ssm start-session --target i-0d5c7930bbb66cc43 --profile default
An error occurred (TargetNotConnected) when calling the StartSession operation: i-0d5c7930bbb66cc43 is not connected.

# TH co attach IAM role
aws ssm start-session --target i-0d5c7930bbb66cc43 --profile default
Starting session with SessionId: dotqadm-00854d7434e4d2d3b # "dotqadm" chinh la IAM username
sh-4.2$ hostname
-> ip-172-31-30-29.ec2.internal
```
#### connect tu local browser KO can thong qua internet
```shell
aws ssm start-session --target i-0d5c7930bbb66cc43 --profile default --document-name AWS-StartPortForwardingSession \
--parameter "localPortNumber=10022,portNumber=10022"
->
Starting session with SessionId: dotqadm-0071a551b594168de
Port 10022 opened for sessionId dotqadm-0071a551b594168de.
Waiting for connections...
```
- Mở một terminate khác và tiến hành chạy port forwarding đến endpoint bạn mong muốn thông qua SSH (chưa làm!!!💣💣)
```shell
ssh -i bastionHostKey.pem ec2-user@localhost -p 10022 -L 8443:${WEB_APP_ENDPOINT}:443
```
- Sau khi chạy xong bạn có thể mở https://localhost:8443 để đến với server private từ máy local rồi!