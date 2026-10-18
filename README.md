# Spring Boot Monitoring Starter

Dashboards Grafana prêts à importer pour monitorer votre application Spring Boot avec Prometheus et Micrometer.

## Dashboards inclus

| Dashboard | Fichier | Métriques |
|---|---|---|
| JVM | `dashboards/jvm-dashboard.json` | Heap, GC, threads, classes |
| HTTP | `dashboards/http-dashboard.json` | Requêtes/s, latence p50/p99, erreurs 4xx/5xx |
| Database | `dashboards/database-dashboard.json` | Connexions HikariCP, temps d'acquisition, timeouts |
| Cache | `dashboards/cache-dashboard.json` | Hit ratio, évictions, taille |
| Custom (template) | `dashboards/custom-template-dashboard.json` | Template vide pour vos métriques métier |

## Prérequis

- Spring Boot 2.x, 3.x ou 4.x avec `spring-boot-starter-actuator`
- Micrometer Prometheus registry (voir dépendance ci-dessous selon votre version)
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

### Spring Boot 2.x / 3.x

```xml
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

### Spring Boot 4.x

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-micrometer-metrics</artifactId>
</dependency>
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

Si vous utilisez `@Timed` ou `@Counted`, remplacez `spring-boot-starter-aop` par `spring-boot-starter-aspectj`.

## Import dans Grafana

1. Ouvrir Grafana > Dashboards > Import
2. Charger le fichier JSON
3. Sélectionner votre datasource Prometheus
4. Filtrer par application et instance via les variables en haut du dashboard

## Dashboard Custom

Le fichier `custom-template-dashboard.json` est un template. Dupliquez-le et remplacez les requêtes PromQL par vos métriques métier.

Exemple : remplacer `your_custom_metric_total` par `orders_created_total` pour suivre vos commandes.

## Structure des métriques

Tous les dashboards utilisent les variables Grafana `$application` et `$instance` pour filtrer. Configurez le tag `application` dans votre Spring Boot (voir configuration ci-dessus).
