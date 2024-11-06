# Stage 1: Build the AWS CLI from source
FROM alpine:3.18 AS builder

RUN apk update && apk add --no-cache cmake make gcc g++ musl-dev libffi-dev python3-dev
RUN apk add --no-cache python3 py3-pip curl tar gzip make gcc musl-dev libffi-dev openssl-dev

WORKDIR /aws-cli
RUN curl -L "https://github.com/aws/aws-cli/archive/refs/tags/2.13.1.tar.gz" -o awscli.tar.gz && \
    tar -xzvf awscli.tar.gz --strip-components=1

RUN pip install .

# Stage 2: Create a minimal runtime image
FROM alpine:3.18

RUN apk add --no-cache python3 py3-pip

COPY --from=builder /usr/lib/python3.11/site-packages /usr/lib/python3.11/site-packages
COPY --from=builder /usr/bin/aws /usr/bin/aws

ENTRYPOINT ["aws"]