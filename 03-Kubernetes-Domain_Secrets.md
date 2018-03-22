# Worker Node Certificate Secret Configuration

Request a **.pfx certificate** for the domain that you want to use and use that certificate to generate **.pem , .cert & .key** files required to create a secret

```
openssl pkcs12 -in <domain.name>.pfx -nocerts -out privatekey.pem -nodes
openssl pkcs12 -in <domain.name>.pfx -nocerts -out <key_name>.key
openssl pkcs12 -in <domain.name>.pfx -clcerts -nokeys -out <cert_name>.cert
openssl rsa -in <key_name>.key -out <key_name>-unen.key
```
Create the TLS Secret using the files created
**Command:**
```
kubectl create secret tls <domain.name> --key /home/mule_mc/deployments/certs/<key_name>-unen.key --cert /home/mule_mc/deployments/certs/<cert_name>.cert
```