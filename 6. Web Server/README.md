# Web Server
## Local
### Create domains
buat domain pada https://dash.cloudflare.com/  
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4a6be065-3481-4de5-9260-eac3a0c92cfe)
nodegate.wilson.studentdumbways.my.id  
nodeapp.wilson.studentdumbways.my.id  
api.wilson.studentdumbways.my.id  
dash.wilson.studentdumbways.my.id  
prom.wilson.studentdumbways.my.id  
wilson.studentdumbways.my.id  
dengan ip gateway kita 103.127.97.70  
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/26fc16fd-68e4-424e-982a-f0792b8430e5)
kemudian buat proxy.conf pada direktori ansible untuk mengatur domain, lalu jalankan kembali nginx menggunakan ansible
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
### All domains are HTTPS
pertama install snap pada gateway
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b11307aa-b700-43ef-8a12-b5e19f7d28b6)
```
snap --version
```
```
sudo snap install core; sudo snap refresh core
```
```
snap --version
```
kemudian install certbot
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ca4bf431-be7c-4b02-b769-03d209d32cea)
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/fb89aa24-5a5a-4ab1-81e9-2b2cbfcfd9c2)
```
sudo certbot certonly \
--manual \
--preferred-challenges=dns \
--email wilsonakbar3@gmail.com \
--server https://acme-v02.api.letsencrypt.org/directory \
--agree-tos \
-d *.studentdumbways.my.id
```
masukan nama domain yang ingin di wildcard
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8420924f-6d42-4f23-b317-92fd920c9d94)
```
*.studentdumbways.my.id
```
pada https://dash.cloudflare.com/ kita buat record baru isi name sesuai domain dan konten nya lalu save
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/24eaa9a1-505c-4e22-b77b-a1c7e42aafed)
kemudian kembali ke gateway lalu enter
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/653d100a-8427-41eb-96de-e8c3de63cac4)
jalankan perintah
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/109e6b4a-15de-4c72-a0ea-eb08fbc68424)
```
sudo certbot install --cert-name studentdumbways.my.id
```
buat semua HTTPS menjadi Yes
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/58681b4e-5083-451d-975d-aff8ff636cb2)
hasil proxy.conf akan berubah otomatis jika sudah tersertifisi
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1ee42a41-3d5e-491a-a22c-a47d3440a79f)

![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3ecb34fd-4fa4-47e5-8201-0f185605f083)
https://prom.wilson.studentdumbways.my.id
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/7d651a16-c01a-443b-b5c6-cc2d4c3f0aae)
https://dash.wilson.studentdumbways.my.id
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b9f1b83d-0c95-447c-a2fb-a88417c1d83b)
https://nodeapp.wilson.studentdumbways.my.id
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d5c22721-7c7b-4a52-8c5d-e0bb95628d71)
https://nodegate.wilson.studentdumbways.my.id
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/e3386b74-127c-4be1-a142-9f0e885ba9d0)
https://wilson.studentdumbways.my.id
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b30ef179-a334-45c2-bc96-129b19a9ffa2)
https://api.wilson.studentdumbways.my.id
