# Server Management
## Server login only with SSH key
### buat file one-ssh.yml pada ansible
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8aaee3a3-c547-4b17-878d-ff2816afc1b7)
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
```
![Screenshot_16](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a94af885-22f7-4d64-8343-df5ebd670239)
```
ansible-playbook one-ssh.yml
```
## Password login disabled
### masuk ke file sshd config
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/09ce51e6-620e-4a4d-9c89-ec4a4ff61b01)
```
sudo nano /etc/ssh/sshd_config
```
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/09f94fa2-d329-429f-96b7-67c44c04e892)
```
PasswordAuthentication no
```
## Create a working **SSH config** to log into servers
### buat file config pada ssh server local
![Screenshot_5](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/569b4682-7f13-4fa7-b72e-bb27dac24750)
```
host gateway
    HostName 103.127.97.70
    User wilson

host appserver
    Hostname 103.127.97.68
    User wilson
```
lalu coba login appserver dan gateway dari local server
![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3aee4a5a-1318-4a56-8a8e-d73952f9e2cc)
## Only use **1 SSH keys** for all purpose (Repository, CI/CD etc.)
### salin file key id_rsa.pub ke authorized_keys pada local server ke appserver dan gateway
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f21a034c-ebf5-43be-854d-b4698cbc9bfd)
buat file ssh.yml pada ansible
![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c3c1acdb-de58-4bfd-9e78-bdbc0811564d)
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
![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/51d57403-5745-47c0-98a0-9cbd8414d3ac)
```
ansible-playbook ssh.yml
```
## UFW enabled with only used ports allowed
### buat file  firewall pada ansible
![Screenshot_9](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b4180e75-6a4b-41ab-9e21-2b6a3ce3cb3d)
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
    - name: enable ufw
      community.general.ufw:
        state: reloaded
        policy: allow
```
lalu jalankan firewall.yml
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/487cd1f7-22b2-4b0a-a6f4-62a40799ec95)
```
ansible-playbook firewall.yml
```
