FROM golang:1.19-alpine
WORKDIR /go/src/chat
COPY . .
RUN go mod download
RUN go build -o /chat
CMD [ "/chat" ]
EXPOSE 8080

#
## This is for documentation purposes only.
## To actually open the port, runtime parameters
## must be supplied to the docker command.
#

# (Optional) environment variable that our dockerised
# application can make use of. The value of environment
# variables can also be set via parameters supplied
# to the docker command on the command line.
#ENV HTTP_PORT=8081

# Run
#CMD [ "/chat" ]


##build stage
#
#FROM golang:alpine AS builder
#RUN apk add --no-cache git
##WORKDIR /go/src/chat
##COPY . .
#RUN pwd
#RUN go get -d -v ./...
##RUN go build -o /go/bin/app -v ./...
#
##final stage
##FROM alpine:latest
##RUN apk --no-cache add ca-certificates
##COPY --from=builder /go/bin/app /app
##ENTRYPOINT /app
##LABEL Name=getdesk Version=0.0.1
##EXPOSE 8080
