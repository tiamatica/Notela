# Usage

Notela has been designed to be used together with the [Otel-Collector](https://github.com/open-telemetry/opentelemetry-collector) for two reasons:

1. Low latency \
   The Otel-Collector runs local to your application and can be configured to export telemetry to one or more backends using a faster protocol (grpc/protobuf).
2. HTTP/JSON \
   There is no grpc implementation available today for Dyalog APL, something many (most) telemetry backends require.

This is why the default configuration is to emit data to localhost. If you want to emit telemetry from your application, plan to run an instance of the Otel-Collector on the same machine or at least in close proximity (same network, or a side-car in a container environment).

## Setup

To start using Notela in your application you simply install the package and add a couple of lines to initialise the engine with your configuration. To use the default configuration it is enough to provide the name and version of your application, e.g.

```apl
      cfg←Notela.NewConfiguration'MyApp' '1.0.0'  ⍝ Create a configuration space 
      Notela.Start cfg                            ⍝ Start Notela engine
```

Depending on what type of telemetry you plan to emit (traces, logs, metrics) you then request a `Tracer`, `Logger` and/or a `Meter` instance. These instances can be kept as global instances used throughout your application (more efficient) or requested as and when needed.

```apl
      tracer ← Notela.GetTracer''  ⍝ Get a tracer to create spans
      logger ← Notela.GetLogger''  ⍝ Get a logger to create logs
      meter  ← Notela.GetMeter''   ⍝ Get a meter to create metrics
```

## Teardown

To gracefully shut down Notela and flush the buffers of pending telemetry data, you call the `Stop` method. Make usre to add this to the end of your application when shutting down.

```apl
      Notela.Stop                                 ⍝ Stop Notela engine
```

## Telemetry

There are three different types of telemetry that you can emit:

1. traces
2. logs
3. metrics

Each type has its own controller described below.

### Tracer

The `Tracer` is used to create spans around units of work. A span has a name and optionally attributes and other properties, but in its simplest form it is a named unit of work that has a start and end time. 

```apl
      tracer ← Notela.GetTracer''  ⍝ Get a tracer to create spans
      span ← tracer.StartSpan 'Create order'
      ⍝ some code that generates an order
      span.End
```

To create a hiearchical structure of spans you can instead create active spans. An active span groups all spans created within it as child spans, making it possible to trace and measure sub-units of work.

```apl
      tracer ← Notela.GetTracer''  ⍝ Get a tracer to create spans
      span ← tracer.StartActiveSpan 'Create order'
      subspan ← tracer.StartSpan 'Read DB'
      ⍝ read record from database
      subspan.End
      subspan ← tracer.StartSpan 'Create Order'
      ⍝ compose an order record
      subspan.End
      subspan ← tracer.StartSpan 'Write DB'
      ⍝ compose an order record
      subspan.End
      span.End
```

Refer to the [Tracer](Tracer.md) documentation for all features available.

### Logger

The `Logger` is used to create structured log entries. The logger is context aware and will link the log entry to the currently active span. This makes it possible to relate log entries to a specific unit of work.

```apl
      logger ← Notela.GetLogger''  ⍝ Get a logger to create logs
      logger.Log 'Initialising library'
```

Refer to the [Logger](Logger.md) documentation for all features available.

### Meter

The `Meter` is used to create instruments for measuring. This could be used to measure resources or activities in the process.

```apl
      meter ← Notela.GetMeter''  ⍝ Get a meter to create metrics
      gauge ← meter.CreateGauge'workspace.used' 'WS used'
      gauge.Record 2000⌶1
```
