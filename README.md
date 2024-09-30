# Java spring boot app with OpenTelemetry (OTEL) and Grafana stack

To run OpenTelemetry (OTEL) collector for  local and also one to send logs to Context for a Java (Spring Boot) application.

Check [Spring Boot app/](app/) directory for more details on the demo app.

## OpenTelemetry for Java

There are different ways to collect observability data out of your java applicaion. Refer : <https://opentelemetry.io/docs/languages/java/#repositories>

We are using the <https://github.com/open-telemetry/opentelemetry-java-instrumentation> java agent method.

### Get OpenTelemetry Java Agent

First, download the OpenTelemetry Java agent:

```bash
## get latest when possible (test it first, ofcourse)

cd examples/java/spring-boot/etc/
wget -q -O ./opentelemetry-javaagent.jar https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v2.4.0/opentelemetry-javaagent.jar
```

## Build the java app

```bash
# Pre-req: you need to have `sdk`: https://sdkman.io/install. 
# Again this is upto you. If you have the correct java runtime already, feel free to move to the build part.
sdk use java 17.0.9-tem # to select correct java runtime

## Build the jar using gradle. You can use maven if you want, then the path to the jar needs to be updated in the Dockerfile
./gradlew bootJar
```

### Run Docker Compose

Next, run the Docker Compose file with the following commands:

#### For full stack on local

```bash
docker compose up --remove-orphans
```

### Endpoints for full local stack

**Loki**: [See Logs](http://localhost:3000/explore?schemaVersion=1&panes=%7B%221bi%22:%7B%22datasource%22:%22loki%22,%22queries%22:%5B%7B%22refId%22:%22A%22,%22expr%22:%22%7Bexporter%3D%5C%22OTLP%5C%22%7D%20%7C%3D%20%60%60%20%7C%20json%22,%22queryType%22:%22range%22,%22datasource%22:%7B%22type%22:%22loki%22,%22uid%22:%22loki%22%7D,%22editorMode%22:%22builder%22%7D%5D,%22range%22:%7B%22from%22:%22now-1h%22,%22to%22:%22now%22%7D%7D%7D&orgId=1)

**Tempo**: [See Traces](http://localhost:3000/explore?schemaVersion=1&panes=%7B%22xkp%22:%7B%22datasource%22:%22tempo%22,%22queries%22:%5B%7B%22refId%22:%22A%22,%22datasource%22:%7B%22type%22:%22tempo%22,%22uid%22:%22tempo%22%7D,%22queryType%22:%22traceqlSearch%22,%22limit%22:20,%22tableType%22:%22traces%22,%22filters%22:%5B%7B%22id%22:%224112063b%22,%22operator%22:%22%3D%22,%22scope%22:%22span%22%7D%5D%7D%5D,%22range%22:%7B%22from%22:%22now-1h%22,%22to%22:%22now%22%7D%7D%7D&orgId=1)

**Prometheus**: [See Metrics](http://localhost:3000/explore?schemaVersion=1&panes=%7B%22vtd%22:%7B%22datasource%22:%22prometheus%22,%22queries%22:%5B%7B%22refId%22:%22A%22,%22expr%22:%22application_ready_time_seconds%22,%22range%22:true,%22instant%22:true,%22datasource%22:%7B%22type%22:%22prometheus%22,%22uid%22:%22prometheus%22%7D,%22editorMode%22:%22builder%22,%22legendFormat%22:%22__auto%22,%22useBackend%22:false,%22disableTextWrap%22:false,%22fullMetaSearch%22:false,%22includeNullMetadata%22:true%7D%5D,%22range%22:%7B%22from%22:%22now-1h%22,%22to%22:%22now%22%7D%7D%7D&orgId=1)

---

### Cleanup

```bash
docker-compose down --remove-orphans

docker volume rm otel_grafana-storage otel_loki-storage otel_mimir-storage otel_minio-data otel_prometheus-data otel_tempo-storage
```


