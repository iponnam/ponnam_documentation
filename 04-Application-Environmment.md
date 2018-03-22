# Application Environment: Creation and Deployment

## Deployment of State-ful pod with application container

Here we create pod(s) based on # of replicas required and mount the volumes from the container to the hostPath for easier access of logs/files that support or development team need on frequent basis

**Command:**
```
echo "
apiVersion: apps/v1beta2
kind: StatefulSet
metadata:
  name: <appName>
  namespace: <namespace>
spec:
  selector:
    matchLabels:
      app: <appName>
  serviceName: <appName>
  replicas: <# of pods> 
  template:
    metadata:
      labels:
        app: <appName>
    spec:
      terminationGracePeriodSeconds: 5
      containers:
      - name: <appName>
        image: <Image_Info>
        ports:
        - containerPort: <portNum>
        volumeMounts:
        - mountPath: /Path/on/Container/<vol-01>
          name: <vol-01>
        - mountPath: /Path/on/Container/<vol-02>
          name: <vol-02>
      volumes:
      - name: <vol-01>
        hostPath:
         path: /Path/on/Host/<vol-01>/
         type: DirectoryOrCreate
      - name: <vol-02>
        hostPath:
         path: /Path/on/Host/<vol-02>/
         type: DirectoryOrCreate
" | kubectl apply -f -
```

## Kubernetes Service

 A Sevice will be created to route the traffic to the container based on the selector

```
echo "
apiVersion: v1
kind: Service
metadata:
  name: <project>
  namespace: <nameSpace>
spec:
  type: NodePort
  ports:
  - port: <container portNum>
    targetPort: <portNum>
    protocol: TCP
  selector:
    app: <project>
" | kubectl apply -f -
```

## Nginx Ingress Mapping

Nginx Ingress Controller Mapping to route traffic based on the URI of the application based on the service defined in **Rules Section** below:

**Command:**

```
echo "
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <project>
  namespace: <namespace>
  annotations:
    nginx.ingress.kubernetes.io/secure-backends: \"true\"
spec:
  tls:
  - hosts:
    - <domain.name>
    secretName: <Secret_Name>
  rules:
  - host: <domain.name>
    http:
      paths:
      - backend:
          serviceName: <project>
          servicePort: <Service Port #>
        path: <uri>
" | kubectl apply -f -

``