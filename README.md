# Notela - OpenTelemetry 

Notela is a framework for Dyalog APL to facilitate creating and emitting telemetry data. It is delivered as a Tatin package and can be used both in applications directly or via libraries.

## How

Notela will emit telemetry to a backend via HTTP/JSON payloads. For evaluation and testing you can use [aspire-dashboard](https://aspiredashboard.com/) which is a full-stack solution for collecting and visualizing telemetry data. An example docker compose file is provided in [aspire-dashboard/compose.yaml](aspire-dashboard/compose.yaml). Launch it with:

```
docker compose --file .\aspire-dashboard\compose.yaml up -d
```

With the backend up and running, you can start emitting telemetry with Notela:

```apl
      ]TATIN.LoadPackages Notela
      cfg←Notela.NewConfiguration'MyApp' '1.0.0'  ⍝ Create a configuration space 
      Notela.Start cfg                            ⍝ Start Notela engine
      tracer←Notela.GetTracer'MyApp' '1.0.0'      ⍝ Get a tracer to create spans
      span←tracer.StartSpan'MyCalcs'              ⍝ Create a span around some work
      ⎕DL 1                                      ⍝ Do some work
      span.End                                    ⍝ Close span
      Notela.Stop                                 ⍝ Stop Notela engine
```

View the dashboard on [localhost:18888](https://localhost:18888) and check out the `Traces` tab.

For detailed examples of how to use Notela and the various configuration options, see [Usage.md](docs/Usage.md) and [Configuration.md](docs/Configuration.md).

## Develop

Notela is a Cider project. Start Dyalog and then open the project with:

```
      ]CIDER.OpenProject path/to/Notela
```

## Links

[https://opentelemetry.io/](https://opentelemetry.io/)
