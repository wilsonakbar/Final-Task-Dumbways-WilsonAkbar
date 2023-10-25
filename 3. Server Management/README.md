# Server Management
## Server login only with SSH key
### buat kunci baru pada server lokal
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/62ad3669-6153-465b-aee1-183df5aefbf0)
```
cd ~/.ssh
```
### buat file ssh.yml pada direktori ansible
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0600b7bd-5d86-4089-8098-ba38e000b104)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4477e468-df94-4cd5-86f3-d7afa76a84ce)
```
ansible-playbook ssh.yml
```
### Password login disabled
masuk ke file sshd config lalu edit menjadi no / tidak
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/068531bf-49f9-45b6-911b-082b26a5885f)
```
sudo nano /etc/ssh/sshd_config
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d01f6d4a-cb57-474c-8d71-e837d504ce3b)
```
PasswordAuthentication no
```
## Create a working **SSH config** to log into servers
### buat file config pada ssh server local
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a88f813c-3a04-476c-9ead-1b7b24afc13c)
```
host gateway
    HostName 103.127.97.70
    User wilson

host appserver
    Hostname 103.127.97.68
    User wilson
```
lalu coba login menggunakan SSH
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c29d54ec-5cbc-4d45-8286-4852268eff96)
```
ssh appserver
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ab69a750-b264-41f2-a79d-7fd75dacb210)
```
ssh gateway
```
## Only use **1 SSH keys** for all purpose (Repository, CI/CD etc.)
### salin file key id_rsa.pub ke authorized_keys pada local server ke appserver dan gateway
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f21a034c-ebf5-43be-854d-b4698cbc9bfd)
buat file ssh.yml pada ansible
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5ff54091-6523-4569-a6cb-6020a9179fd3)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c8b82c80-61d6-4c3e-b16e-94b0f6339351)
```
ansible-playbook ssh.yml
```
## UFW enabled with only used ports allowed
### buat file  firewall pada ansible
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/33955e56-f057-4990-b990-5cf60ead09db)
```
---
- become: true
  gather_facts: false
  hosts: all
  tasks:
    - name: enabled firewall
      apt:
        name: ufw
        update_cache: yes
        state: latest
    - name: enable ufw
      community.general.ufw:
        state: enabled
        policy: allow
    - name: rules
      community.general.ufw:
        rule: allow
        proto: tcp
        port: "{{ item }}"
      with_items:
      - 22
      - 80
      - 443
      - 3000
      - 5000
      - 3306
      - 9090
      - 9100
      - 8080
      - 5432
    - name: enable ufw
      community.general.ufw:
        state: reloaded
        policy: allow
```
lalu jalankan firewall.yml
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ccc045dc-1d01-483e-9952-31e4306b598d)
```
ansible-playbook firewall.yml
```
