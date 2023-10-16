# Provisioning
## Local
### buat server lokal multipass pada windows
![Screenshot_1](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8fc3c2a1-20a3-4400-916d-797af13c1b4a)
```
multipass launch --name wilson 20.04
```
```
multipass shell wilson
```
### install Ansible pada server lokal
https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
![Screenshot_2](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/51474922-4969-4f88-a472-79b53c6e1243)
```
ansible --version
```
### install Terraform pada server lokal
https://developer.hashicorp.com/terraform/tutorials/docker-get-started/install-cli
![Screenshot_3](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/6921c619-5bc0-48bc-be36-ccd0d3442961)
```
terraform --version
```
### Attach SSH keys & IP configuration to all VMs
ssh-keygen pada direktori ~/.ssh
![Screenshot_4](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1d0836bd-2acd-4658-96d9-95377b50e73a)
```
ssh-keygen
```
salin isi kunci pada id_rsa.pub ke vm yang akan kita gunakan
![Screenshot_5](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0c499ae6-577f-4510-824c-a8295e3f7376)
```
cat id_rsa.pub
```
![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/544f6cda-fbb4-448b-96a0-0f2d6fcaee9d)
![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/35404ef7-8a09-4e53-b2b5-a926d6e3f122)
```
sudo nano ./.ssh/authorized_keys
```
### Server Configuration using Ansible
buat direktori ansible kemudian buat file
Inventory
![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/035ecfa0-4bf1-4e8a-9d83-a148444d826c)
```
[appserver]
103.127.97.68

[gateway]
103.127.97.70

[all:vars]
ansible_user="wilson"
ansible_pythone_interpreter=/usr/bin/python3
```
ansible.cfg
![Screenshot_9](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3c08aac3-e49d-472d-b8ea-f4cf615d9553)
```
[defaults]
inventory = Inventory
private_key_file = ~/.ssh/id_rsa
host_key_checking = false
interpreter_python = auto_silent
```
### Docker Engine
buat file install-docker.yml
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5d81e0fc-02ce-470e-8f39-83639e005615)
```
---
- become: true
  gather_facts: false
  hosts: all
  tasks:
    - name: Docker Dependencies
      apt:
        update_cache: true
        name:
          - ca-certificates
          - curl
          - gnupg
          - lsb-release
    - name: Docker GPG Key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
    - name: Docker Repository
      apt_repository:
        repo: deb https://download.docker.com/linux/ubuntu focal stable
    - name: Docker Engine
      apt:
        update-cache: true
        name:
          - docker-ce
          - docker-ce-cli
          - containerd.io
          - docker-compose-plugin
    - name: Python Dependencies
      apt:
        name: python3-pip
        state: latest
        update_cache: true
      become: true
    - name: Docker SDK for Python
      pip:
        name: docker
    - name: add user docker group
      user:
        name: wilson
        groups: docker
        append: yes
```
jalankan file docker
![Screenshot_11](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4cf4874b-3bbd-4799-aff8-afd274c34196)
```
ansible-playbook install-docker.yml
```
### Node Exporter
buat file install_node_exporter.yml
![Screenshot_12](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/75c37a78-63d4-45b8-865d-9c85986be66d)
```
- name: Deploy Node Exporter with Docker
  hosts: all
  become: true
  tasks:
    - name: Pull the bitnami/node-exporter Docker image
      docker_image:
        name: bitnami/node-exporter
        source: pull

    - name: Run the Node Exporter container
      docker_container:
        name: node-exp
        image: bitnami/node-exporter
        state: started
        restart_policy: unless-stopped
        published_ports:
          - "9100:9100"
```
jalankan file node exporter
![Screenshot_13](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ed5098bb-81d6-4e54-8f2b-8dff0fd03ad8)
```
ansible-playbook install_node_exporter.yml
```
### Appserver
## git repo (dumbmerch)
buat file repo_dumbmerch.yml
![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/cd6e0f28-169b-4378-bb86-5f7f522a3a28)
```
---
- become: true
  gather_facts: false
  hosts: appserver
  tasks:
    - name: Clone Git Repository fe-dumbmerch
      git:
        repo: https://github.com/demo-dumbways/fe-dumbmerch
        dest: /home/wilson/fe-dumbmerch
    - name: Clone Git Repository be-dumbmerch
      git:
        repo: https://github.com/demo-dumbways/be-dumbmerch
        dest: /home/wilson/be-dumbmerch
```
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/120ede11-d634-4e6f-bb3a-40d7558d5305)
```
ansible-playbook repo_dumbmerch.yml
```
## Prometheus 
buat file install_prometheus.yml
![Screenshot_16](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/aaaeedf3-3aed-4e95-a732-8d260845b4f3)
```
- name: prometheus on top docker
  hosts: appserver
  become: true
  tasks:
    - name: Pull the prometheus Docker image
      docker_image:
        name: bitnami/prometheus
        source: pull

    - name: Run the prometheus container
      docker_container:
        name: prometheus
        image: bitnami/prometheus
        state: started
        restart_policy: unless-stopped
        published_ports:
          - "9090:9090"
```
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5c8986d9-a3e8-48c2-9c70-804da0d718ef)
```
ansible-playbook install_prometheus.yml
```
## Grafana
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1950112a-7437-486d-997d-50f08e06ec41)
buat file install_grafana.yml
```
- name: Deploy grafana with Docker
  hosts: appserver
  become: true
  tasks:
    - name: Pull the grafana/grafana Docker image
      docker_image:
        name: grafana/grafana
        source: pull

    - name: Run the grafana container
      docker_container:
        name: grafana
        image: grafana/grafana
        state: started
        restart_policy: unless-stopped
        published_ports:
          - "3000:3000"
```
![Screenshot_19](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/2d46b5d7-6bfd-428e-806e-321e1c31fcee)
```
ansible-playbook install_grafana.yml
```
### Gateway
## Reverse Proxy
buat file proxy.conf
![Screenshot_21](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/18d1c540-111a-4a68-bb03-51456698d2fe)
```
server {
    server_name prom.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:9090;
    }
}
server {
    server_name grafana.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:3000;
    }
}
```
## NGINX
buat file nginx.yml
![Screenshot_20](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f7a78c9c-4be1-47b0-a1a8-50b7df3e4d25)
```
---
- become: true
  gather_facts: false
  hosts: gateway
  tasks:
    - name: "Installing nginx"
      apt:
        name: nginx
        state: latest
        update_cache: yes
    - name: start nginx
      service:
        name: nginx
        state: started
    - name: copy proxy.conf
      copy:
        src: /home/ubuntu/ansible/proxy.conf
        dest: /etc/nginx/sites-enabled/
    - name: reloaded nginx
      service:
        name: nginx
        state: reloaded
```
![Screenshot_22](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d05513c2-c54d-44f5-8fed-1ed941f9025e)
```
ansible-playbook nginx.yml
```
