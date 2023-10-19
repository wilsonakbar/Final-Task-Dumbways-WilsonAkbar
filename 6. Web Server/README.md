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
setelah itu pada gateway 
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
![Screenshot_3](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b776abd4-7d8e-4b0b-946d-c24638047c6e)
prom.wilson.studentdumbways.my.id

![Screenshot_4](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d6900dcc-7ffb-45b5-a794-05003a60d6f3)
dash.wilson.studentdumbways.my.id

![Screenshot_5](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5ccecf9c-ba14-4b4e-96f3-4e886c511214)
nodeapp.wilson.studentdumbways.my.id

![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5c6f8551-3f99-489f-9116-03cd0b1d9184)
nodegate.wilson.studentdumbways.my.id

![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f37daac4-dcb7-4165-bdad-ae92c4a15638)
wilson.studentdumbways.my.id

![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/2ca6c5cf-7885-4d1a-9a9d-7bbb370a9938)
api.wilson.studentdumbways.my.id
### All domains are HTTPS






```
