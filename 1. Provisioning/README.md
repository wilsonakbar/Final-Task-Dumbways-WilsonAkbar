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
## Attach SSH keys & IP configuration to all VMs
### buat kunci baru pada server lokal
![Screenshot_4](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1d0836bd-2acd-4658-96d9-95377b50e73a)
```
cd ~/.ssh
```
### buat file ssh.yml pada direktori ansible
![Screenshot_29](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/26f88fcd-bc84-4e27-8c11-937a16c4cc4e)
```
---
- become: true
  gather_facts: false
  hosts: all
  tasks:
    - name: copy ssh key
      copy:
        src: ~/.ssh/id_rsa
        dest: /home/wilson/.ssh
    - name: copy ssh pub key
      copy:
        src: ~/.ssh/id_rsa.pub
        dest: /home/wilson/.ssh
    - name: copy auth
      copy:
        src: ~/.ssh/authorized_keys
        dest: /home/wilson/.ssh
```
jalankan ansible-playbook ssh.yml
![Screenshot_19](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/079641d7-506c-4a32-b3e0-eca3f629c6da)
```
ansible-playbook ssh.yml
```
### coba login menggunakan ssh pada vm appserver dan gateway
![Screenshot_23](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/864e9907-33ea-4bdf-8d29-c8aa97d0fd6f)
```
ssh appserver
```
![Screenshot_24](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5a44f6e8-2beb-4223-b90b-bcbae4f312eb)
```
ssh gateway
```
## Server Configuration using Ansible
buat direktori ansible kemudian buat file  
Inventory
![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1b1ce62c-6d10-4d08-910f-23fbf87fcc91)
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
![Screenshot_25](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/7141d755-7f4c-4ea7-a799-4463f75e86ae)
```
---
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
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5c8986d9-a3e8-48c2-9c70-804da0d718ef)
```
ansible-playbook install_prometheus.yml
```
## Grafana
![Screenshot_26](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3b0f994f-d5f0-47cb-ae5b-174f96c8afc6)
buat file install_grafana.yml
```
---
- become: true
  gather_facts: false
  hosts: appserver
  tasks:
    - name: modify permission
      ansible.builtin.file:
        path: ~/grafana
        state: directory
        mode: "0755"
    - name: grafana on top docker
      community.docker.docker_container:
        name: grafana
        image: grafana/grafana
        ports:
          - 8080:3000
        restart_policy: unless-stopped
        volumes:
          - ~/grafana:/var/lib/grafana
        user: root
```
![Screenshot_19](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/2d46b5d7-6bfd-428e-806e-321e1c31fcee)
```
ansible-playbook install_grafana.yml
```
### Gateway
## Reverse Proxy
buat file proxy.conf
![Screenshot_28](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c17869e3-d914-4d82-ab38-f9aec94d1ecf)
```
server {
    server_name prom.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:9090;
    }
}
server {
    server_name dash.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:8080;
    }
}
server {
    server_name nodeapp.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:9100;
    }
}
server {
    server_name nodegate.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.70:9100;
    }
}
server {
    server_name wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:3000;
    }
}
server {
    server_name api.wilson.studentdumbways.my.id;

    location / {
             proxy_pass http://103.127.97.68:5000;
    }
}
```
## NGINX
buat file nginx.yml
![Screenshot_27](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5b167ebe-04a7-4980-b4fd-31002d217984)

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
    - name: copy anible/proxy
      copy:
        src: /home/ubuntu/ansible/proxy/
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

