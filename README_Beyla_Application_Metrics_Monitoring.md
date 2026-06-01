# Application Metrics Monitoring System  

# 1. Install Prometheus on Ubuntu Machine:    
Please refer the below page to install Prometheus on Ubuntu/Linux Box: 

    https://github.com/Bhoopesh123/Grafana_onprem/blob/main/Prometheus.yml

# 2. Install Grafana on Ubuntu Machine:  
Please refer the below page to install Prometheus on Ubuntu/Linux Box: 

    https://github.com/Bhoopesh123/Grafana_onprem/blob/main/Grafana.yml

# 3. Install Application (Petclinic) on Ubuntu Machine: 
Reference Documentation: https://github.com/spring-projects/spring-petclinic  

Please follow the below commands to install Springboot Application

    git clone https://github.com/spring-projects/spring-petclinic.git
    cd spring-petclinic

Add below dependencies in pom.xml for enabling metrics endpoint 

sudo vi pom.xml

    <!-- Spring and Spring Boot dependencies -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

    <dependency>
      <groupId>io.micrometer</groupId>
      <artifactId>micrometer-registry-prometheus</artifactId>
    </dependency>


Add the following configuration to your application.properties file:

    cd spring-petclinic/src/main/resources
    sudo vi application.properties

    management.endpoints.web.exposure.include=*
    management.metrics.export.prometheus.enabled=true

Create the maven package

    ./mvnw package -Dmaven.test.skip=true 

Start the Petclinic Application

    java -jar target/*.jar 

Check the application on port like as below:  

http://localhost:8080


# 4. Add scraping information to Prometheus:  
Paste the contents from "prometheus.yml" from this repo to /etc/prometheus/prometheus.yml  

Restart Prometheus  

    sudo systemctl restart prometheus 

# 5. Generate some traffic in petclinic application:  
http://localhost:8080 

# 6. Check the Application metrics on Grafana
To Search all of the time series data points grouping by job  

    count({__name__=~".+"}) by (job)

# 5. Server Requests count for App endpoints
  
    http_server_requests_seconds_count{job="petclinic_otel",status!="200"}