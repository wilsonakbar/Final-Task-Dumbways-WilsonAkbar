# Web Server
## Local
### Create domains
buat domain pada https://dash.cloudflare.com/  
![Screenshot_1](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f2d843d7-0f3b-4d1a-825a-28c574b76b69)
nodegate.wilson.studentdumbways.my.id  
nodeapp.wilson.studentdumbways.my.id  
api.wilson.studentdumbways.my.id  
dash.wilson.studentdumbways.my.id  
prom.wilson.studentdumbways.my.id  
wilson.studentdumbways.my.id  
dengan ip gateway kita 103.127.97.70  
![Screenshot_2](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/73c31d24-b379-4b65-ad6c-5523096d33c5)  
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
![Screenshot_13](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3d366aaf-bdfa-46fd-9c0e-e875d6e9593b)
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
![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/306cbead-7d6f-4fa7-8a40-5105be2567ef)
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/aa9c8c33-6830-40f0-9389-bf7231d58774)
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
![Screenshot_16](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1b8de203-2d94-4fc6-90ac-b2b01bebb489)
```
*.studentdumbways.my.id
```
pada https://dash.cloudflare.com/ kita buat record baru isi name sesuai domain dan konten nya lalu save
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/dafecc24-e672-45a6-8aa8-d9d1337204e3)
kemudian kembali ke gateway lalu enter
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/6c54408f-d2c9-471b-a1f4-a3b1813edfae)
jalankan perintah
![Screenshot_20](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/bc258cb6-e21a-46bc-95c2-74a1fa69891a)
```
sudo certbot install --cert-name studentdumbways.my.id
```
buat semua HTTPS menjadi Yes
![Screenshot_21](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a14267b0-c775-44b1-8944-62c18832f9ae)
hasil proxy.conf akan berubah otomatis jika sudah tersertifisi
![Screenshot_22](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/7a268e10-e536-41f2-8847-6c53fa0b147f)

![Screenshot_25](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/21313e58-4681-4d7c-837f-27b49ce5f451)
https://prom.wilson.studentdumbways.my.id
![Screenshot_28](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c1b440da-7651-47c3-a3bc-6eaabad74163)
https://dash.wilson.studentdumbways.my.id
![Screenshot_26](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/707f7cf3-035e-406e-ab8f-a866e806ba2a)
https://nodeapp.wilson.studentdumbways.my.id
![Screenshot_27](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/78623aab-917c-4889-9a75-545ed62c4397)
https://nodegate.wilson.studentdumbways.my.id
![Screenshot_23](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f22cd557-4769-45ac-92d5-ae8db2632402)
https://wilson.studentdumbways.my.id
![Screenshot_29](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1c73f6b5-4202-4146-aa20-8bd02cc0b601)
https://api.wilson.studentdumbways.my.id
