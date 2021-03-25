# Dataiku Install on GCP

## Prepare the instance
• Create a VM
  - Setup a CentOS 7 
  - you select the right Service Account
  - you set the access scope to “read + write” for the Storage API
  - Create firewall rules
• Install and configure Docker CE
```
	$ sudo yum install -y yum-utils
	$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
	
	sudo yum install docker-ce docker-ce-cli containerd.io
```  
• Install kubectl
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
sudo yum install -y kubectl
```
• Setup a non-root user for the dssuser
```
sudo adduser dssuser ## Password = dataiku123#
sudo usermod -aG google-sudoers dssuser
```
## Install DSS
```
mkdir install 
 mkdir data
 ```
• Download DSS, together with the “generic-hadoop3” standalone Hadoop libraries and standalone Spark binaries.
```
sudo yum install -y wget
wget https://downloads.dataiku.com/public/studio/8.0.1/dataiku-dss-8.0.1.tar.gz
wget https://downloads.dataiku.com/public/studio/8.0.1/dataiku-dss-hadoop-standalone-libs-generic-hadoop3-8.0.1.tar.gz
wget https://downloads.dataiku.com/public/studio/8.0.1/dataiku-dss-spark-standalone-8.0.1-2.4.5-generic-hadoop3.tar.gz
```
• Install DSS, see Installing and setting up
• Build base container-exec and Spark images, see Initial setup
# Start from the home directory of user account "dssuser"
# which will be used to run the Data Science Studio
# We will install DSS using data directory: /home/dssuser/data
$ pwd
/home/dssuser
$ cd install
$ ls -l
-rw-rw-r--.  1 dssuser dssuser 222634590 Jul 30  2020 dataiku-dss-8.0.1.tar.gz
-rw-rw-r--.  1 dssuser dssuser 222634590 Jul 30  2020 dataiku-dss-hadoop-standalone-libs-generic-hadoop3-8.0.1.tar.gz
-rw-rw-r--.  1 dssuser dssuser 410899130 Jul 30  2020 dataiku-dss-spark-standalone-8.0.1-2.4.5-generic-hadoop3.tar.gz
# Unpack distribution kit
$ tar xzf dataiku-dss-8.0.1.tar.gz
# Run installer, with data directory $HOME/data and base port 10000
# This fails because of missing system dependencies
$ dataiku-dss-8.0.1/installer.sh -d /home/dssuser/data -p 10000

# Install dependencies with elevated privileges, using the command shown by the previous step
$ sudo -i "/home/dssuser/install/dataiku-dss-8.0.1/scripts/install/install-deps.sh"

# Rerun installer script, which will succeed this time
$ dataiku-dss-8.0.1/installer.sh -d /home/dssuser/data -p 10000

***************************************************************
* Installation complete (DSS node type: design)
* Next, start DSS using:
*         '/home/dssuser/data/bin/dss start'
* Dataiku DSS will be accessible on http://<SERVER ADDRESS>:10000
*
* You can configure Dataiku DSS to start automatically at server boot with:
*    sudo -i "/home/dssuser/install/dataiku-dss-8.0.1/scripts/install/install-boot.sh" "/home/dssuser/data" dssuser
***************************************************************
	
# Configure boot-time startup, using the command shown by the previous step
$ sudo -i "/home/dssuser/install/dataiku-dss-8.0.1/scripts/install/install-boot.sh" "/home/dssuser/data" dssuser

# Manually start DSS, using the command shown by the installer step
$ /home/dssuser/data/bin/dss start

# Connect to Data Science Studio by opening the following URL in a web browser:
#    http://HOSTNAME:10000
# Initial credentials : username = "admin" / password = "admin"
