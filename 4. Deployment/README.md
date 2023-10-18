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
![Screenshot_8](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/eb941996-cc1a-4c74-95f4-58d4cfd8fe71)
![Screenshot_9](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8569bc78-0b56-4872-aed5-fbaa07315d43)
jangan lupa untuk upload direktori yang sudah kita ubah push ke github
![Screenshot_10](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/fe872e95-5af9-4dd6-99bf-401705653005)
![Screenshot_11](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/035bc67c-981f-4558-bd34-bba7f2c84cec)
### Production: Deploy a production ready app
![Screenshot_12](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d8739a8f-18e2-4824-bf46-a5a4e778e46d)
```
git checkout Production
```
```
git branch
```
```
sudo nano Dockerfile
```
![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b004c9cf-2860-420a-a0cb-19b989052547)
```
FROM node:16
WORKDIR /app
COPY . .
RUN npm install -g serve
RUN npm run build
EXPOSE 3000
CMD ["serve", "-s", "build"]
```
![Screenshot_16](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/2a587a0d-8a86-4a6e-966f-f11003b1bcbe)
```
docker build -t wilsonakbar/fe-dumbmerch-production:latest .
```
![Screenshot_13](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/70b0b975-1b1b-4b11-9ae6-3bc800ee0b72)
```
git checkout Production
```
```
git branch
```
```
sudo nano Dockerfile
```
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/294ae39d-ec3a-465a-9256-af709d3767fc)
```
FROM node:16-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npx","serve","build","-l","3000"]
```
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/071fed80-2508-43eb-9ae4-f7e6f0859def)
```
docker build -t wilsonakbar/be-dumbmerch-production:latest .
```
![Screenshot_18](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/edc6bd1d-4a34-4fce-9882-51c10df0fd1a)
![Screenshot_19](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5e6d71a2-e6f1-4538-8d60-0012c1441313)
jangan lupa untuk upload direktori yang sudah kita ubah push ke github
![Screenshot_20](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d9dbedd4-18ab-4e39-a0b1-4eb9a0ba8af9)
![Screenshot_21](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1fa574d6-95b6-4d86-b894-5c95a8c160e3)
### Create a Docker image for frontend & backend
![Screenshot_24](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/70c1a7ca-aba9-4dc0-b391-2a27a14a9fae)
```
docker build -t wilson/fe-dumbmerch-staging:latest .
```
![Screenshot_25](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/f408954b-5871-4f3d-96d1-6de6a7e0bb7c)
```
docker images
```
### App database using *PostgreSQL*
buat docker compos dengan db postgres
![Screenshot_28](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/066137e6-72e4-4b9f-bac7-f679e6ccf447)
```
version: "3.8"
services:
   db:
    image: postgres:latest
    container_name: db_dumbmerch
    ports:
      - 5432:5432
    volumes:
      - ~/postgresql:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=wilson
      - POSTGRES_PASSWORD=wilson
      - POSTGRES_DB=db_dumbmerch
   frontend:
    image: wilsonakbar/fe-dumbmerch-production:latest
    container_name: fe-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 3000:3000
   backend:
    depends_on:
      - db
    image: wilsonakbar/be-dumbmerch-production:latest
    container_name: be-dumbmerch
    stdin_open: true
    restart: unless-stopped
    ports:
      - 5000:5000
```
![Screenshot_26](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/e72c56e5-cdd5-42a1-ac39-b54c79f42f78)
```
docker compose up -d
```
![Screenshot_27](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/532037c8-0d64-4a0c-a112-7e4e167202f0)
```
docker ps -a
```
