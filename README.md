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
	• Download DSS, together with the “generic-hadoop3” standalone Hadoop libraries and standalone Spark binaries.
  ```
  sudo yum install -y wget
	wget https://downloads.dataiku.com/public/studio/8.0.1/dataiku-dss-8.0.1.tar.gz
	wget https://downloads.dataiku.com/public/studio/8.0.1/dataiku-dss-hadoop-standalone-libs-generic-hadoop3-8.0.1.tar.gz
	wget https://downloads.dataiku.com/public/studio/8.0.1/dataiku-dss-spark-standalone-8.0.1-2.4.5-generic-hadoop3.tar.gz
  ```
	• Install DSS, see Installing and setting up
	• Build base container-exec and Spark images, see Initial setup
	
	```
	mkdir install 
  mkdir data
  ```

