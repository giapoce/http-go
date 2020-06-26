FROM golang:1.12-alpine


ENV APP_NAME http-go

ENV PORT 9090

COPY . /go/src/${APP_NAME}
WORKDIR /go/src/${APP_NAME}

RUN apk add --no-cache git
RUN go get github.com/go-redis/redis
RUN go get golang.org/x/net/context


CMD ["go","run","main.go"]

EXPOSE ${PORT}
