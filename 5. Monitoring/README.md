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

![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/75b81c38-8040-4b12-a3ad-ecc7330f6a37)


