# Repository
## Create a repository on Github or Gitlab (Private repository access)
### buat 2 repositori fe dan be pada github dan pilih private
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c146e8f6-0a64-4e0d-b727-6b78ee5c5fab)
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0ac1febe-5f16-444b-9e82-81491d42282a)
### sambungkan appserver ke github
login github pada ubuntu appserver
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/52386e48-cd7f-4890-b747-1bd1180a50cb)
set username
```
git config --global user.name "wilsonakbar"
```
set email
```
git config --global user.email "wilsonakbar2@gmail.com"
```
melihat akun github yang sudah tersambung
```
git config --list
```
copy public key appserver ke ssh github
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/295bec4d-d892-44ae-84a2-32680f82ca1b)
```
cat id_rsa.pub
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/10b68dbf-20e3-465c-a66f-4f0a0fb26821)
test appserver sudah terhubung pada github
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/45131cf2-6d13-4bd1-81aa-0034a652a5a5)
```
ssh git@github.com -T
```
### Upload direktori fe dan be yang sudah kita clone dari appserver ke github
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f56bcee7-d1a2-43a7-b06a-23dd4344715e)
```
git branch
```
melihat git local berada di branch mana
```
git checkout production
```
mengganti posisi branch pada lokal git
```
git fetch
```
menyingkron repositori github ke direktori git lokal
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/797be4c6-9a6f-433f-93c6-73a4f317c804)
```
git remote set-url origin git@github.com:wilsonakbar/fe-dumbmerch.git
```
menghubungkan git lokal ke url 
```
git remote -v
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/95eb82ca-4bfe-4da9-bac2-c0db23d8606a)
```
git init
```
menjadikan direktori sebagai git lokal 
```
git add .
```
menambahkan file yg ingin di upload
```
git commit -m "test"
```
menambahkan komit pada setiap file yang di upload
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/aa938a55-53c3-4507-bba8-c237f6261d96)
```
git push origin production
```
melakukan push dari git lokal ke repositori github
lakukan hal yang sama pada setiap branch Staging dan Production untuk file fe-dumbmerch dan be-dumbmerch
### buat 2 branches Staging & Production pada masing2 repository
[fe-dumbmerch](https://github.com/wilsonakbar/fe-dumbmerch)
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d76f129d-0759-4a94-b113-3b018840831c)
[be-dumbmerch](https://github.com/wilsonakbar/be-dumbmerch)
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/41eae384-5c4f-4798-9e28-6cebcac77304)
### Each Branch have own CI/CD
CI/CD saya menggunakan gitlab dan akan saya jelaskan pada [Deployment](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/blob/47416b714f87baa32c8777112c30e9594f6221b0/4.%20Deployment/README.md#deployment)


