# Monitoring
## Monitor resources for *Appserver & Gateway*
### tampilan node exporter data matrix
server appserver https://nodeapp.wilson.studentdumbways.my.id/metrics
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/6e03e3c8-5dab-46f9-a11f-75e610a94d6f)
server gateway https://nodegate.wilson.studentdumbways.my.id/metrics
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c489cb23-9a0a-4e29-8a97-70c45d5b34c5)
### buat file prometheus.yml pada ansible pada lokal server
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4a7a45ee-e94f-40ed-bd96-5733b7b99cef)
```
scrape_configs:
  - job_name: grafana
    scrape_interval: 5s
    static_configs:
      - targets: ["103.127.97.68:9100", "103.127.97.70:9100"]
```
### buat file install_prometheus.yml pada ansible pada lokal server
kemudian jalankan prometheus dengan ansible ontop docker
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3940e440-04a4-42c0-9f32-b56307928157)
```
- become: true
  gather_facts: false
  hosts: appserver
  tasks:
    - name: copy file prometheus.yml
      copy:
        src: /home/ubuntu/ansible/prometheus.yml
        dest: /home/wilson/prometheus/

    - name: prometheus on top docker
      community.docker.docker_container:
        name: prometheus
        image: bitnami/prometheus
        ports:
          - 9090:9090
        restart_policy: unless-stopped
        volumes:
          - /home/wilson/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
```
### ini tampilan data metrix yang sudah terhubung dengan prometheus
https://prom.wilson.studentdumbways.my.id/
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/22dce90f-e79d-4588-8c7d-ffc3ce14a23c)
103.127.97.68 untuk ip app server
103.127.97.70 untuk ip gateway
## Create a fully working dashboard in Grafana
https://dash.wilson.studentdumbways.my.id/
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4b1bb13e-6444-4390-aaa2-cbce810a2085)
### pada bagian ini kita buat connection antara grafana dengan data sources prometheus
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ebeee30e-4efe-45e5-b226-e42ff8034f71)
kemudian masukan alamat prometheus server URL
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/88daebf8-443e-4b10-a59a-ee7c1e192e51)
pada bagian scrape interval kita buat 5s kemudian save
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c46c55a6-d955-4669-851b-4c34d940db99)
### Template Dashboard
lanjut pada bagian dashboard kita upload template file json
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ee9d27b0-0443-4dff-a053-d8c6d9eb2bf0)
https://grafana.com/grafana/dashboards/ nah sini kita bisa download template sesuai yang kita inginkan dan download menjadi file json
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0bc6f56b-b231-469b-acd6-48f5bbddf1ce)
kemudian hubungkan template tadi dengan data sources prometheus yang sudah buat
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/fb4aff03-4bd9-4de0-b35c-0176d072087a)
dan ini adalah tampilan grafana Dashboard yang sudah di setting menggunakan Template
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/fc91059a-2d44-47b0-a9c0-08b784d27f35)
## Grafana Alert/Prometheus Alertmanager
pada bagian menu alerting kita setting contact point
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d462c017-4281-4131-b1fe-8a944734d99e)
kemudian pada BOT API token dari untuk membuat bot dan akan mendapatkan token https://t.me/BotFather
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/06908d32-ea68-4841-b766-4d37bb6a9508)
start Chat ID didapat dari https://t.me/get_id_bot
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/2ff1d8a1-9592-4bf3-9743-0021345ac7c4)
kemudian kita masukan data token dan id chat kita dan integration pada telegram
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1222b007-2f73-49aa-b1ff-e755b5a486cb)
kemudian kita test alert nya, sebelum itu jangan lupa kita jalankan bot yang baru saja kita buat https://t.me/finaltestwilsonbot
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/df27792e-342f-49f0-83ae-ecf54123a258)
### alart rules - CPU Usage, RAM Usage & Free Storage
pada bagian alert rules kita buat new rules
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/80a98404-85dd-40be-895f-08d676046518)
kita masukan nama rules, kemudian ini metrics queries nya
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c68d5bb3-9561-45a0-80ed-ee44c574e3a9)
```
100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle", job="grafana"}[5m])) * 100)
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/37021dc0-cde1-4d93-b99d-d2b008332092)
scroll pada bagian bawah, threshold kita sesuaikan alfabet input nya, alert nya di isi pada IS ABOVE, dan jangan lupa folder serta group nya, lalukan hal yang sama saat setting ram dan storage yang berbeda hanya metrics queries nya
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/33c815c3-15de-415c-be4f-c6eb0843bf3d)
```
100 * (1 - ((avg_over_time(node_memory_MemFree_bytes{}[10m]) + avg_over_time(node_memory_Cached_bytes{}[10m]) + avg_over_time(node_memory_Buffers_bytes{}[10m])) / avg_over_time(node_memory_MemTotal_bytes{}[10m])))
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/7a4ccb8e-b460-4cb7-8fcc-d80500686f10)
```
100 - ((node_filesystem_avail_bytes{instance="103.127.97.68:9100",job="grafana",device!~'rootfs'} * 100) / node_filesystem_size_bytes{instance="103.127.97.68:9100",job="grafana",device!~'rootfs'})
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/061cbee0-0315-4ab0-b502-47c36d2a5821)
pada notification policies, kita pilih default nya menjadi telegram kemudian update
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5496e34e-4337-4e35-8866-4646bf75f4cb)
contoh alert RAM
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3ae1786a-9303-4933-8862-d974f36c4916)
contoh alert CPU
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/56710ef2-eaa9-4d8c-b652-1135e72e2c6f)
contoh alert Storage
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0dbf2547-a2a8-4fb4-9f58-74c5f0f8bcca)
