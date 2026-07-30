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
