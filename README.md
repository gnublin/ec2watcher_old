ec2watcher
==========

## Description
This daemon is create for AWS Ec2 instances, Elastic IPs fail-over.

I've found a script which can do this functionality provide by Amazon. Only I've thought it was incomplet. Indeed, you should duplicate this script if you want to monitor more Elastic IPs. 

I've decided to create a daemon which could monitor Elastic IP, managed by an init script.

**_It's not recommended to set this daemon on over two ec2 instances_**

## Requirement 
You should install : 
* Java runtime environment (ex: java-7-openjdk)
* AWS ec2 tools : [ec2-api-tools](http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip)
* AWS credentials (Access and Secret keys) with interface reassignment permissions
* Sub-interface should be configured on both instance (ex: eth0:0 10.X.X.X which is the nat of your public IP)

## Installation
### Java runtime environment
On Debian/Ubuntu : apt-get install openjdk-7-jre openjdk-7-jre-headless
On Redhat/Centos : yum install java-1.7.0-openjdk

### AWS Ec2 tools
* wget http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip
* unzip ec2-api-tools.zip
* mv ec2-api-tools-* /usr/lib/

### Ec2watcher
* git clone git@github.com:gnublin/ec2watcher.git
* cd ec2watcher
* cp -r etc/* /etc/
* cp -r usr/lib/ec2watcher /usr/lib/

## Configuration
### Global
Edit /etc/aws_profile.conf change these values : 
 * AWS_ACCESS_KEY
 * AWS_SECRET_KEY 
 * JAVA_HOME (if you haven't installed openjdk-7-jre)
 * EC2_HOME (if you have installed ec2-api-tool in other path than /usr/lib/ec2) 
 * EC2_MAIL (email address you want to receive the failover report
Edit /etc/default/ec2watcher and change START_DAEMON from no to yes

### Instance
Create one symbolic link by IP public you want to monitor : 
 * ln -s /etc/init.d/ec2watcher /etc/init.d/ec2watcher-11.22.33.44
Start this instance
 * ∕etc/init.d/ec2watcher-11.22.33.44 start

**You can create as many symbolic link that you have IPs to monitored**

**_you can view what is happening in the log file /var/log/ec2watcher.log (default path)_**
### Create your own package
