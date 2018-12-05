# ODK on AWS cloud

This is the process of ODK-Aggregate installation on AWS Cloud. For that we are using Ubuntu Server 16.04 LTS (HVM) with t2.small instance.

First we need to fill-up some requriment for install ODK-Aggregate on Ubuntu Server 16.04 LTS (HVM) with t2.small instance.

- AWS account 
- AWS elastic ip/Public IP/domain
- AWS Security group
- Java 8 install
- Install Apache and phpMyAdmin
- Install PHP and PHP modules
- Install MySQL
- Tomcat 8
- ODK-Aggregate server
- mysql-connector-java-8.0.11.jar or upgarde version .jar file


### AWS Cloud Instance

 Create a AWS account - https://aws.amazon.com/ 
- Setting up an Ubuntu EC2 instance, we’ll choose Ubuntu Server 16.04 LTS (HVM) with t2.small instance. 
- Storage will be depand on your demand. By default you AWS volumes is 8GB. i will suggest take 20-25gb.
- Enable HTTP, HTTPS, Tomcat ,SSH port in Security group
- if you use instance public IP then be sure instance will be always running. because if you stop instace then ip will be change and you will be unable to access your Aggregate server. So after deploy instance make sure to you assign elastic IP or Domain.

NOTE: You can use Free tier eligible t2.micro instance but it will gives you low performance. it's better to use t2.small or upper instance type rather then t2.micro. you can try with t2.micro instance, if you satisfy with it then ok! if not then you can create custom AWS AMI from that instance and migrate to t2.small instance or other upgrade instance type.

### AWS Elastic IP / Public IP

- if you use instance public IP then be sure instance will be always running. because if you stop instance then IP will be change and you will be unable to access your Aggregate server. So after deploy instance make sure to you assign elastic IP, public IP or Domain.


### ODK-Aggregate

- Now install ODK Aggregate your local computer (not on your AWS instance). For me i am using ODK-Aggregate-v1.7.0-Windows version. you can download the latest version and install in your local computer. You can find aggregate releases on `https://github.com/opendatakit/aggregate/releases` 
- During installation process , it’s important to specify that this will be a MySQL installation, and it is also very important that you specify the correct domain name or IP address that will be used to access your Aggregate server. Ideally, this will be a specific domain name that you have already mapped to an elastic IP (and can re-map later if you change the IP).

Thank you
