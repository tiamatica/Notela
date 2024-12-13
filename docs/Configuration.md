# Configuration
The Configuration class  `Configuration` sets up a default configuration if there are no settings in the environment variables to retrieve. To customize the settings, first retrieve the default configuration, then adjust it with your desired settings. Or put all your settings in the environment variables.
To set up the default configuration create an instance of the class:  
`cfg←⎕NEW Notela.Configuration('Prod' '0.1.0')`

## The Settings  
 
### BatchSize  

Default: `10`  
Any value ≥ 1. Send the telemetry data when it reaches this amount.

### Debug  
Default: `0`  
`0`: All tests are executed without logging errors.   
`1`: Errors are logged.

### ExportInterval    
Default: `120000` ms  
The interval for how often to send metric data.  

### OtelHttpUrl  
Default: `http://localhost:4318/`  
Environment variable: `OTEL_EXPORTER_OTLP_ENDPOINT`  
The root endpoint for telemetry data.

### OtelHttpUrlLogs  
Default: `http://localhost:4318/v1/logs`  
Environment variable: `OTEL_EXPORTER_OTLP_LOGS_ENDPOINT`  
Endpoint for the logs data.

### OtelHttpUrlMetrics  
Default: `http://localhost:4318/v1/metrics`  
Environment variable: `OTEL_EXPORTER_OTLP_METRICS_ENDPOINT`  
Endpoint for the metrics data.

### OtelHttpUrlTraces  
Default: `http://localhost:4318/v1/traces`  
Environment variable: `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT`  
Endpoint for the traces data.

### ServiceName  
Default: `⍬`  
Environment variable: `OTEL_SERVICE_NAME`  
Optional name for the telemetry instance.  

### ServiceVersion  
Default: `⍬`  
Environment variable: `OTEL_SERVICE_VERSION`  
Optional name for the telemetry instance.  

### Exporters  
Default: `⍬`  
By specifying a function name you can override sending data via http.  
Use the function `WithExporter` to achieve that.  
`cfg.WithExporter'TokenExporter'`
