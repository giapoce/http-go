# http-go

A simple docker-compose app that runs an http server and a redis server.

The http server just increments a variable in redis then reads back its value.

Provided are: docker-compose yaml file, golang container Dockerfile and main.go

For this to work on Centos 8 I had to trust docker0 interface 
and enable ip masquerading via firewalld,issuing the follwing commands:

firewall-cmd --zone=trusted --add-interface=docker0 --permanent

firewall-cmd --zone=public --add-masquerade --permanent

firewall-cmd --reload

To build the docker image for go http app, I used the GO111MODULE functionality,
which allows to manage app dependencies quite easily.

Inside the app directory I ran the following commands to build the dependecies modules:

go mod init
go mod vendor


