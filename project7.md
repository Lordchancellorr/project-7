   -    ## Lordchancellor's Documentation of Project 7

 Summary: 
- In [Project 6](https://www.darey.io/docs/project-6-step-1/), I implemented a WordPress based solution that is ready to be filled with content and can be used as a full fledged website or blog. (please check [Project 6 Documentation](https://github.com/Lordchancellorr/project-6) to access the documentation). However, this project is about the deployment of Devops Tools like [Jenkins](https://www.jenkins.io/), [Kubernetes](https://kubernetes.io/), [Jrog Artifactory](https://jfrog.com/artifactory/), [Rancher](https://rancher.com/products/rancher/), [Grafana](https://grafana.com/), [Prometheus](https://prometheus.io/), [Kibana](https://www.elastic.co/kibana) responsible for the day to day activities in managing, developing, testing, deploying and monitoring different projects. Read below to see how it was done!

## implementation of Instructions in Project 7
- Step 1
New Instances were launched in this order: 4 instances were spinned up usinf Redhat(For 3 Webservers and the NFS) while the last instance spinned up was Ubuntu 20.04 for the Database server. See the Image below:![Instances](https://github.com/Lordchancellorr/project-7/blob/main/Images/Instances.PNG). Just as i did in project6, the Logical volumes were configured. See the Images below for significant output: ~[Connection to webserver](https://github.com/Lordchancellorr/project-7/blob/main/Images/connection%20to%20nfs%20server%20from%20webserver1.PNG)

![Attached Volumes](https://github.com/Lordchancellorr/project-7/blob/main/Images/Attached%20Volumes.PNG) ![First partition](https://github.com/Lordchancellorr/project-7/blob/main/Images/first%20partition.PNG)  ![Logical Volumes](https://github.com/Lordchancellorr/project-7/blob/main/Images/Logical%20volumes.PNG)  Mount Points were created and named: `lv-apps`(mounted on `/mnt/apps` - to be used by the webservers), `lv-logs`(mounted on `/mnt/logs` to be used by webservers logs) and `lv-opt` (mounted on `/mnt/opt` for Jenkins in Project 8) respectively. 
- NFS was set up and enabled with these codes: `sudo yum -y update`, `sudo yum install nfs-utils -y`, `sudo systemctl start nfs-server.service`, `sudo systemctl enable nfs-server.service`and `sudo systemctl status nfs-server.service` 


-In other for the webservers  to read,write and execute files on the NFS, i run these commands: `sudo yum -y update`
`sudo yum install nfs-utils -y`
`sudo systemctl start nfs-server.service`
`sudo systemctl enable nfs-server.service`
`sudo systemctl status nfs-server.service` See the the status below.

 ![NFS Server Status](https://github.com/Lordchancellorr/project-7/blob/main/Images/NFS%20Status.PNG)
 
I configured access to the NFS for clients within the same subnet(pasted the codes in the documentation saved and exited). After editing and adding new rules that fits in to the proper working status of NFS to the security groups,(See the image belwo to the see the changes effected to the default security group) ![Security group](https://github.com/Lordchancellorr/project-7/blob/main/Images/Security%20groups.PNG) i ran this command - `rpcinfo -p | grep nfs` and the image below was the output: 

![NFS Port](https://github.com/Lordchancellorr/project-7/blob/main/Images/NFS%20Port.PNG)

- STEP 2-3

CONFIGURING THE DATABASE SERVER
I installed mysql and created a database. See the image below. ![mysql configuration](https://github.com/Lordchancellorr/project-7/blob/main/Images/Mysql%20configuration.PNG)

On the webservers created, I ran `sudo yum install nfs-utils nfs4-acl-tools -y` crreated a directory, mounted `/var/www` with `sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www`(I edited and inserted the NFS Private IP). After installing apache and php in Remi's repository for the three servers, the images below were the output for php succesful installation and activeness- ![php-fpm for webserver1](https://github.com/Lordchancellorr/project-7/blob/main/Images/php-fpm%20status%20on%20webserver%201.PNG) ![php-fpm for webserver2](https://github.com/Lordchancellorr/project-7/blob/main/Images/php-fpm%20status%20on%20webserver%202.PNG) ![php-fpm for webserver3](https://github.com/Lordchancellorr/project-7/blob/main/Images/php-fpm%20status%20on%20webserver%203.PNG) See the msql database set uo below: ![Mysql database](https://github.com/Lordchancellorr/project-7/blob/main/Images/mysql%20setup%20on%20database.PNG)

- I verified if  Apache files and directories were available on the Web Server in `/var/www` and also on the NFS server in `/mnt/apps`then I located the log folder for Apache on the Web Server and mounted it to NFS server’s export for logs. i forked the tooling source code gottten from [Darey.io Tooling](https://github.com/darey-io/tooling.git)... learn how i did it [here](https://youtu.be/f5grYMXbAV0) afer which git clloned the files...check the image below- 
![Cloned Tooling](https://github.com/Lordchancellorr/project-7/blob/main/Images/tooling%20cloned%20from%20darey.PNG) 
![Tooling](https://github.com/Lordchancellorr/project-7/blob/main/Images/tooling.PNG)

I also deployed the  html folder from the repository  to /var/www/html and accessed the page using the webservers IP addresses and the images below were the outcome.

![Webserver1](https://github.com/Lordchancellorr/project-7/blob/main/Images/Webserver1.PNG)

![Webserver2](https://github.com/Lordchancellorr/project-7/blob/main/Images/Webserver2.PNG)

Thank You!