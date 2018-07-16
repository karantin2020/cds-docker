FROM	golang:1.10.3-alpine3.7
RUN	apk --no-cache add libsecret-dev git gcc musl-dev wget
RUN	mkdir -p /go/src/github.com/ovh; \
	cd /go/src/github.com/ovh; \
	git clone --depth=1 https://github.com/karantin2020/cds
RUN	go get -u -v github.com/ovh/cds/engine
RUN	cd /go/src/github.com/ovh/cds; \
	CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -v -o ./build/cds-engine ./engine; \
	chmod +x ./build/cds-engine
RUN	cd /go/src/github.com/ovh/cds; \
	CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -v -o ./build/worker ./engine/worker; \
	chmod +x ./build/worker
WORKDIR	/go/src/github.com/ovh/cds/

FROM 	alpine:3.8
WORKDIR	/app/
COPY	--from=0 /go/src/github.com/ovh/cds/build .
COPY	--from=0 /go/src/github.com/ovh/cds/engine/sql ./sql
RUN	apk --no-cache add ca-certificates
CMD	["/app/cds-engine"]
