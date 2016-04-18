# Dockerfile

FROM alpine:3.3
MAINTAINER Dave Oxley <aws-s3-proxy-docker@oxley.email>

LABEL com.github.daveoxley.application="aws-s3-proxy" \
      com.github.daveoxley.usage="docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e BASIC_AUTH_USER -e BASIC_AUTH_PASS daveoxley/s3-proxy" \
      com.github.daveoxley.license="MIT"

ENV GOPATH=/go PATH=/go/bin:$PATH APP_PORT=80

RUN apk add --no-cache ca-certificates \
      && apk --no-cache add --virtual build-dependencies go git \
      && mkdir -p /go/src /go/bin \
      && chmod -R 777 /go \
      && go get -u github.com/daveoxley/aws-s3-proxy \
      && apk del --purge build-dependencies \
      && rm -rf /go/pkg /go/src

EXPOSE 80

ENTRYPOINT ["/go/bin/aws-s3-proxy"]
