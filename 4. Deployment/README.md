# Deployment
## Application
### Create a Docker image for frontend & backend
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/d05024a1-08c5-4405-b1b0-a2af03ea5838)
```
docker build -t wilson/fe-dumbmerch-staging:latest .
```
lakukan hal yang sama pada backend dan branch yang berbeda dengan total 4 images
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/b2380cb6-8394-4e97-b1a7-40416206bbed)
```
docker images
```
### App database using *PostgreSQL*
buat docker compos dengan db postgres
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/cb1d377c-6fbd-431d-94ec-85a161a464f5)
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
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ae836e10-9495-4e69-86b9-32539019a748)
```
docker compose up -d
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/06f01acf-3fb9-4bb0-b585-04fa8992acd4)
```
docker ps -a
```
### Staging: A lightweight docker image (as small as possible)
cek branch Staging pada fe-dumbmerch
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/322a92e4-2115-45be-8022-58785e6a33c4)
```
git branch
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/accd0953-563b-46c7-8d81-0ee522ff2f16)
```
sudo nano Dockerfile
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/94cc1d3e-639e-4426-9c26-2ee180cdd42d)
```
sudo nano .dockerignore
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/8dd232ae-6d29-4cdd-a4f8-d310bede12c7)
```
docker build -t wilson/fe-dumbmerch-staging:latest .
```
lalukan hal yang sama pada be-dumbmerch
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/c974fb41-de10-40c7-9a21-7596885e7d5f)
```
sudo nano Dockerfile
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/aadefae6-6e98-44fd-aef7-269149d5771a)
```
sudo nano .dockerignore
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/5328e7c4-c355-4520-8cb5-6deab7532a3d)
```
docker build -t wilson/be-dumbmerch-staging:latest .
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4ddf70b3-4c5c-47c3-9abf-70552ed22341)
```
docker images
```
### Production: Deploy a production ready app
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/7de38c39-9fb5-4e66-bcec-311e2145c380)
```
git checkout Production
```
```
git branch
```
```
sudo nano Dockerfile
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/95e6a5f0-6d86-4035-8371-9506855d7fc6)
```
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
RUN npm install -g serve
EXPOSE 3000
CMD ["serve", "-s", "-l", "3000", "build"]
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0d6f7d27-399b-471a-9f14-7fd97fdd63a4)
```
docker build -t wilsonakbar/fe-dumbmerch-production:latest .
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/61aa290e-19f8-4d74-936a-e4e2fa6c9c20)
```
git checkout Production
```
```
git branch
```
```
sudo nano Dockerfile
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/0a1a35bc-4d4f-4709-8962-8f6f95b284c4)
```
FROM golang:1.16-alpine
RUN mkdir /app   
COPY . /app   
WORKDIR /app   
RUN go get ./ && go build && go mod download
EXPOSE 5000
CMD ["go", "run", "main.go"]
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/de91ad21-03e5-41bc-8129-d3c60773d353)
```
docker build -t wilsonakbar/be-dumbmerch-production:latest .
```
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/63202443-8dd1-4c58-8035-842df6e01092)
```
docker images
```
## CI/CD
### Using GitlabCI, create a pipeline running
buat repository fe dan be pada gitlab
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4fd2f377-ec15-48c3-af4f-4792b3d66dfe)
buat pipeline pada gitlab di masing-masing branch staging dan production
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/af60952e-7041-4213-b7ec-a01460a6ca65)
```
stages:
  - repository_pull
  - build_push
  - test
pull:
  stage: repository_pull
  before_script:
    - command -v ssh-agent >/dev/null || ( apk add --update openssh )
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - ssh-keyscan $VM_ADDRESS >> ~/.ssh/known_hosts
    - chmod 644 ~/.ssh/known_hosts
  script:
    - ssh $SSH_USER@$VM_ADDRESS "cd ~/fe-dumbmerch && git checkout Production && git pull origin Production"
build_push:
  stage: build_push
  image: docker:1.11
  services:
    - docker:dind
  script:
    - export DOCKER_HOST=tcp://docker:2375/
    - docker version
    - docker build -t wilsonakbar/fe-dumbmerch-production:latest .
    - docker login -u wilsonakbar -p $PASS_DOCKER
    - docker push wilsonakbar/fe-dumbmerch-production:latest
test:
  stage: test
  before_script:
    - command -v ssh-agent >/dev/null || ( apk add --update openssh )
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - ssh-keyscan $VM_ADDRESS >> ~/.ssh/known_hosts
    - chmod 644 ~/.ssh/known_hosts
  script:
    - ssh $SSH_USER@$VM_ADDRESS "cd ~/fe-dumbmerch && git checkout Production && docker compose up -d"
```
kemudian commite changes
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/4aa918a5-8fce-4fcd-8317-843471321363)
kemudian jika hijau seperti ini pipelines berhasil
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/ed58cd06-a7a9-41bd-8e99-f6007b636b0a)
images berhasil di build dan push ke dockerhub
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/3d30d3af-1124-4a1c-95ec-d2da7fea97b3)
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/1682bd95-7038-45ad-ba0b-2005b1c02416)
![image](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/83ebab03-cf9e-4f0d-9e44-8b8c21036636)
aplikasi berhasil dijalankan
