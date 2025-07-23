# Cloud Native Observability

- Telemetry
  - Known as the 3 pillars of observability
    - Logs
      - Available in a variety of formats
      - Verbosity levels can typically be adjusted
      - Usually file based, but streaming options exist
      - Can sometimes be manipulated through Syslog
    - Metrics
      - Emitted over a period of time
      - Memory Metrics - Amount of memory used
      - Typcially time based and are measured at set intervals
      - Useful for reviewing whether a componenet is working as expected - or underperforming/overperforming
    - Traces
      - Essential component in Cloud Native Observability
      - Trace/Track progerss of a request as it traverses through the system
      - Fantastic insights
      - End to End lifecycle

- `kubectl top`
  - Shows current resource utilization - CPU, Memory usage for pods and nodes

  ## Promethesus and Grafana

- Prometheus

  - Multidimensional data model with time series data, identifiable by metric name and key/value pairs
  - PromQL - Flexible query language
  - No dependancies on distributed storage, single nodes are autonomous
  - Data collection via push/pull method
  - Alerting

- Grafana

  - Visualization - support for graphs, charts and alerts
  - Data sources integration - support for InfluxDB, Elasticsearch
  - Observability and Monitoring - Realtime performance analysis and troubeshooting
  - Alerting - Slack and PagerDuty
  - Customisability and exensibility

- Personas
  - Site Reliablity Engineer
    - SLA - Service Level Agreement
    - SLO - Service Level Objectives
    - SLI - Service Level Indicators

- Essential Components
  - kube-prom-operator
    - Kubernetes operator that simplfies the configuration of promethesus based monitoring
    - Promethesus running on port 9090
  - Alert Manager
    - StatefulSet handles alerts
  - Node Exporter
    - Provides hardware and OS metrics exposed by the kernal
  - kubestate-metrics
    - Gathers metrics about the state of objects via th Kubernetes API
  - Prometheus Adapter
    - Converts data from Kubernetes to Promethesus metrics
  - Grafana
    - Observability stack

## Cost Management

- Cloud Native FinOps

- Make effective use of cloud resources and cloud native approaches to minimize costs
- Potential opportunities to utilize per second billing
- Dynamic compute approaches
- Cloud native applications may potentially run accross different cloud providers, including hybrid and private
- Hybrid and private may have reglatory requirements
- KubeCost
  - Monitors Kubernetes costs
  

