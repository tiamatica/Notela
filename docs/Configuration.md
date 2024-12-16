# Configuration

The `Configuration` class is used to control the behaviour of Notela. Most of the properties can be set using environment variables, making it possible to initialise Notela without any configuration in code. To create a default configuration in code, you only need to specify the name and version of the instrumented application:

```apl
cfg←Notela.NewConfiguration 'MyApp' '1.1.0'
```
## Properties
 
### BatchSize  

Default: `100`  
Any value ≥ 1.   
Buffer up telemetry data and export only when `BatchSize` is reached.

### Debug  
Default: `0`  
`0`: All exceptions are trapped and quietly ignored (internal logging).   
`1`: Exception handling disabled to allow debugging.

### ExportInterval    
Default: `120000` ms  
The interval for how often to export metric data.  

### OtelHttpUrl  
Default: `'http://localhost:4318/'`  
Environment variable: `OTEL_EXPORTER_OTLP_ENDPOINT`  
The root endpoint for telemetry data.

### OtelHttpUrlLogs  
Default: `OtelHttpUrl,'v1/logs'`  
Environment variable: `OTEL_EXPORTER_OTLP_LOGS_ENDPOINT`  
Endpoint for the logs data. 
If specified it must be a fully qualified url.

### OtelHttpUrlMetrics  
Default: `OtelHttpUrl,'v1/metrics'`  
Environment variable: `OTEL_EXPORTER_OTLP_METRICS_ENDPOINT`  
Endpoint for the metrics data.
If specified it must be a fully qualified url.

### OtelHttpUrlTraces
Default: `OtelHttpUrl,'v1/traces'`  
Environment variable: `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT`  
Endpoint for the traces data.
If specified it must be a fully qualified url.

### ServiceName  
Default: `''`  
Environment variable: `OTEL_SERVICE_NAME`
Name for the telemetry instance.  

### ServiceVersion  
Default: `''`  
Environment variable: `OTEL_SERVICE_VERSION`  
Optional name for the telemetry instance.  

### Exporters (read only)
Default: `⍬`  
Vector of exporter hooks registered using `WithExporter`.

### Loggers (read only)
Default: `⍬`  
Vector of logger hooks registered using `WithLogger`.

## Methods

### WithExporter arg
`arg` is the name of a function to invoke when telemetry is ready to be exported. If an exporter is registered, telemetry will not be posted via HTTP. Instead the hook will be invoked monadically with the serialised telemetry data (json) as the right argument.
```apl 
cfg.WithExporter'MyTelemetryExporter'
```

### WithLogger arg
`arg` is the name of a function to invoke when internal logs are created. If a logger is registered, internal logs will not go to stdout. Instead the hook will be invoked monadically with the log record as the right argument.
```apl 
cfg.WithLogger'MyLoggerFn'
```

