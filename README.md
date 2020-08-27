# nuxt-on-docker
this repository contain nuxt steps on docker

## Step1
First of all please install docker based on your operating system.

[Docker on Windows](https://docs.docker.com/docker-for-windows/install/ "Go to docker on desktop installation guide page")

[Docker on Ubuntu](https://docs.docker.com/engine/install/ubuntu/ "Go to docker on ubuntu installation guide page")

[Docker on CentOS](https://docs.docker.com/engine/install/centos/ "Go to docker on centos installation guide page")

[Docker on MacOS](https://docs.docker.com/docker-for-mac/install/ "Go to docker on macos installation guide page")

## Step2
Start Docker service on your system and create [docker hub account](https://hub.docker.com/signup)
### note: on windows docker desktop please use powershell or gitbash

## Step3
Create Dockerfile on project root directory with following content(change ARG Project parameter with your project name)
```
FROM mrtshoot/node:14.6.0
RUN yarn add @vue/cli
RUN yarn add nuxt --global
RUN yarn add @nuxtjs/axios
RUN yarn add sass-loader --save-dev
RUN yarn add node-sass --save-dev

ARG PROJECT=YOU_PROJECT_NAME
ARG PROJECT_DIR=/var/www/${PROJECT}/
RUN mkdir -p /${PROJECT_DIR}
WORKDIR /${PROJECT_DIR}

ADD . /${PROJECT_DIR}
RUN yarn
#RUN yarn run dev
RUN yarn build

ENV HOST 0.0.0.0
EXPOSE 3000

CMD ["yarn", "start"]
```

## Step4
Create three bash file on root of project
add following content on each bash file

#### up.sh
```
#!/bin/bash
docker build -t $1:latest .
docker run -d  -p $2 --name $1 $1:latest
```
with up.sh you can build and run your project on docker and see result of your project configuration.you should run up.sh with following structure

```
./up.sh [image-name] [host_port]:3000
```
for example
```
./up.sh myimagename 3000:3000
```

becareful that container port must be unchanged.

#### down.sh
```
#!/bin/bash
docker rm -f $1
```

if you test your code and everything is ok you should down your container and run push.sh script, otherwise you should check your bug and fix it then run up.sh bash script again.you should run down.sh with following structure

```
./down.sh [image-name]
```


#### push.sh
```
#!/bin/bash
registry=$2
docker tag $1:latest $registry/$1:latest
docker push $registry/$1:latest
```
if everything is ok on up.sh you should transfer your stable image on private registry with following structure
```
./push.sh [image-name] [private-registry-url]

for example

./push.sh my-image local.docker.com
```

## Step6
ignore all three script and Dockerfile on your .gitignore file and then
git push to your origin tmaster and after everything is ok go to master.

## Author
Mrtshoot - mrtshoot@gmail.com


