# Demo notes

## Build and run in local machine

Show from VSCode the project structure

    cd code/spring-microservices-demo/

    mvn clean package -DskipTests -Denv=cloud

Open new terminal for each app

    cd code/spring-microservices-demo/spring-petclinic-config-server/
    ../mvnw spring-boot:run

    cd code/spring-microservices-demo/spring-petclinic-discovery-server/
    ../mvnw spring-boot:run

    cd code/spring-microservices-demo/spring-petclinic-api-gateway/
    ../mvnw spring-boot:run

    cd code/spring-microservices-demo/spring-petclinic-customers-service/
    ../mvnw spring-boot:run

Take note of the port number from api-gateway

## Provision an Azure Spring Apps service instance

* 3 ways to deploy a resource in Azure: Portal, CLI, Infra-as-code
* Demo from Azure Portal
* Do not create as it takes time, instead use existing demo

## Build and deploy to Azure

For this section, use CLI

    az login

    az configure --defaults group=demo spring=spring-microservices-demo

Setup config server

    az spring config-server git set -n spring-microservices-demo --uri https://github.com/azure-samples/spring-petclinic-microservices-config

Create 4 Spring Apps

    az spring app create \
    --name api-gateway \
    --runtime-version Java_17 \
    --instance-count 1 \
    --memory 2Gi \
    --assign-endpoint

    az spring app create \
    --name customers-service \
    --runtime-version Java_17 \
    --instance-count 1 \
    --memory 2Gi
    
    az spring app create \
    --name vets-service \
    --runtime-version Java_17 \
    --instance-count 1 \
    --memory 2Gi

    az spring app create \
    --name visits-service \
    --runtime-version Java_17 \
    --instance-count 1 \
    --memory 2Gi

Deploy 4 Spring Apps

    az spring app deploy \
    --name api-gateway \
    --artifact-path spring-petclinic-api-gateway/target/spring-petclinic-api-gateway-3.0.1.jar \
    --jvm-options="-Xms2048m -Xmx2048m"

    az spring app deploy \
    --name customers-service \
    --artifact-path spring-petclinic-customers-service/target/spring-petclinic-customers-service-3.0.1.jar \
    --jvm-options="-Xms2048m -Xmx2048m"

    az spring app deploy \
    --name vets-service \
    --runtime-version Java_17 \
    --jar-path spring-petclinic-vets-service/target/spring-petclinic-vets-service-3.0.1.jar \
    --jvm-options="-Xms1536m -Xmx1536m"

    az spring app deploy \
    --name visits-service \
    --runtime-version Java_17 \
    --jar-path spring-petclinic-visits-service/target/spring-petclinic-visits-service-3.0.1.jar \
    --jvm-options="-Xms1536m -Xmx1536m"

Demo from https://spring-microservices-demo-api-gateway.azuremicroservices.io

## Use logs, metrics and tracing

Demo Monitoring -> Logs

Demo Monitoring -> Metrics

Demo Monitoring -> Application Insights -> Investigate