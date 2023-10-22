# Monitoring
## Monitor resources for *Appserver & Gateway*
### tampilan node exporter data matrix
server appserver https://nodeapp.wilson.studentdumbways.my.id/metrics
![Screenshot_1](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4da4eef3-9f0d-4dc0-b721-5c1676cdf738)
server gateway https://nodegate.wilson.studentdumbways.my.id/metrics
![Screenshot_2](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/45f70bc6-da0f-464f-8d28-54a216101c2e)
### buat file prometheus.yml pada ansible pada lokal server
![Screenshot_4](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/20a21bb5-3781-43de-9f0a-a019b4a5bea4)
```
scrape_configs:
  - job_name: grafana
    scrape_interval: 5s
    static_configs:
      - targets: ["103.127.97.68:9100", "103.127.97.70:9100"]
```
### buat file install_prometheus.yml pada ansible pada lokal server
kemudian jalankan prometheus dengan ansible ontop docker
![Screenshot_3](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/001483a8-937d-488e-a8ca-540fbbbe6bad)
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
![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d54ee869-0681-4e83-bd3d-d65269c150e0)
103.127.97.68 untuk ip app server
103.127.97.70 untuk ip gateway
## Create a fully working dashboard in Grafana
https://dash.wilson.studentdumbways.my.id/
![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/69a63f72-02fc-41bf-89e3-2a16fd183eba)
### pada bagian ini kita buat connection antara grafana dengan data sources prometheus
![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f0cb2164-d3cc-4b8c-9c13-8b6b8dba9ae0)
kemudian masukan alamat prometheus server URL
![Screenshot_9](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/53085470-6aa3-4284-9428-9336b1332000)
pada bagian scrape interval kita buat 5s kemudian save
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f9b4c4a1-9b35-4b90-a24c-6dd0bb2937fa)
### Template Dashboard
lanjut pada bagian dashboard kita upload template file json
![Screenshot_11](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8df4fb22-1181-42ff-b992-fd98f2d56424)
https://grafana.com/grafana/dashboards/ nah sini kita bisa download template sesuai yang kita inginkan dan download menjadi file json
![Screenshot_12](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/da46000c-4b75-4104-8d07-342f3aacd15b)
kemudian hubungkan template tadi dengan data sources prometheus yang sudah buat
![Screenshot_13](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b1409319-84a3-4c36-a12a-04b31a164236)
dan ini adalah tampilan grafana Dashboard yang sudah di setting menggunakan Template
![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/75b81c38-8040-4b12-a3ad-ecc7330f6a37)
## Grafana Alert/Prometheus Alertmanager
pada bagian menu alerting kita setting contact point
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/88b468a5-ae81-447a-9f26-57d3f3cf1fa6)
kemudian pada BOT API token dari untuk membuat bot dan akan mendapatkan token https://t.me/BotFather
![Screenshot_16](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/38dec0d4-c3a2-4256-9c47-5655d620d47c)
start Chat ID didapat dari https://t.me/get_id_bot
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/00267c5a-953b-443a-941a-3693a66562b1)
kemudian kita masukan data token dan id chat kita dan integration pada telegram
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/586617f1-bf4c-49e0-8341-69235271ddbd)
kemudian kita test alert nya, sebelum itu jangan lupa kita jalankan bot yang baru saja kita buat https://t.me/finaltestwilsonbot
![Screenshot_19](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3b47be4e-4949-4e18-8ee6-e93f53bc7492)
### alart rules - CPU Usage, RAM Usage & Free Storage
pada bagian alert rules kita buat new rules
![Screenshot_20](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1dab6b3a-77fe-43d1-b232-75d741e98f26)
kita masukan nama rules, kemudian ini metrics queries nya
![Screenshot_21](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/fef1dffe-d9ea-4ca5-b4ae-14d21db8cf0b)
```
100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle", job="grafana"}[5m])) * 100)
```
![Screenshot_22](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f32f2e55-a6bc-45a5-bb0e-86e3cf8b4569)
scroll pada bagian bawah, threshold kita sesuaikan alfabet input nya, alert nya di isi pada IS ABOVE, dan jangan lupa folder serta group nya, lalukan hal yang sama saat setting ram dan storage yang berbeda hanya metrics queries nya
![Screenshot_23](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/dea60d62-5381-4258-9221-20ecb92a94f3)
```
100 * (1 - ((avg_over_time(node_memory_MemFree_bytes{}[10m]) + avg_over_time(node_memory_Cached_bytes{}[10m]) + avg_over_time(node_memory_Buffers_bytes{}[10m])) / avg_over_time(node_memory_MemTotal_bytes{}[10m])))
```
![Screenshot_24](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/bec86857-393d-471a-a2ba-66570e427f5d)
```
100 - ((node_filesystem_avail_bytes{instance="103.127.97.68:9100",job="grafana",device!~'rootfs'} * 100) / node_filesystem_size_bytes{instance="103.127.97.68:9100",job="grafana",device!~'rootfs'})
```
![Screenshot_26](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0ad0d6c7-25d8-4b1d-b1ff-8ce735f6170d)
pada notification policies, kita pilih default nya menjadi telegram kemudian update
![Screenshot_25](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0d834078-757c-4dd9-8373-c265643f82a2)
contoh alert RAM
![Screenshot_27](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/e89bce72-2c50-48a1-bc88-c6977f49fc74)
contoh alert CPU
![Screenshot_28](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f8376523-1955-4d10-a3e1-3e617d1f06e3)
contoh alert Storage
![Screenshot_29](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/23352a9c-f798-4fd5-9f7b-9c3498a9232b)















