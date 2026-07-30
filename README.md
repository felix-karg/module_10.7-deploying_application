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
