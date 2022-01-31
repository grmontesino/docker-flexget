FROM python:3.10-alpine

ENV CRYPTOGRAPHY_DONT_BUILD_RUST=1

COPY requirements.txt /tmp

RUN apk add --no-cache libstdc++ tzdata su-exec && \
	apk add --virtual builddeps --no-cache build-base libffi-dev openssl-dev && \
	pip install --upgrade --no-cache-dir -r /tmp/requirements.txt && \
	apk del builddeps && \
	mkdir /flexget

RUN adduser -S -u 110 -H flexget
USER flexget

VOLUME /flexget
WORKDIR /flexget
ENTRYPOINT ["flexget"]
CMD ["daemon", "start", "--autoreload-config"]
