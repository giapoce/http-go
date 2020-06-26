FROM golang:1.12-alpine


ENV APP_NAME http-go

ENV PORT 9090

COPY . /go/src/${APP_NAME}
WORKDIR /go/src/${APP_NAME}

RUN apk add --no-cache git
RUN go get github.com/go-redis/redis


RUN go build -o ${APP_NAME}

CMD ./${APP_NAME}

EXPOSE ${PORT}
