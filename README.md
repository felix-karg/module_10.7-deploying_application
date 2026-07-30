# Module 10.7 of DevOps Bootcamp by [TechWorld with Nana](https://www.techworld-with-nana.com/)
Deploy MongoDB and Mongo Express into local K8s cluster

## Technologies used:
- Kubernetes
- Docker
- MongoDB
- Mongo Express

## Project Description:
- Setup local K8s cluster
- Deploy MongoDB and Mongo Express with configuration and credentials extracted into ConfigMap and Secret

## Implementation Steps:
1. Add YAML file for deployment ('mongo.yaml' in this repository)
2. Add YAML for secret ('mongo-secret.yaml' in this repository)
3. Encode username in shell with `echo -n '<your-username>' | base64`
4. Encode password in the same way
5. Copy username and password to mongo-secret.yaml
6. Create secret in k8s: `kubectl apply -f mongo-secret.yaml (must be created before deployment as deployment is referencing it)
7. Verify that secret was created with `kubectl get secret`
8. Reference secrets from mongo-secret.yaml env-block of deployment:
    ```
    env:
    - name: MONGO_INITDB_ROOT_USERNAME
        valueFrom:
        secretKeyRef:
            name: mongodb-secret
            key: mongo-root-username
    - name: MONGO_INITDB_ROOT_PASSWORD
        valueFrom:
        secretKeyRef:
            name: mongodb-secret
            key: mongo-root-password
    ```
9. Create deployment: `kubectl apply -f mongo.yaml`
10. Verify: `kubectl get deployment`
11. Use `---` as separator and add service to mongo.yaml:
    ```
    ---
    apiVersion: v1
    kind: Service
    metadata:
    name: mongodb-service
    spec:
    selector:
        app: mongodb
    ports:
    - protocol: TCP
        port: 27017
        targetPort: 27017
    ```
12. Create service: `kubectl apply -f mongo.yaml`
13. Verify: `kubectl get service`
14. Add YAML for Mongo Express deployment ('mongo-express.yaml' in this repository)
15. Add YAML for ConfigMap ('mongo-configmap.yaml' in this repository) to store environmental variable(s) there
16. Reference value from ConfigMap in env-block of deployment:
    ```
    - name: database_url
        valueFrom:
        configMapKeyRef:
            name: mongodb-configmap
            key: database_url
    ```
17. Create ConfigMap: `kubectl apply -f mongo-configmap.yaml` (again must be created before deployment as it's referenced there)
18. Verify: `kubectl get configmap`
19. Create Deployment: `kubectl apply -f mongo-express.yaml` (again must be created before deployment as it's referenced there)
20. Verify: `kubectl get pod`
21. Add Service to 'mongo-express.yaml':
    ```
    ---
    apiVersion: v1
    kind: Service
    metadata:
    name: mongo-express-service
    spec:
    selector:
        app: mongo-express
    type: LoadBalancer
    ports:
    - protocol: TCP
        port: 8081
        targetPort: 8081
        nodePort: 30000
    ```
    To make this an external service you need to add `type: LoadBalancer` and `nodePort` (the port where external service is reachable from outside)
22. Create Service: `kubectl apply -f mongo-express.yaml`
23. Verify: `kubectl get service`
24. Open browser at `localhost:8081` (port is shown in `kubectl get pod` output)
25. View logs with `kubectl logs <mongo-express-pod-name>` and retrieve credentials there
26. Login to Mongo Express UI
