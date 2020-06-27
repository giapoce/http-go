FROM golang

RUN mkdir /app
WORKDIR /app
COPY . /app/

ENV GO111MODULE=on
ENV PORT=9090

COPY go.mod .
COPY go.sum .

RUN go mod download 

RUN go get github.com/go-redis/redis
CMD ["go","run","main.go"]
EXPOSE ${PORT}
