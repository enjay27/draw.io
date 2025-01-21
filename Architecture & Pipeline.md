# Architecture

## Home Network Architecture

external IP : 221.138.xxx.xxx -> Router

### port forwarding

> forward internal IP and PORT from Router

- Synology: 80 -> 80, 443 -> 443 (for subdomain)

- Linux PC: 18000 -> 8000

  > specific service requires direct connection. ex) database

### reverse proxy sub-domain with Synology

> forward internal or external IP and PORT from synology

- [Service Name].enjay.myds.me
  - ex) minio, keycloak, backend, frontend, wordpress



## Kubernetes Service Access Flow

1. End user Call
2. External Router
3. Internal IP and Port forwarding to Kubernetes Host
4. Load Balancer (Ingress Controller)
5. Forward Pod (Cluster IP)
6. Forward Container (Port)
7. Forward Application Port



---

<div style="page-break-after: always;"></div>

## Commonly Use

### Monitoring

- Metrics Monitoring (Prometheus & Grafana)
- Log Monitoring (OpenSearch)

### Storage

- Object Storage (MinIO)
- Network Attached Storage -NAS- (Synology)

### Build & Publish

- Package Management (Nexus)
- Source Code Management (Github, Gitlab, Gitea)
- Container Image Management (Harbor)
- Gitops Pipeline (Jenkins, Gitlab-CI, ArgoCD)

## Backend

- Datasource (RDBMS, NoSQL)
- In-Memory Cache (Redis)
- Event Source (RabbitMQ, Kafka, Kurrent)
- IAM Control (Active Directory)
- Session Management & Security (Keycloak)
- Backend Application (Spring Framework, NestJS, FastAPI)

## Frontend

- Web Server with Reverse Proxy (Nginx, Traefik)

- Frontend Application (React, Vue, Next)

<div style="page-break-after: always;"></div>

# Pipeline

## Gitops Pipeline

1. commit/push

2. pull request

   - build application
   - save it as an artifact to Object Storage

   - check compatibility with defined test

3. approve & merge

   - load artifact from stage 2 and run container build

   - container push

## Deploy Pipeline (Manual)

1. select application
2. choose option (resource, location, deploy method[A/B, Rollout])
3. build script dynamically & run
4. deploy at desired location (K8s Node, Linux, Windows...)

## MLOps Pipeline

> training dataset / validation dataset (DVC)
>
> training code (Git)

1. commit/push (DVC or Git)

2. pull request

   - git clone, dvc pull

   - run experiment
   - save metrics and artifacts (MLflow)
   - write model info(metrics, metadata) at pull request comment
     - also, this experiment is viewable in MLflow-UI

3. approve & merge

   - register experiment as a new model version to MLflow

   - change model version tag from MLflow
