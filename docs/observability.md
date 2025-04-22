# Observability
The pillars of observability are:
- **Metrics**: is something happening?
- **Logs**: what is happening? 
- **Traces**: where is it happening? 

This repository implements the LGTM-stack for delivering these capabilities:
* [**L**oki](https://grafana.com/oss/loki/) for logs
* [**G**rafana](https://grafana.com) for visualisation
* [**T**empo](https://grafana.com/oss/tempo/) for traces
* [**M**imir](https://grafana.com/oss/mimir/) for metrics
* [Alloy](https://grafana.com/oss/alloy-opentelemetry-collector/) as OpenTelemetry collector

> Examine these tools by bootstrapping a local cluster as described in the main [README](../README.md). This should give you all the observability tools mentioned previously. 

## Grafana Dashboards
Grafana comes installed with a set of dashboards which display cluster metrics. To view them, click on `Dashboards` in the menu on the left. Ensure you open a port-forward first:
```
kubectl -n grafana port-forward svc/grafana-service 3000:3000

# default username/pw = admin/admin
```

## Verifying OpenTelemetry (otel) on your local env
An [OTEL pipeline](../infrastructure/alloy/overlays/local/config/otel.alloy) is configured for Alloy. This means that any application can send metrics / logs / traces to Alloy via the [OpenTelemetry](https://opentelemetry.io) standard. 
  
To verify this, [install Telemtrygen](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/cmd/telemetrygen). You can run `telemtrygen` from your localhost to simulate a client which generates traces, logs and metrics. 

```
# set-up a port-forward to Alloy
kubectl -n alloy port-forward svc/alloy 4317:4317

# generate traces
telemetrygen traces --otlp-insecure --rate 20 --duration 5s --otlp-endpoint localhost:4317

# generate logs
telemetrygen logs --otlp-insecure --rate 20 --duration 5s --otlp-endpoint localhost:4317

# generate metrics
telemetrygen metrics --otlp-insecure --rate 20 --duration 5s --otlp-endpoint localhost:4317
```

You can visualize the generated traces, logs and metrics in Grafana. First open a port-forward:
```
kubectl -n grafana port-forward svc/grafana-service 3000:3000

# default username/pw = admin/admin
```

In Grafana, go to `Explore` and 
* select `Tempo` as datasource to view traces
* select `Loki` as datasource to view logs - as label filter select `job` = `telemetrygen`
* select `Mimir` as datasource to view metrics - as label filter select `job` = `telemetrygen`

