# Installation and Configuration of Jenkins
Here we cover the installation and Configuration of Jenkins on server(s) **with/without** access to INTERNET

## Installation 

### On server(s) **with** access to INTERNET
Add Jenkins Repository Key to the system
```
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
```
Append the Debian package repository address to the server's sources.list

```
echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
```
Run update so that apt-get will use the new repository
```
sudo apt-get update
```
Install Jenkins and its dependencies, that includes Java
```
sudo apt-get install jenkins
sudo systemctl enable jenkins #To enable to jenkins service to start at boot time
sudo systemctl status jenkins #To verify the status of jenkins service
```
### On server(s) **without** access to INTERNET

To install Jenkins on server that **does not have access to INTERNET**, download the jenkins debian package on a machine that access to the internet using the following steps:

Add Jenkins Repository Key to the system
```
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
```
Append the Debian package repository address to the server's sources.list

```
echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
```
Run update so that apt-get will use the new repository
```
sudo apt-get update
```

Download the debian package and copy the .deb file to server where you wish to install

```
sudo apt-get download jenkins
scp <jenkins>.deb <user>@<DestinatationServer>:/path/to/transfer
```
Log on to DestinatationServer and run execute the following:

```
sudo dpkg -i /path/of/deb/<jenkins>.deb
sudo systemctl enable jenkins #To enable to jenkins service to start at boot time
sudo systemctl status jenkins #To verify the status of jenkins service
```

## Configuration of Jenkins to access over HTTPS protocol


Copy the PFX certificate issued by eBay to server where jenkins installed. 
Executing the following command will ask for the password of .pfx keystore and one would have set a new password for the destination jenkins.jks keystore when asked for
```
cd </to/path/of/certificate>
keytool -importkeystore -srckeystore <certificate>.pfx -srcstoretype pkcs12 -destkeystore jenkins.jks -deststoretype JKS
#First provide source PFX password.
#Second enter NEW password for jenkins.jks [destination keystore]
#Third RE-enter new password for jenkins.jks [destination keystore]
cp jenkins.jks /var/lib/jenkins/.
````

Update jenkins configuration file **/etc/default/jenkins** to include HTTPS configuration
Take backup of original file
> sudo cp /etc/default/jenkins /etc/default/jenkins_original

### Change following section**s** of the file (/etc/default/jenkins) as follows to disable HTTP
#### Change #1
FROM:
```
# port for HTTP connector (default 8080; disable with -1)
HTTP_PORT=8080
```
TO:
```
# port for HTTP connector (default 8080; disable with -1)
HTTP_PORT=-1
```

#### Change #2
FROM:
```
JENKINS_ARGS="--webroot=/var/cache/$NAME/war --httpPort=$HTTP_PORT"
```
TO:
```
JENKINS_ARGS="--webroot=/var/cache/$NAME/war --httpPort=$HTTP_PORT --httpsPort=8443 --httpsKeyStore=/var/lib/jenkins/jenkins.jks --httpsKeyStorePassword=*keystore password provided during importkeystore step*
```

Restart Jenkins Service for HTTPS changes to take effect

```
sudo systemctl daemon-reload
sudo systemctl restart jenkins
```
Now visit Jenkins Master @ https://<*server*>:8443/

