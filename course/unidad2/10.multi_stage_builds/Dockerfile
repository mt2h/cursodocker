#FROM golang:1.7.3
FROM golang:1.7.3 as builder
WORKDIR /go/src/github.com/alexellis/href-counter/
RUN go get -d -v golang.org/x/net/html  
COPY app.go .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

#CREATE IMAGE FROM OTHER PRINCIPAL IMAGE FOR RESULT TO IMAGE SMALL SIZE
FROM alpine:latest  
RUN apk --no-cache add ca-certificates
WORKDIR /root/
#COPY --from=0 /go/src/github.com/alexellis/href-counter/app .
COPY --from=builder /go/src/github.com/alexellis/href-counter/app .
#copy from image dockerhub.com
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
CMD ["./app"]
