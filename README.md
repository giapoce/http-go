# http-go

This is a simple docker-compose app that runs an http server and a redis server.

The http server just increments a variable in the redis db, then reads back its value.


For this to work on Centos 8 I had to trust docker0 interface and enable ip masquerading via firewalld
This is done with the following commands:

firewall-cmd --zone=trusted --add-interface=docker0 --permanent

firewall-cmd --zone=public --add-masquerade --permanent

firewall-cmd --reload

To build the docker image for go http app, I used the GO111MODULE functionality,
which allows to manage app dependencies quite easily.

Inside the app directory I ran the following commands to build the dependecies modules:

go mod init
go mod vendor


Then I built and started the containers with:

docker-compose build
docker-compose up -d

To test the app I ran:

curl -XGET 127.0.0.1:9090/
which gave:

key count: 10

# minikube

First, I installed both kubectl and minikube

a) kubectl:

curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl

b) minikube:
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
chmod +x minikube
install minikube /usr/local/bin/

Then I prepared both for redis and go containers a service and a deployment file

Then I built the http-go image and created service and deployment for golang and redis:

docker build -t httpgo:v1 ./app
minikube start

kubectl delete deployment redis
kubectl delete service redis
kubectl delete deployment webappgo
kubectl delete service webappgo

kubectl create -f kube8/redis/
kubectl create -f kube8/http-go/

Then I checked the outcome:

kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
http-go      LoadBalancer   10.105.3.81     <pending>     9090:32517/TCP   21m
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          78m
redis        LoadBalancer   10.105.158.72   <pending>     6379:31938/TCP   45m

To test the application I ran: minikube service http-go

|-----------|---------|-------------|------------------------|
| NAMESPACE |  NAME   | TARGET PORT |          URL           |
|-----------|---------|-------------|------------------------|
| default   | http-go |        9090 | http://10.0.2.15:32517 |
|-----------|---------|-------------|------------------------|
* Opening service default/http-go in default browser...

This gave me an IP to connect to the golang service

And finally I ran the app:

curl -XGET http://10.0.2.15:9090/

key count: 12

