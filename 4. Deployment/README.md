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
![Screenshot_14](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/00f9ccff-aec5-4133-b11f-814bac46d24d)
```
FROM node:16-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npx","serve","build","-l","3000"]
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
![Screenshot_15](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/e49bd2cd-2ffe-4ac2-b37b-8f00a79fa63b)
```
#distroless
FROM golang:1.18 as build

WORKDIR /go/src/app
COPY . .

RUN go mod download
RUN CGO_ENABLED=0 go build -o /go/bin/app

FROM gcr.io/distroless/static-debian11
COPY --from=build /go/bin/app /
CMD ["/app"]
```
![Screenshot_17](https://github.com/wilsonakbar/Final-Task-Dumbways-WilsonAkbar/assets/132327628/071fed80-2508-43eb-9ae4-f7e6f0859def)
```
docker build -t wilsonakbar/be-dumbmerch-production:latest .
```
