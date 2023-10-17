# Repository
## Personal Github accounts
### Create a repository on Github or Gitlab (Private repository access)
buat 2 repositori fe dan be pada github dan pilih private
![Screenshot_1](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a3d1fefc-9629-481e-963a-2e4185f8f6d4)
![Screenshot_2](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a59a4c16-0163-4239-b60c-f256de4acc2f)
### sambungkan appserver ke github
![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/746b92e6-0cbe-47b8-bf13-a95f19b32961)
```
git config --global user.name "wilsonakbar"
```
```
git config --global user.email "wilsonakbar2@gmail.com"
```
```
git config --list
```
buat ssh baru pada appserver
![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0566509a-f727-4a87-947c-3b461daf4448)
```
ssh-keygen
```
copy public key appserver ke ssh github
![Screenshot_9](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/545e97a2-49a6-4fec-91c5-ed782169a07f)
```
cat id_rsa.pub
```
test appserver sudah terhubung pada github
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/00cce254-c13e-481c-892f-a4c798d53125)
```
ssh git@github.com -T
```
### Upload direktori fe dan be yang sudah kita clone dari appserver ke github
![Screenshot_3](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0a6e784c-6b07-483e-8703-09ab5974d6b2)
```
cd be-dumbmerch
```
```
git init
```
```
git add .
```
```
git commit -m "test"
```
### buat 2 branches Staging & Production pada masing2 repository
![Screenshot_4](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/cf8a6fbc-701d-48d3-b419-dc31f593fd5e)
```
sudo git remote set-url origin git@github.com:wilsonakbar/be-dumbmerch.git
```
```
sudo git remote -v
```
```
git branch Staging
```
```
git push --all origin
```
[fe-dumbmerch](https://github.com/wilsonakbar/fe-dumbmerch)
![Screenshot_5](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/dbf8da4b-33c9-4523-9079-0910423a7943)
[be-dumbmerch](https://github.com/wilsonakbar/be-dumbmerch)
![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a59acdf1-9679-4285-9f57-597c77e615c0)
### Each Branch have own CI/CD
buat ci/cd dengan pilihan action pada github
![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1047d3ee-1b96-463a-97ef-347d3b6f4efc)

![Screenshot_12](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/84dee92f-a7db-4809-ac12-e771db0f3b31)
![Screenshot_13](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/9fe3b205-71c1-4873-8af9-45ca209f4687)



