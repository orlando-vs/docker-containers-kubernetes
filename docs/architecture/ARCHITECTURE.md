# Arquitectura

## Servicios
- gateway (8080): rutear /api/** → api:8081
- api (8081): Spring Boot + MySQL
- frontend (8082): Angular 17 + Nginx
- mysql (3306 interno): base de datos
- prometheus (9090): monitoreo, scrapea api:/actuator/prometheus

## Red
- Red Docker personalizada: app-net
- Internamente, los contenedores se resuelven por nombre de servicio:
  - gateway → api:8081
  - api → mysql:3306
  - prometheus → api:8081

## Variables de entorno (archivo .env)
- MYSQL_DB, MYSQL_USER, MYSQL_PASSWORD, MYSQL_ROOT_PASSWORD
- API_PORT, SERVER_PORT
- FRONTEND_PORT, GATEWAY_PORT, PROMETHEUS_PORT
- API_BASE, GATEWAY_BASE, PROMETHEUS_SCRAPE_PATH
- DOCKER_NETWORK
- K8S_NAMESPACE

## Flujo
frontend → gateway:8080 → api:8081 → mysql:3306  
prometheus:9090 → api:8081 (/actuator/prometheus)

## Kubernetes (referencia)
- Namespace: demo
- Services: ClusterIP (api, frontend, gateway)
- Exposición: LoadBalancer/Ingress (gateway)
- Prometheus: Deployment + Service; ConfigMap con scrape job a api
