# Grafana Faro  

# 1. Install Web Based Application on Ubuntu Server.   
Install it by following documentation.  
https://github.com/curioustushar/react-sample-projects 

    git clone https://github.com/curioustushar/react-sample-projects 
    sudo apt-get update
    sudo apt install npm

    npm i @grafana/faro-react
    npm i @grafana/faro-web-sdk
    cd  demo_cart
    npm install
    npm start

# 2. Configure the Application for sending Web Vitals to Grafana Cloud.  
Add the below snippet in index.js under /src folder

    import { getWebInstrumentations, initializeFaro } from '@grafana/faro-web-sdk';
    import { TracingInstrumentation } from '@grafana/faro-web-tracing';

    initializeFaro({
    url: 'http://localhost:12346/collect',
    app: {
        name: 'frontend_app',
        version: '1.0.0',
        environment: 'production'
    },

    instrumentations: [
        // Mandatory, omits default instrumentations otherwise.
        ...getWebInstrumentations(),

        // Tracing package to get end-to-end visibility for HTTP requests.
        new TracingInstrumentation(),
    ],
    });  

Restart the application  

    cd demo_cart
    npm start

# 3. Bring up the Grafana, Loki Tempo and Alloy Setup in Local.
    
    git clone https://github.com/grafana/quickpizza.git
    cd quickpizza

Please make some changes in the alloy config as below:

Paste the contents of faro_config.alloy from this repo to the end of alloy config of quickpizza.
    deployments\docker-compose\grafana-local-stack\config.alloy

Also add one more listening port(12346) to alloy docker container in the below path.

    C:\Users\Public\github\quickpizza\compose.grafana-local-stack.monolithic.yaml
    compose.grafana-local-stack.monolithic.yaml

    ports:
      - "12346:12346"
  

The compose.grafana-local-stack.monolithic.yaml file is set up to run and orchestrate the QuickPizza, Grafana, Tempo, Loki, Prometheus, Pyroscope, and Grafana Alloy containers.

Grafana Alloy collects traces, metrics, logs and profiling data from the QuickPizza app, forwarding them to the Tempo, Prometheus and Loki. Finally, you can visualize and correlate data stored in these containers with the locally running Grafana instance.

To start the local environment with the complete observability stack, use the following command:

    docker compose -f compose.grafana-local-stack.monolithic.yaml up -d 

This setup runs QuickPizza in monolithic mode, where all QuickPizza components run in a single instance.

Like before, QuickPizza is available at localhost:3333. It's time to discover some fancy pizzas!
Then, you can visit the Grafana instance running at localhost:3000 and use Explore or Drilldown apps to access QuickPizza data.



# 4. Verify the Web Vitals and RUM on Grafana Cloud  

Import the below dashboard in your Grafana for better visualization  

    https://grafana.com/grafana/dashboards/17766-frontend-monitoring/