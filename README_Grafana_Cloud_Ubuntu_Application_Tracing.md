# Grafana Alloy  
Alloy is a flexible, high performance, vendor-neutral distribution of the OpenTelemetry (OTel) Collector. It’s fully compatible with the most popular open source observability standards such as OpenTelemetry (OTel) and Prometheus.

Reference Documentation:  

    https://grafana.com/docs/alloy/latest/introduction/
# 1. Install Application (Petclinic) on Ubuntu Machine: 
Reference Documentation: https://github.com/spring-projects/spring-petclinic  

Please follow the below commands to install Springboot Application

    git clone https://github.com/spring-projects/spring-petclinic.git
    cd spring-petclinic

Create the maven package

    ./mvnw package -Dmaven.test.skip=true 

Download opentelemetry java agent for Auto Instrumentation  
Reference Documentation: 

https://github.com/open-telemetry/opentelemetry-java-instrumentation 

    wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar


Set OTEL Agent Environment Variables  

    export OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=http://localhost:4318/v1/traces 

Start the Petclinic Application

    java -javaagent:./opentelemetry-javaagent.jar -Dotel.service.name=petclinic -jar target/*.jar

Check the application on port like as below:  

    http://localhost:8080

# 2. Validate the Traces on Grafana  Cloud

Go to explore and verify the traces from Tempo Datasource

# 3. Configuration of Alloy config file is as bwelow:

See file config_ubuntu_tracing.alloy