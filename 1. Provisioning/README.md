# Provisioning
## Local
### buat server lokal multipass pada windows
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/64fde52b-f5b1-49ff-8000-e25c4c779af5)
```
multipass launch --name wilson 20.04
```
```
multipass shell wilson
```
### install Ansible pada server lokal
https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/172e3a30-45da-424f-be10-29346d0d83f8)
```
ansible --version
```
### install Terraform pada server lokal
https://developer.hashicorp.com/terraform/tutorials/docker-get-started/install-cli
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/baeb7152-c00c-4993-80c6-b0d495d7e107)
```
terraform --version
```
## Attach SSH keys & IP configuration to all VMs
### buat kunci baru pada server lokal
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/59191ebc-7749-4a9f-be7f-a3d36ddf3e41)
```
cd ~/.ssh
```
### buat file ssh.yml pada direktori ansible
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/40b9a940-833d-4460-a82d-da746d7a8d34)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/7ac26287-6998-4382-95fa-52433fc09da5)
```
ansible-playbook ssh.yml
```
### coba login menggunakan ssh pada vm appserver dan gateway
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8f9fe89d-a90c-4662-a93e-339297ff6770)
```
ssh appserver
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/16861114-8427-4ad0-a65f-e7d28bdb07ae)
```
ssh gateway
```
## Server Configuration using Ansible
buat direktori ansible kemudian buat file  
Inventory
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b5a61fce-a826-4cff-a101-d9437fd1d911)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8ed9b943-0bf1-4f01-94cd-d9c0053a3c2f)
```
[defaults]
inventory = Inventory
private_key_file = ~/.ssh/id_rsa
host_key_checking = false
interpreter_python = auto_silent
```
### Docker Engine
buat file install-docker.yml
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/55e0c4e8-d77b-4ceb-ac01-2dedbe95e3e6)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b0adadf8-f1e1-42f4-98da-0bd2e43955e4)
```
ansible-playbook install-docker.yml
```
### Node Exporter
buat file install_node_exporter.yml
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/943fdcc5-5cc5-4a6e-9f20-b77b545ecbbe)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c36e8c39-089a-4657-b81d-34693f0c1ca9)
```
ansible-playbook install_node_exporter.yml
```
### Appserver
## git repo (dumbmerch)
buat file repo_dumbmerch.yml
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/24ceebae-d15a-47da-9127-be63a658b5a4)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d748d805-d2fe-4c35-9a6a-b380dee84507)
```
ansible-playbook repo_dumbmerch.yml
```
## Prometheus 
buat file install_prometheus.yml
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/6397ec69-5b15-4562-818a-8acaac558294)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/87f8ebc5-75bd-41ca-ba5e-128804ded749)
```
ansible-playbook install_prometheus.yml
```
## Grafana
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/91132b0f-9e2a-4250-9a63-ea55ad256195)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a73ea785-02ca-456d-8d9f-bd92398e4160)
```
ansible-playbook install_grafana.yml
```
### Gateway
## Reverse Proxy
buat file proxy.conf
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/27641851-0d67-41e0-b590-90d6bc17e49a)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/72e29e02-f9f1-4017-8af7-4b947b7c6287)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f871086c-25fd-43a7-b038-3e37c661e25f)
```
ansible-playbook nginx.yml
```

