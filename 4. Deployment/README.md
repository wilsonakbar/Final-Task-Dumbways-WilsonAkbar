# Deployment
## Application
### Staging: A lightweight docker image (as small as possible)
cek branch Staging pada fe-dumbmerch
![Screenshot_1](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/cdc23162-2488-4dd9-96a8-9a4e3a2a0dbe)
```
git branch
```
![Screenshot_2](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/dbc1dde9-ddb9-417b-a1e5-8e91ee12c530)
```
sudo nano Dockerfile
```
![Screenshot_3](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/cc821969-72ad-4d9e-8ecf-6bceddc6627b)
```
sudo nano .dockerignore
```
![Screenshot_4](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/08083c60-165e-465c-848d-828efef41c91)
```
docker build -t wilson/fe-dumbmerch-staging:latest .
```
lalukan hal yang sama pada be-dumbmerch
![Screenshot_5](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3080c6d8-6855-4538-878b-1ed5853a99fa)
```
sudo nano Dockerfile
```
![Screenshot_6](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/a1565d73-693f-41e5-a2fb-f01c97f33bfe)
```
sudo nano .dockerignore
```
![Screenshot_7](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f2b4319d-4207-4c66-a064-07c50c079ca5)
```
docker build -t wilson/be-dumbmerch-staging:latest .
```
