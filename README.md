# 1.4

![image](https://github.com/user-attachments/assets/ffa31f41-dd1b-4598-892f-bb76ad1180ad)

deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-multitool-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-multitool
  template:
    metadata:
      labels:
        app: nginx-multitool
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
      - name: multitool
        image: wbitt/network-multitool
        ports:
        - containerPort: 8080
        env:
        - name: HTTP_PORT
          value: "8080"

```

service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-multitool-svc
spec:
  selector:
    app: nginx-multitool
  ports:
  - name: nginx-port
    port: 9001
    targetPort: 80
  - name: multitool-port
    port: 9002
    targetPort: 8080
  type: ClusterIP

```

nodeport-service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport-svc
spec:
  selector:
    app: nginx-multitool
  ports:
  - name: http
    port: 80
    targetPort: 80
    nodePort: 30080
  type: NodePort

```

![image](https://github.com/user-attachments/assets/5c63deec-b92e-4a02-a939-ce2369f63ebd)


