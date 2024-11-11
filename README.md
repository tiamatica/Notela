# Notela - APL OpenTelemetry Tools

Tools for sending OpenTelemetry messages over HTTP.

Experimentation with OpenTelemetry Collector connected to a telemetry backend using Docker Compose.

## Aspire dashboard
Start dashboard with docker compose:

```
cd aspire-dashboard
docker compose up -d
```

View dashboard on [localhost:18888](https://localhost:18888)

Sample docker compose files in [aspire-dashboard/compose.yaml](./aspire-dashboard/compose.yaml)

OpenTelemetry Collector configuration is in [aspire-dashboard/otel-collector-config.yaml](./aspire-dashboard/otel-collector-config.yaml)

## Demo

Notela is a Cider project, start Dyalog and then run:

```
      ]CIDER.OpenProject path/to/projectfolder
```

Step through the `#.Notela.Demo` and watch the dashboard in the browser.

## Links to documentation and specifications

https://github.com/open-telemetry/opentelemetry-proto/blob/v1.0.0/opentelemetry/proto/trace/v1/trace.proto

https://github.com/open-telemetry/opentelemetry-proto/blob/v1.0.0/opentelemetry/proto/common/v1/common.proto

https://github.com/open-telemetry/semantic-conventions-java/blob/2be178a7fd62d1073fa9b4f0f0520772a6496e0b/build.gradle.kts#L96-L141