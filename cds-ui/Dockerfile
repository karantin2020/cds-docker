FROM	alpine:3.8
RUN	apk --no-cache add ca-certificates curl wget tar; \
	mkdir /app
RUN	cd /app; \
	LAST_RELEASE=$(curl -s https://api.github.com/repos/ovh/cds/releases | grep tag_name | head -n 1 | cut -d '"' -f 4) && \
	curl -s https://api.github.com/repos/ovh/cds/releases | grep ${LAST_RELEASE} | grep browser_download_url | grep 'ui.tar.gz' | cut -d '"' -f 4 | xargs wget
RUN	cd /app; \
	tar xzf ui.tar.gz && mv dist/* .
RUN	cd /app; \
	wget https://github.com/ovh/cds/releases/download/0.8.0/caddy-linux-amd64
RUN	cd /app; \
	mv caddy-linux-amd64 caddy; \
	chmod +rx caddy setup
WORKDIR /app

FROM	alpine:3.8
WORKDIR	/app/
COPY	--from=0 /app .
RUN	apk --no-cache add ca-certificates
CMD	["/app/setup"]
