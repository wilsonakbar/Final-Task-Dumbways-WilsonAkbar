# Repository
## Create a repository on Github or Gitlab (Private repository access)
### buat 2 repositori fe dan be pada github dan pilih private
![Screenshot_1](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a3d1fefc-9629-481e-963a-2e4185f8f6d4)
![Screenshot_2](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a59a4c16-0163-4239-b60c-f256de4acc2f)
### sambungkan appserver ke github
login github pada ubuntu appserver
![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/746b92e6-0cbe-47b8-bf13-a95f19b32961)
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
![Screenshot_9](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/545e97a2-49a6-4fec-91c5-ed782169a07f)
```
cat id_rsa.pub
```
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/da33c637-77c0-4c10-a1b4-4328d982ebb1)
test appserver sudah terhubung pada github
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/00cce254-c13e-481c-892f-a4c798d53125)
```
ssh git@github.com -T
```
### Upload direktori fe dan be yang sudah kita clone dari appserver ke github
![Screenshot_16](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5d11c79c-1e46-4263-a6ad-c6ca6244bc01)
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
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d6472e1d-66cf-4a3e-ab93-17d7d10c32e3)
```
git remote set-url origin git@github.com:wilsonakbar/fe-dumbmerch.git
```
menghubungkan git lokal ke url 
```
git remote -v
```
![Screenshot_3](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/76dbf85e-22b1-4aae-a646-c1671a45c84c)
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
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c2d9cc34-58c7-4602-a1f8-47408c1ed11b)
```
git push origin production
```
melakukan push dari git lokal ke repositori github
lakukan hal yang sama pada setiap branch Staging dan Production untuk file fe-dumbmerch dan be-dumbmerch
### buat 2 branches Staging & Production pada masing2 repository
[fe-dumbmerch](https://github.com/wilsonakbar/fe-dumbmerch)
![Screenshot_5](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/dbf8da4b-33c9-4523-9079-0910423a7943)
[be-dumbmerch](https://github.com/wilsonakbar/be-dumbmerch)
![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a59acdf1-9679-4285-9f57-597c77e615c0)
### Each Branch have own CI/CD
CI/CD saya menggunakan gitlab dan akan saya jelaskan pada [Deployment](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/blob/47416b714f87baa32c8777112c30e9594f6221b0/3.%20Server%20Management/README.md)


