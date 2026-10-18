# Spring Boot Monitoring Starter

Dashboards Grafana pour monitorer une application Spring Boot avec Prometheus et Micrometer.

## Dashboards inclus

| Dashboard | Fichier | Metrics |
|---|---|---|
| JVM | `dashboards/jvm-dashboard.json` | Heap, GC, threads, classes |
| HTTP | `dashboards/http-dashboard.json` | Request rate, latence p50/p99, erreurs 4xx/5xx |
| Database | `dashboards/database-dashboard.json` | HikariCP connections, acquire/usage time, timeouts |
| Cache | `dashboards/cache-dashboard.json` | Hit ratio, evictions, size |
| Custom (template) | `dashboards/custom-template-dashboard.json` | Template vide pour vos metrics metier |

## Pre-requis

- Spring Boot 2.x ou 3.x avec `spring-boot-starter-actuator`
- Micrometer Prometheus registry (`micrometer-registry-prometheus`)
- Prometheus qui scrape `/actuator/prometheus`
- Grafana 9+

## Configuration Spring Boot

```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  metrics:
    tags:
      application: ${spring.application.name}
```

```xml
<!-- pom.xml -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

## Import dans Grafana

1. Ouvrir Grafana > Dashboards > Import
2. Charger le fichier JSON
3. Selectionner la datasource Prometheus
4. Filtrer par application et instance via les variables en haut du dashboard

## Dashboard Custom

Le fichier `custom-template-dashboard.json` est un template. Dupliquez-le et remplacez les requetes PromQL par vos metrics metier.

Exemple : remplacer `your_custom_metric_total` par `orders_created_total` pour suivre les commandes.

## Structure des metrics

Tous les dashboards utilisent les variables Grafana `$application` et `$instance` pour filtrer. Assurez-vous que le tag `application` est configure dans Spring Boot (voir configuration ci-dessus).
