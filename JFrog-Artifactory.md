# JFrog Artifactory
* [Installation](https://github.corp.ebay.com/pponnam/k8s-documentation/blob/master/JFrog-Artifactory.md#installation)
* [HTTPS Configuration](https://github.corp.ebay.com/pponnam/k8s-documentation/blob/master/JFrog-Artifactory.md#configuring-artifactory-to-https-protocol)

## Installation
1.	Download the zip file from [JFrog Artifactory WebSite]( https://api.bintray.com/content/jfrog/artifactory/jfrog-artifactory-oss-$latest.zip;bt_package=jfrog-artifactory-oss-zip "open source JFrog Artifactory")

2.	Copy the downloaded file to the server and Unzip the downloaded file to /opt/jfrog/
```
mkdir –p /opt/jfrog
cd /opt/jfrog
unzip <path-to-file-downloaded>.zip
mv artifactory-oss-5.8.1 artifactory
```

3.	Install Artifactory Service
```
cd /opt/jfrog/artifactory/bin
./installService.sh
```

4.	Verification
command:
```
sudo systemctl status artifactory
```
output:
```
 artifactory.service - Setup Systemd script for Artifactory in Tomcat Servlet Engine
   Loaded: loaded (/lib/systemd/system/artifactory.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-01-10 16:42:38 -07; 3 weeks 0 days ago
  Process: 8154 ExecStop=/opt/jfrog/artifactory/bin/artifactoryManage.sh stop (code=exited, status=0/SUCCESS)
  Process: 8222 ExecStart=/opt/jfrog/artifactory/bin/artifactoryManage.sh start (code=exited, status=0/SUCCESS)
 Main PID: 8271 (java)
```

## Configuring Artifactory to HTTPS protocol:
1. Navigate to in-built tomcat configuration directory
```
cd /opt/jfrog/artifactory/tomcat/conf/
```
2.	Generate certificate {set a password during the generation}
```
sudo  keytool -genkey -alias tomcat -keyalg RSA -keystore <filename>.cert
```
3.	Take a back up of existing “server.xml” file in the directory
4.	Update the content of server.xml under "Connector" section as follows:
FROM:
```
<Connector port="8081" sendReasonPhrase="true"/>
```
TO:
```
<Connector
<protocol="org.apache.coyote.http11.Http11NioProtocol"
<port="8081" sendReasonPhrase="true" maxThreads="200"
<scheme="https" secure="true" SSLEnabled="true"
<keystoreFile="/opt/jfrog/artifactory/tomcat/conf/art.cert" keystorePass="<password>"
<clientAuth="false" sslProtocol="TLS"/>

```
5. Restart JFrog Artifactory Service

```
sudo systemctl stop artifactory
sudo systemctl daemon-reload artifactory
sudo systemctl start artifactory
```
For additional documentation for JFrog Artifactory can be referenced [here](https://www.jfrog.com/confluence/display/RTF/Installing+Artifactory "Installing JFrog Artifactory")
