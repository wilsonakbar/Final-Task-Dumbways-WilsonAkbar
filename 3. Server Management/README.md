# Server Management
## Server login only with SSH key
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
### Password login disabled
masuk ke file sshd config lalu edit menjadi no / tidak
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
lalu coba login menggunakan SSH
![Screenshot_23](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/864e9907-33ea-4bdf-8d29-c8aa97d0fd6f)
```
ssh appserver
```
![Screenshot_24](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5a44f6e8-2beb-4223-b90b-bcbae4f312eb)
```
ssh gateway
```
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
![Screenshot_11](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/9214542f-f0ed-46f0-ae75-7e931a87a75c)
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
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/487cd1f7-22b2-4b0a-a6f4-62a40799ec95)
```
ansible-playbook firewall.yml
```
