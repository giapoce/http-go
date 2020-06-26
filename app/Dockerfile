FROM golang:1.12-alpine

RUN apk add --no-cache git


ENV GOPATH /c/Users/Besitzer/go/src/
RUN go get -u github.com/go-redis/redis

COPY . ./app

WORKDIR /app

EXPOSE 9090

CMD ["go","run","main.go"]
