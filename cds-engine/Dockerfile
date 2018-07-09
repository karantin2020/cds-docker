FROM	golang:1.10.3-alpine3.7
RUN	apk --no-cache add libsecret-dev git gcc musl-dev wget
RUN	mkdir -p /go/src/github.com/ovh; \
	cd /go/src/github.com/ovh; \
	git clone --depth=1 https://github.com/ovh/cds
RUN	go get -u -v github.com/ovh/cds/...
RUN	cd /go/src/github.com/ovh/cds; \
	CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -v -o cds-engine ./engine; \
	chmod +rx cds-engine
WORKDIR	/go/src/github.com/ovh/cds/

FROM 	alpine:3.8
WORKDIR	/app/
COPY	--from=0 /go/src/github.com/ovh/cds/cds-engine .
COPY	--from=0 /go/src/github.com/ovh/cds/engine/sql ./sql
RUN	apk --no-cache add ca-certificates
CMD	["/app/cds-engine"]
