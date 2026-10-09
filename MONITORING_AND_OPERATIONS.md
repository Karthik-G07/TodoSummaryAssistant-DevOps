# Monitoring and Operations

## Components
- Prometheus scrapes its own metrics, Spring Boot Actuator Prometheus metrics, and Node Exporter.
- Grafana provides dashboards using Prometheus as a data source.
- Node Exporter exposes host-level metrics.

## Validation
- Prometheus targets: `/targets`
- Prometheus rules: `/api/v1/rules`
- Backend health: `/actuator/health`
- Backend metrics: `/actuator/prometheus`

## Alerts
- BackendDown: backend target remains unavailable for 2 minutes.
- HighErrorRate: HTTP 5xx error ratio exceeds 5% for 5 minutes.
- HighJvmMemoryUsage: JVM heap usage exceeds 85% for 5 minutes.

## Routine operations
Check container status and logs, GitHub Actions results, RDS connectivity, disk usage, Prometheus targets and alert state after each deployment. Keep image tags for rollback. Back up important database data and protect all credentials.

## Limitations
Prometheus rules define alert conditions; notification delivery requires Alertmanager or another configured notification integration. Grafana dashboards should be imported and validated in the running Grafana instance.
