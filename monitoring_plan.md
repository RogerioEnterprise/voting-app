Monitoring Plan

Tools yang digunakan:
1. Prometheus: untuk mengumpulkan dan menyimpan metrik
2. Manager: sebagai manajemen peringatan
3. Loki: agregasi log
4. Grafana: visualisasi dan dashboardAlert
5. cAdvisor: metrik untuk container (CPU, memori, disk, dan network)

Key Metrics:
1. Metrik untuk aplikasi: mencakup request rate, error rate, waktu respon (P95), vote submission, dan koneksi pada database
2. Metrik untuk infrastruktur: mencakup CPU, memori, disk I/O, dan traffic jaringan
3. Container Health: restarts, status, dan batasan resource

Konfigurasi Prometheus
```yaml
global:
scrape_interval: 15s
scrape_configs:
- job_name: 'vote-app'
static_configs: [{targets: ['vote:80']}]
- job_name: 'result-app'
static_configs: [{targets: ['result:80']}]
- job_name: 'cadvisor'
static_configs: [{targets: ['cadvisor:8080']}]
- job_name: 'postgres'
static_configs: [{targets: ['postgres-exporter:9187']}]
- job_name: 'redis'
static_configs: [{targets: ['redis-exporter:9121']}]
```

Konfigurasi Alert Rules
```yaml
groups:
- name: alerts
rules:
- alert: HighErrorRate
expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
- alert: HighLatency
expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 1
- alert: ContainerDown
expr: up == 0
- alert: HighMemoryUsage
expr: (container_memory_usage_bytes / container_spec_memory_limit_bytes) > 0.9
```

Dashboard Grafana
1. Service overview: uptime, requests, errors, user yang aktif
2. Performa: latency (P95), requests/detik, waktu query Database
3. Infrastruktur: CPU, Memori, disk, dan jaringan
4. Business: total votes, votes per opsi, peak voting times

Logging (Loki)
1. Semua log dari container dikirim ke Loki
2. Menggunakan format JSON untuk memudahkan parsing
3. Level log mulai dari Debug, Info, Warn, dan Error
